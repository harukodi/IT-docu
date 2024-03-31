```Caddyfile
subdomain.example.tld {
  redir / /status/ 302

  route /status/* {
  	reverse_proxy 0.0.0.0:3001
  }
  route /assets/* {
  	reverse_proxy 0.0.0.0:3001
  }
  route /icon.svg {
  	reverse_proxy 0.0.0.0:3001
  }
  route /api/* {
  	reverse_proxy 0.0.0.0:3001
  }
  route /upload/* {
  	reverse_proxy 0.0.0.0:3001
  }
  route /metrics/* {
  	reverse_proxy 0.0.0.0:3001
  }

  respond 403
}
```