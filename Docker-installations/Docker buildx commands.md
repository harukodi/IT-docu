## Use this command to enable docker buildx
```bash
sudo docker buildx create --use
```

## Use this command to build the docker image
**NOTE:** In the `--platform` parameter, if you omit certain architectures, Docker will build the image only for the specified architectures. the `--push` parameter can be omited if you dont want to push to a registry.
**The supported platforms are the following:**
- linux/amd64
- linux/arm64
- linux/arm/v7
- linux/arm/v6
- linux/ppc64le
- linux/s390x
```bash
sudo docker buildx build --platform linux/amd64 -t myapp:latest . --push
```

```bash
sudo docker push your_docker_username/your_docker_repo:latest
```