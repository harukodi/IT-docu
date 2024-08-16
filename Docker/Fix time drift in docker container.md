**NOTE:** To fix time drift in your Docker container and sync the container's clock with your host machine, you can add the following volume mounts to your docker-compose file:
```yaml
    volumes:
      - "/etc/timezone:/etc/timezone:ro"
      - "/etc/localtime:/etc/localtime:ro"
```