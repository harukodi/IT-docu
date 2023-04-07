## Create the namespace named Argocd
```bash
kubectl create namespace argocd
```

## Apply the Argocd manifest
```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
## Command to fetch the password for the admin user of Argocd
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```