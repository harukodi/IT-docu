## Example `config.yaml` file
```yaml
config: {}
networks:
- config:
    ipv4.address: auto
    ipv6.address: auto
  description: ""
  name: incusbr0
  type: ""
  project: default
storage_pools:
- config: {}
  description: ""
  name: incus-storage-pool
  driver: dir
profiles:
- config: {}
  description: ""
  devices:
    eth0:
      name: eth0
      network: incusbr0
      type: nic
    root:
      path: /
      pool: incus-storage-pool
      type: disk
  name: default
projects: []
cluster: null
```

## incus file location `Ubuntu`
```
/var/lib/incus
```