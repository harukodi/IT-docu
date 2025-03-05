```bash
scp -r . emby-server:/home/xia1997x/mics/docker-image-build; ssh emby-server -t "cd mics/docker-image-build; bash"
```

```bash
sudo docker buildx build --platform linux/amd64 -t myapp:latest . --push; rm -rf *; exit
```