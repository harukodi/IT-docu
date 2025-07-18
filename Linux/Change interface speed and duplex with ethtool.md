## Use ethtool to change speed and duplex
**NOTE:** This only allows changing to the fixed speeds supported by the NIC
```bash
sudo ethtool -s <interface> speed 10 duplex full
```