## Xray config example
**NOTE** you will have to change the `path` parameter
```json
{
    "log": {
        "loglevel": "info"
    },
    "inbounds": [
        {
            "tag": "VLESS_INBOUND_WS",
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
```plaintext
subdomain.domain.tld {
  tls {
    protocols tls1.3 tls1.3
    ciphers TLS_AES_256_GCM_SHA384 TLS_CHACHA20_POLY1305_SHA256
    curves x25519
  }
  @xray_websocket {
    path /same-path-as-in-the-xray-config-file
    header Connection Upgrade
    header Upgrade websocket
  }
  handle @xray_websocket {
    reverse_proxy @xray_websocket 0.0.0.0:8443 {
      header_up X-Real-IP {http.request.header.CF-Connecting-IP}
      header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
      header_up X-Forwarded-Proto {http.request.scheme}
    }
  }
  handle {
    redir https://subdomain.domain.tld 302
  }
}
```
**NOTE:** The `redir` parameter can be changed to any URL you want for obfuscation in the Caddyfile.

