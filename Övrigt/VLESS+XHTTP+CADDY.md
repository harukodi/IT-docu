
## Xray config example
```json
{
    "log": {
        "loglevel": "error",
        "dnsLog": true
    },
    "inbounds": [
        {
            "tag": "VLESS_INBOUND_XHTTP",
            "listen": "0.0.0.0",
            "port": 8444,
            "protocol": "vless",
            "settings": {
                "clients": [{"id": "UUID-GOES-HERE"}],
                "decryption": "none"
            },
            "streamSettings": {
              "network": "xhttp",
              "xhttpSettings": {
                "mode": "auto",
                "path": "PATH-GOES-HERE"
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

## Caddyfile example
```Caddyfile
## Uncomment if you are using cloudflare as the dns provider
##{
##  acme_dns cloudflare {env.CLOUDFLARE_AUTH_TOKEN}
##}

subdomain.domain.tld {
  tls {
    protocols tls1.3 tls1.3
    ciphers TLS_AES_256_GCM_SHA384 TLS_CHACHA20_POLY1305_SHA256
    curves x25519
  }

  @xray_xhttp {
    path /PATH-GOES-HERE/*
  }

  handle @xray_xhttp {
    reverse_proxy @xray_xhttp 0.0.0.0:8444 {
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
	respond "Error: Invalid or Missing API Key" 401 {
	    close
	}
  }
}
```