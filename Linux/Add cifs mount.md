
## Command to add a CIFS mount with explanation
**NOTE** Do not include the {} when using the command
```bash
mount -t cifs -o username={namn of the smb account},password={password of the smb account} //192.168.1.11/{shared folder on the smb share} /{local folder of the location to mount the smb share}
```


## Command to add a CIFS mount without explanation
**NOTE** Do not include the {} when using the command
```bash
mount -t cifs -o username={},password={} //192.168.1.11/{} /{}
```

## Add cifs mount to a user's uid
**NOTE** Do not include the {} when using the command
```bash
mount -t cifs -o username={},password={},uid={} //192.168.1.11/{} /{}
```
### to list your user id you can use the following command
```bash
id example-user
```