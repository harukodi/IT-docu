# Create the folders for marzban xray
```bash
cd /home/$USER/; mkdir -p marzban/config/{certs,xray-core}; cd marzban/config/certs
```

# Create the fullchain.pem / key.pem files for marzban
```bash
openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out fullchain.pem -subj "/C=/ST=/L=/O=/OU=/CN="
```
# Cd to the xray folder
```bash
cd /home/$USER/marzban/config/xray-core
```

# Fetch the xray-core
**NOTE**: At the time of writing, the latest Xray version is `v1.8.13`. To view all the current and previous Xray versions, visit the [Releases · Xray-core (github.com)](https://github.com/XTLS/Xray-core/releases)
```bash
wget "https://github.com/XTLS/Xray-core/releases/download/v1.8.13/Xray-linux-64.zip"
```

# Unzip the xray-core zip file
**NOTE**: If you dont have unzip install it with `sudo apt install unzip -y`
```bash
sudo unzip Xray-linux-64.zip; sudo rm Xray-linux-64.zip
```

# Cd back to root of the marzban folder
```bash
cd /home/$USER/marzban/
```

# Touch the docker-compose.yaml file
```bash
touch docker-compose.yaml
```

# Open the docker-compoe.yaml file with nano
```bash
nano docker-compose.yaml
```

# Paste the following content in the docker-compose.yaml file
```yaml
services:
  marzban:
    image: gozargah/marzban:latest
    restart: always
    env_file: .env
    ports:
      - "8880:8880"
      - "8443:8443"
    volumes:
      - ./xray_config.json:/xray_config.json
      - ./config:/var/lib/marzban/
```

# Touch the env file
```bash
touch .env
```

# Open the env file with nano
```bash
nano .env
```

# Paste the following content to the env file
*Change the SUDO_USERNAME and the SUDO_PASSWORD to your own NAME/PASSWORD*
```env
SUDO_USERNAME = "SUDO_USERNAME"
SUDO_PASSWORD = "SUDO_PASSWORD"
UVICORN_PORT = 8880
SQLALCHEMY_DATABASE_URL = "sqlite:////var/lib/marzban/db.sqlite3"
XRAY_JSON = "/xray_config.json"
UVICORN_SSL_CERTFILE = "/var/lib/marzban/certs/fullchain.pem"
UVICORN_SSL_KEYFILE = "/var/lib/marzban/certs/key.pem"
XRAY_EXECUTABLE_PATH = "/var/lib/marzban/xray-core/xray"
```

# Touch the xray_config.json file
```bash
touch xray_config.json
```

# Open the xray_config.json file with nano
```bash
nano xray_config.json
```

# Paste the following content to the xray_config.json file
*This is just an example file, you may need to do futher changes refer to [Configurations | Project X](https://xtls.github.io/en/config/#overview)*
```json
{
    "log": {
        "loglevel": "info"
    },
    "inbounds": [
        {
            "tag": "VMESS_INBOUND",
            "listen": "0.0.0.0",
            "port": 2053,
            "protocol": "vmess",
            "settings": {
                "clients": []
            },
            "streamSettings": {
                "network": "ws",
                "wsSettings": {
                    "path": "/path"
                },
                "security": "tls",
                "tlsSettings": {
                    "serverName": "sub.example.tld",
                    "certificates": [
                        {
                            "ocspStapling": 3600,
                            "certificateFile": "/var/lib/marzban/certs/fullchain.pem",
                            "keyFile": "/var/lib/marzban/certs/key.pem"
                        }
                    ],
                    "minVersion": "1.2",
                    "cipherSuites": "TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256:TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256:TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384:TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384:TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256:TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256"
                }
            }
        },
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
                "wsSettings": {
                    "path": "/path"
                },
                "security": "tls",
                "tlsSettings": {
                    "serverName": "sub.example.tld",
                    "certificates": [
                        {
                            "ocspStapling": 3600,
                            "certificateFile": "/var/lib/marzban/certs/fullchain.pem",
                            "keyFile": "/var/lib/marzban/certs/key.pem"
                        }
                    ],
                    "minVersion": "1.2",
                    "cipherSuites": "TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256:TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256:TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384:TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384:TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256:TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256"
                }
            }
        },
        {
            "tag": "TROJAN_INBOUND",
            "listen": "0.0.0.0",
            "port": 2087,
            "protocol": "trojan",
            "settings": {
                "clients": []
            },
            "streamSettings": {
                "network": "ws",
                "wsSettings": {
                    "path": "/path"
                },
                "security": "tls",
                "tlsSettings": {
                    "serverName": "sub.example.tld",
                    "certificates": [
                        {
                            "ocspStapling": 3600,
                            "certificateFile": "/var/lib/marzban/certs/fullchain.pem",
                            "keyFile": "/var/lib/marzban/certs/key.pem"
                        }
                    ],
                    "minVersion": "1.2",
                    "cipherSuites": "TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256:TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256:TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384:TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384:TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256:TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256"
                }
            }
        },
        {
            "tag": "SHADOWSOCKS_INBOUND",
            "listen": "0.0.0.0",
            "port": 2096,
            "protocol": "shadowsocks",
            "settings": {
                "clients": [],
                "network": "tcp,udp"
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
# File structure should look like this
```
📦marzban  
 ┣ 📂config  
 ┃ ┣ 📂certs  
 ┃ ┃ ┣ 📜fullchain.pem  
 ┃ ┃ ┗ 📜key.pem  
 ┃ ┗ 📂xray-core  
 ┃ ┃ ┣ 📜LICENSE  
 ┃ ┃ ┣ 📜README.md  
 ┃ ┃ ┣ 📜geoip.dat  
 ┃ ┃ ┣ 📜geosite.dat  
 ┃ ┃ ┗ 📜xray  
 ┣ 📜docker-compose.yaml  
 ┣ 📜env  
 ┗ 📜xray_config.json
```

# Bring up the project with docker-compose command
```bash
docker-compose up -d
```
# Now you are done ;)

#### caddy + vless + ws example
[[caddy + vless + ws example]]


# OUTDATED! UNABLE TO BIND TO 0.0.0.0 DUE TO SSL ENFORCEMENT!