# Installation of nginx-proxy-manager


## Make a directory for the nginx-proxy-manager installation
```bash
mkdir nginx-proxy-manager
```

## Cd in to the directory
```bash
cd nginx-proxy-manager
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
```yml
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

## Run the docker-compose up command to start nginx-proxy-manager
```bash
docker-compose up -d
```

### Now you have finished installing **nginx-proxy-manager**
