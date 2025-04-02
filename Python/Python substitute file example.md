

```yaml
services:
  ${stack_name}:
    image: ${stack_image}
    networks:
    - traefik-public
    restart: always
    deploy:
      mode: replicated
      replicas: 1
      labels:
      - traefik.enable=true
      - traefik.docker.network=traefik-public
      - traefik.constraint-label=traefik-public
      - traefik.http.routers.${stack_name}-http.rule=Host(`${stack_name}.${domain_name}`)
      - traefik.http.routers.${stack_name}-http.entrypoints=http
      - traefik.http.routers.${stack_name}-http.middlewares=https-redirect
      - traefik.http.routers.${stack_name}-https.rule=Host(`${stack_name}.${domain_name}`)
      - traefik.http.routers.${stack_name}-https.entrypoints=https
      - traefik.http.routers.${stack_name}-https.tls=true
      - traefik.http.routers.${stack_name}-https.tls.certresolver=le
      - traefik.http.services.${stack_name}.loadbalancer.server.port=8000
networks:
  traefik-public:
    external: true
```


## Python code to change the ${placeholders}
```py
def update_docker_compose_file_with_envs(stack_name, stack_image, docker_compose_file_path):
    temp_file_path = "temp-files"
    docker_compose_envs = {
        "stack_name": stack_name,
        "stack_image": stack_image,
        "domain_name": domain_name   
    }
    
    with open(f"{docker_compose_file_path}/docker-compose.yaml", "r") as compose_file:
        docker_compose_file = compose_file.read()
    
    template = Template(docker_compose_file)
    docker_compose_file = template.substitute(docker_compose_envs)
    with open(f"{temp_file_path}/docker-compose-temp.yaml", "w") as compose_file:
        compose_file.write(docker_compose_file)
```