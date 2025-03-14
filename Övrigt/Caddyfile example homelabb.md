```Caddyfile
{
  acme_dns cloudflare {env.CLOUDFLARE_AUTH_TOKEN}
}

(security_headers) {
    header {
	    # enable HSTS
	    Strict-Transport-Security max-age=31536000; includeSubDomains; preload
	    # disable clients from sniffing the media type
	    X-Content-Type-Options nosniff
	    # clickjacking protection
	    X-Frame-Options DENY
    }
}

(zstd_comp) {
    encode * {
        zstd fastest
        minimum_length 128
    }
}

(global_alpn) {
    tls {
        alpn h2 h3
    }
}

domain.tld {
    import security_headers
    import global_alpn
    tls {
        protocols tls1.3 tls1.3
    }
    reverse_proxy http://localhost:8096 {
        header_up X-Real-IP {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
        transport http {
            versions 2
        }
    }
}

subdomain.domain.tld {
    import security_headers
    import zstd_comp
    import global_alpn
    tls {
        protocols tls1.3 tls1.3
    }
    reverse_proxy https://localhost:4443 {
        header_up X-Real-IP {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
        transport http {
            versions 2
            tls_insecure_skip_verify
        }
    }
}

subdomain.domain.tld {
    import security_headers
    import zstd_comp
    import global_alpn
    tls {
        protocols tls1.3 tls1.3
    }
    reverse_proxy http://localhost:5678 {
        header_up X-Real-IP {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
        transport http {
            versions 2
        }
    }
}

subdomain.domain.tld {
    import security_headers
    import zstd_comp
    import global_alpn
    tls {
        protocols tls1.3 tls1.3
    }
    reverse_proxy https://localhost:7443 {
        header_up X-Real-IP {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
        transport http {
            versions 2
            tls_insecure_skip_verify
        }
    }
}

subdomain.domain.tld {
    import security_headers
    import zstd_comp
    import global_alpn
    tls {
        protocols tls1.3 tls1.3
    }
    reverse_proxy https://localhost:9443 {
        header_up X-Real-IP {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
        transport http {
            versions 2
            tls_insecure_skip_verify
        }
    }
}

subdomain.domain.tld {
    import security_headers
    import zstd_comp
    import global_alpn
    tls {
        protocols tls1.3 tls1.3
    }
    reverse_proxy http://localhost:7345 {
        header_up X-Real-IP {http.request.header.CF-Connecting-IP}
        header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
        transport http {
            versions 2
        }
    }
}
```