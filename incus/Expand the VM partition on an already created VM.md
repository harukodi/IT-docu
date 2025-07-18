## 1. Stop your vm
```bash
incus stop VM-NAME
```
## 2. Expand root partition
```bash
incus config device set VM-NAME root size=100GiB
```
## 3. Then restart your vm
```bash
incus restart VM-NAME
```