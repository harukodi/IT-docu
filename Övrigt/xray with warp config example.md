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
                "clients": [{"id": "client-id-goes-here-uuid"}],
                "decryption": "none"
            },
            "streamSettings": {
              "network": "xhttp",
              "xhttpSettings": {
                "mode": "auto",
                "path": "your-path-goes-here"
              }
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "blackhole",
            "settings": {},
            "tag": "BLOCK"
        },
        {
            "protocol": "wireguard",
            "settings": {
                "secretKey": "your-secret-key-here",
                "address": ["172.16.0.2/32"],
                "peers": [
                    {
                        "publicKey": "your-public-key-here",
                        "endpoint": "engage.cloudflareclient.com:2408",
                        "allowedIPs": ["0.0.0.0/0", "::/0"]
                    }
                ],
                "mtu": 1280
            },
            "tag": "WARP_OUTBOUND"
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
            },
            {
                "domain": ["geosite:category-ads-all"],
                "outboundTag": "block",
                "type": "field"
            },
            {
                "inboundTag": ["VLESS_INBOUND_XHTTP"],
                "outboundTag": "WARP_OUTBOUND",
                "type": "field"
            }
        ]
    }
}
```