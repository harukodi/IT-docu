# Script example file
```bash
#!/bin/bash
# make script executable
# edit /etc/pam.d/sshd and add:
# session   optional   pam_exec.so /path/to/file.sh

WEBHOOK_URL="webhook-url-goes-here"


# Capture only open and close sessions.
case "$PAM_TYPE" in
    open_session)
        PAYLOAD=" { \"content\": \"User: \`$PAM_USER\` logged in to \`$HOSTNAME\` (remote host: $PAM_RHOST).\" }"
        ;;
    close_session)
        PAYLOAD=" { \"content\": \"User: \`$PAM_USER\` logged out of \`$HOSTNAME\` (remote host: $PAM_RHOST).\" }"
        ;;
esac

# If payload exists send a webhook
if [ -n "$PAYLOAD" ] ; then
    curl -X POST -H 'Content-Type: application/json' -d "$PAYLOAD" "$WEBHOOK_URL"
fi
```
<br>

# Make script executable
```bash
chmod +x script-name.sh
```
# Edit /etc/pam.d/sshd with nano
```bash
nano /etc/pam.d/sshd
```

# Add the following at the end of the file
```bash
session   optional   pam_exec.so /path/to/script-name.sh
```


# Done ayaaa!!!