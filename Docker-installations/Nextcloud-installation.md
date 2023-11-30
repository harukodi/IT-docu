# Installation of nextcloud


## Create the folders for the nextcloud installation
```bash
mkdir -p nextcloud/{appdata,data}
```

## Cd in to the directory
```bash
cd nextcloud
```

## Make the docker-compose.yml file
```bash
touch docker-compose.yml
```

## Open the docker-compose.yml with nano
```bash
nano docker-compose.yml
```

## Paste the following content in your docker-compose.yml
```yaml
version: "2.1"
services:
  nextcloud:
    image: lscr.io/linuxserver/nextcloud:latest
    container_name: nextcloud
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    volumes:
      - ./appdata:/config
      - ./data:/data
    ports:
      - 443:443
    restart: unless-stopped
```

## Run the docker-compose up command to start nextcloud
```bash
docker-compose up -d
```

### Now you have finished installing **nextcloud**
[[Using authentik OICD nextcloud template]]
