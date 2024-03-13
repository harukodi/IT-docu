# Installation of Rancher

## Make the directories for the rancher installation
```bash
mkdir -p rancher/conf
```

## Cd in to the directory
```bash
cd rancher
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
    image: 'rancher/rancher:latest'
    container_name: rancher
    restart: unless-stopped
    ports:
      - '80:80'
      - '443:443'
    privileged: true
    volumes:
      - ./conf:/var/lib/rancher
```

## Run the docker-compose up command to start rancher
```bash
docker-compose up -d
```

### Now you have finished installing **rancher**
