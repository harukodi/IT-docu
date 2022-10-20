# Run a bash script when the system starts up


## Create a service with the command
**NOTE** Do not include the arrows when opening your file
```bash
nano /lib/systemd/system/<name-of-service>.service
```


# Paste the following content in your file
**NOTE** In ExecStart put the filepath where your sh script lives example "/scripts/smb-connector.sh"
```text
[Unit]
Description=SMB mount script
After=network-online.target
[Service]
ExecStart=/path/
[Install]
WantedBy=multi-user.target
```


# Enable your service
**NOTE** Do not include the arrows
```bash
systemctl enable <name-of-service>.service
```


# Start your service 
**NOTE** Do not include the arrows
```bash
systemctl start <name-of-service>.service
```

