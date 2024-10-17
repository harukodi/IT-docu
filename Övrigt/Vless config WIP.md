```json
{
    "log": {
        "loglevel": "info"
    },
    "inbounds": [
        {
            "tag": "VLESS_INBOUND",
            "listen": "0.0.0.0",
            "port": 8443,
            "protocol": "vless",
            "settings": {
                "clients": [],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "ws",
                "security": "none",
                "wsSettings": {
                    "path": "/CJLBtRBNBHtiWXbQ3kcDiDxTYD6BbVZ8"
                }
            }
        },
        {
            "tag": "VLESS_INBOUND_GRPC",
            "listen": "0.0.0.0",
            "port": 8444,
            "protocol": "vless",
            "settings": {
                "clients": [],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "grpc",
                "grpcSettings": {
                    "serviceName": "CJLBtRBNBHtiWXbQ3kcDiDxTYD6BbVZ8"
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