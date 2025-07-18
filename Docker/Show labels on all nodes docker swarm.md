```bash
sudo docker node inspect $(sudo docker node ls -q) | jq '.[] | {hostname: .Description.Hostname, Labels: .Spec.Labels}'
```