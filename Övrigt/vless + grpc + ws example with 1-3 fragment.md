```json
{
    "log": {
        "loglevel": "debug",
        "maskAddress": "half"
    },
    "inbounds": [
        {
            "tag": "VLESS_INBOUND",
            "listen": "0.0.0.0",
            "port": 8443,
            "protocol": "vless",
            "settings": {
                "clients": [{"id": "UUID-goes-here"}],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "ws",
                "security": "none",
                "wsSettings": {
                    "path": "/your-custom-path-goes-here"
                }
            }
        },
        {
            "tag": "VLESS_INBOUND_GRPC",
            "listen": "0.0.0.0",
            "port": 8444,
            "protocol": "vless",
            "settings": {
                "clients": [{"id": "UUID-goes-here"}],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "grpc",
                "grpcSettings": {
                    "serviceName": "serviceName",
                    "multiMode": false,
                    "idle_timeout": 60,
                    "health_check_timeout": 20,
                    "initial_windows_size": 35536, 
                    "permit_without_stream": true
                }
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom",
            "settings": {
                "fragment": {
                  "packets": "1-3",
                  "length": "100-200",
                  "interval": "10-20"
                },
                "noises":[
                    {
                      "type":"rand",
                      "packet":"100-200",
                      "delay":"10-20"
                    }
                ]
            },
            "tag": "DIRECT"
        },
        {
            "protocol": "blackhole",
            "settings": {},
            "tag": "BLOCK"
        }
    ],
    "routing": {
        "rules": [
            {
                "ip": [
                    "geoip:private"
                ],
                "outboundTag": "BLOCK",
                "type": "field"
            },
            {
                "domain": [
                    "localhost"
                ],
                "outboundTag": "BLOCK",
                "type": "field"
            }
        ]
    }
}
```
## **NOTE:** in the serviceName field dont use "/" in your serviceName

## Caddyfile
```Caddyfile
subdomain.domain.tld {
      tls {
          protocols tls1.3
          ciphers TLS_AES_256_GCM_SHA384 TLS_CHACHA20_POLY1305_SHA256
          curves x25519
      }
      handle /same-path-as-in-ws-xray-config {
        @v2ray_websocket {
            path /same-path-as-in-ws-xray-config
            header Connection Upgrade
            header Upgrade websocket
        }
        reverse_proxy @v2ray_websocket 0.0.0.0:8443 {
        header_up X-Real-IP {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-Proto {http.request.scheme}
        }
      }
      handle {
        redir https://subdomain.domain.tld 302
      }
}

subdomain.domain.tld {
  tls {
      protocols tls1.3
      ciphers TLS_AES_256_GCM_SHA384 TLS_CHACHA20_POLY1305_SHA256
      curves x25519
  }
	@grpc {
		protocol grpc
		path /serviceName/*
	}
	reverse_proxy @grpc 0.0.0.0:8444 {
		flush_interval -1
		transport http {
			versions h2c
		}
        header_up X-Real-IP {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-Proto {http.request.scheme}
	}
  @nongrpc {
      not {
          protocol grpc
      }
  }
  handle @nongrpc {
      redir https://subdomain.domain.tld 302
  }
}
```

## **NOTE:** grpc cant be under the same domain. Subdomain usage is required. Also the service name must be the same as the one in the xray config
The `redir` parameter can be changed to any URL you want for obfuscation in the Caddyfile.