

## Services > Dns Resolver > Display Custom Options

### Paste this in the "Custom options" box
```toml
server:
  forward-zone:
    name: "."
    forward-tls-upstream: yes
    forward-addr: 76.76.2.181#REDACTED.dns.controld.com
    forward-addr: 76.76.10.181#REDACTED.dns.controld.com
```

## That's it! :>