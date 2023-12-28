### Step 1: Use apt to install software-properties-common
```bash
sudo apt-get install software-properties-common
```
### Step 2: Use the add-apt to add the suricata repo
```bash
sudo add-apt-repository ppa:oisf/suricata-stable
```
### Step 3: Update your repos
```bash
sudo apt update
```
### Step 4: Install suricata with apt
```bash
sudo apt install suricata jq
```


> **NOTE:** This command can be used to convert all of the suricata rules from alert to drop
```bash
sed -i "s/alert/drop/g" /var/lib/suricata/rules/suricata.rules
```