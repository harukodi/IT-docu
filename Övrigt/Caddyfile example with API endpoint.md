## Caddyfile
```Caddyfile
{
    # Global options block. Entirely optional, https is on by default
    # Optional email key for lets encrypt
    email example@gmail.com
    # Optional staging lets encrypt for testing. Comment out for production.
    # acme_ca https://acme-staging-v02.api.letsencrypt.org/directory
}

# this below is only for reverse proxying a service/app
www.example.com {
    reverse_proxy 10.1.1.12:3001
    encode zstd gzip
 header {
 	Strict-Transport-Security max-age=31536000;
 
 	X-Content-Type-Options nosniff
 
 	X-Frame-Options DENY

  X-XSS-Protection 1
 }
}

# Webhook endpoint this below only allows peers to access the api thus blocking password bruteforces on a dashboard login
api.example.com {
  route /api* {
    reverse_proxy https://10.1.1.12:9443 {
    	transport http {
		tls
		tls_insecure_skip_verify
	  }
    }
  }
  respond "Not found" 404
}
```
[[Caddy-server installation]]