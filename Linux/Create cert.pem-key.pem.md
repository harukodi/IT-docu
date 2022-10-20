# Create cert.pem and key.pem

## Generate cert.pem-key.pem
```bash
openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem
```

## Generate cert.pem-key.pem without prompts
**NOTE** Use this command when using automation tools
```bash
openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem -subj "/C=/ST=/L=/O=/OU=/CN="
```