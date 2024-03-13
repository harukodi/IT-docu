## Create the directory for the caddy-server with the following command
```bash
mkdir -p caddy-server/{caddy,caddy_site}
```


## Use the cd command to change your directory to "caddy-server/"
```bash
cd caddy-server/
```


## Create the docker volumes for the Caddy-server installation with the following command
```bash
docker volume create caddy_data; docker volume create caddy_config
```


## Create the docker-compose.yml
```bash
touch docker-compose.yml
```


## Edit the docker-compose.yml with nano
```bash
nano docker-compose.yml
```


## Input the following content to your docker-compose.yml file
```yaml
version: "3.7"

services:
  caddy:
    image: caddy:latest
    container_name: caddy-server
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - ./caddy/Caddyfile:/etc/caddy/Caddyfile
      - ./caddy_site/site:/srv
      - caddy_data:/data
      - caddy_config:/config
volumes:
  caddy_data:
    external: true
  caddy_config:
networks:
  caddy-server_default:
    driver: bridge
```


## Create the Caddyfile at "caddy/Caddyfile" with the following command
```bash
touch caddy/Caddyfile
```


## Example config for the Caddyfile
```Caddyfile
{
    # Global options block. Entirely optional, https is on by default
    # Optional email key for lets encrypt
    email yourmail@example.com
    # Optional staging lets encrypt for testing. Comment out for testing.
    # acme_ca https://acme-staging-v02.api.letsencrypt.org/directory
}
subdomain.domain.tld {
    reverse_proxy 0.0.0.0:1111
    encode zstd gzip
}
```


## Use nano to open the caddyfile and input the content above
```bash
nano caddy/Caddyfile
```


## Start the caddy-server container with the following command
```bash
docker-compose up -d
```