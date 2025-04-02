
## Open docker.service file with the following command
```bash
sudo systemctl edit docker.service
```

## Add this to the top of the file under the first comment
```
[Service]
ExecStartPre=/bin/sleep 60
```