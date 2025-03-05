

## Create a namespace
```bash
kubectl create namespace [namespace-name]
```

## Change the default namespace for kubectl
```bash
kubectl config set-context --current --namespace=[namespace-name]
```

## List all namespaces
```bash
kubectl get pods --all-namespaces
```


## Retrieve details on your nodes
```bash
kubectl get nodes
```


## Leverage your files to configure Kubernetes
```bash
kubectl apply -f config-file.yaml
```


## Delete a resource
```bash
kubectl delete -f config-file.yaml
```


## Fetch your secrets
```bash
kubectl get secrets
```


## Fetch services
```bash
kubectl get services
```