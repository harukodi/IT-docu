
#### See the current size of the logical volume
```bash
df -hT /dev/mapper/ubuntu--vg-ubuntu--lv
```
 
#### Run a test first
```bash
sudo lvresize -tvl +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```

#### Resize the logical volume
```bash
sudo lvresize -vl +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```

#### Resize the filesystem
```bash
sudo resize2fs -p /dev/mapper/ubuntu--vg-ubuntu--lv
```

#### Check the new size of the logical volume
```bash
df -hT /dev/mapper/ubuntu--vg-ubuntu--lv
```