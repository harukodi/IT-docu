## Xray config
```json
{
    "log": {
        "loglevel": "info"
    },
    "inbounds": [
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
                    "initial_windows_size": 35536, 
                    "permit_without_stream": true
                }
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom",
            "settings": {},
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

## Caddyfile
```Caddyfile
subdomain.domain.tld {
  tls {
    protocols tls1.3 tls1.3
    ciphers TLS_AES_256_GCM_SHA384 TLS_CHACHA20_POLY1305_SHA256
    curves x25519
  }

  @xray_grpc {
    protocol grpc
    path /serviceName/*
  }

  handle @xray_grpc {
    reverse_proxy @xray_grpc 0.0.0.0:8444 {
      header_up X-Real-IP {http.request.header.CF-Connecting-IP}
      header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
      header_up X-Forwarded-Proto {http.request.scheme}
      flush_interval -1
      transport http {
        versions h2c
      }
    }
  }

  handle {
    redir https://subdomain.domain.tld 302
  }
}
```
## **NOTE:** In the `serviceName` field dont use "/" in your serviceName 
The `redir` parameter can be changed to any URL you want for obfuscation in the Caddyfile.