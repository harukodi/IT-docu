```yaml
services:
  minecraft-server:
    user: 1000:1000
    container_name: minecraft-server-ct
    image: xia1997x/priv-registry:minecraft-server
    restart: always
    stdin_open: true
    tty: true
    environment:
      - JVM_XMX=2024M
      - JVM_XMS=2024M
    ports:
      - 25565:25565
    volumes:
      - ./config:/minecraft-server
```