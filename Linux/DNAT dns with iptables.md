## Install iptables-persistent if it's missing from your system
```bash
sudo apt install iptables-persistent
```
## Add nat dns routing
```bash
sudo iptables -t nat -A PREROUTING -p udp --dport 53 -j DNAT --to-destination 76.76.2.199:53
sudo iptables -t nat -A PREROUTING -p tcp --dport 53 -j DNAT --to-destination 76.76.10.199:53
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
