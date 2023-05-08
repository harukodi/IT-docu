## Edit the resolved.conf file
```bash
sudo nano /etc/systemd/resolved.conf
```

## Add the following line to the resolved.conf file
```
DNS=1.1.1.1 1.0.0.1
```
**Change 1.1.1.1 and 1.0.0.1 to your prefered dns servers**

## Once you are done with the changes of your dns servers, reload systemd-resolved
```bash
sudo systemctl restart systemd-resolved.service
```

## That's it!