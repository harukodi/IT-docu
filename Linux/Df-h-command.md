# df -h command


#### the df command gives you a report of the file system disk space usage
```bash
df -h
```


## Params
```bash
-a, --all
              include pseudo, duplicate, inaccessible file systems

       -B, --block-size=SIZE
              scale  sizes  by  SIZE before printing them; e.g., '-BM' prints sizes in units of 1,048,576 bytes; see SIZE
              format below

       -h, --human-readable
              print sizes in powers of 1024 (e.g., 1023M)

       -H, --si
              print sizes in powers of 1000 (e.g., 1.1G)

       -i, --inodes
              list inode information instead of block usage

       -k     like --block-size=1K

       -l, --local
              limit listing to local file systems
```