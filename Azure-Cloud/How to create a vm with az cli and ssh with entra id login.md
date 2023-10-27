
## Run the following command to create a vm in Azure cloud
```bash
az vm create -g resource-group-name-goes-here --name vm-name-goes-here --image Ubuntu2204 --assign-identity --admin-username example --generate-ssh-keys
```

## Enable ssh auth with Microsoft Entra login
```bash
az vm extension set --publisher Microsoft.Azure.ActiveDirectory --name AADSSHLoginForLinux --resource-group resource-group-name-goes-here --vm-name vm-name-goes-here
```

## Use this command to ssh in to your newly created vm
```bash
az ssh vm -g resource-group-name-goes-here -n vm-name-goes-here
```

# Done :>