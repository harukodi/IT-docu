## Install iptables-persistent if it's missing from your system
```bash
sudo apt install iptables-persistent
```

## Open a port e.x port 80
```bash
sudo iptables -I INPUT -m state --state NEW -p tcp --dport 80 -j ACCEPT
```

## Remove the port opening
```bash
sudo iptables -D INPUT -m state --state NEW -p tcp --dport 80 -j ACCEPT
```


## Add nat dns routing
```bash
sudo iptables -t nat -A PREROUTING -p udp --dport 53 -j DNAT --to-destination 76.76.2.199:53
sudo iptables -t nat -A PREROUTING -p tcp --dport 53 -j DNAT --to-destination 76.76.10.199:53
```

## Add masqurade
```bash
sudo iptables -t nat -A POSTROUTING -p udp --dport 53 -d 76.76.2.199 -j MASQUERADE
sudo iptables -t nat -A POSTROUTING -p tcp --dport 53 -d 76.76.10.199 -j MASQUERADE
```

## Remove nat dns routing
```bash
sudo iptables -t nat -D PREROUTING -p udp --dport 53 -j DNAT --to-destination 76.76.2.199:53
sudo iptables -t nat -D PREROUTING -p tcp --dport 53 -j DNAT --to-destination 76.76.10.199:53
```

## Used to save your rules so that they persists after a reboot
```bash
sudo netfilter-persistent save
```

## List iptables rules
```bash
sudo iptables -L
```

## List iptables rules by specification
```bash
sudo iptables -S
```
## List iptables nat rules
```bash
sudo iptables -t nat -L
```