## Open the resolved.conf file
```bash
sudo nano /etc/systemd/resolved.conf
```
## Add the following to the file
```
DNS=76.76.2.199 76.76.10.199
```
## Restart the systemd-resolved.service
```
sudo systemctl status systemd-resolved.service
```

## **NOTE:** If you are on DigitalOcean remove the digitalocean file in
`/etc/systemd/resolved.conf.d/`