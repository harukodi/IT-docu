## Create a namespace
```bash
kubectl create namespace [namespace-name]
```
## Delete namespace
```bash
kubectl delete namespace [namespace-name]
```

## Change the default namespace for kubectl
```bash
kubectl config set-context --current --namespace=[namespace-name]
```

## List all namespaces
```bash
kubectl get pods --all-namespaces
```
## Show all resources in a namespace
```bash
kubectl get all --namespace [namespace-name]
```

## Retrieve details on your nodes
```bash
kubectl get nodes
```


## Leverage your files to configure Kubernetes
```bash
kubectl apply -f config-file.yaml
```

## Launch a troubleshooting container for Kubernetes
```bash
kubectl run -it xia1997x-k8s-small-utils --restart=Never --rm --image=xia1997x/pub:k8s-small-utils -- /bin/bash
```
## Delete a resource
```bash
kubectl delete -f config-file.yaml
```

## Get logs for specific containers.
```bash
kubectl logs -f -l app=[label-selector] --all-containers=true
```
---
# Labeling
## Label a node
```bash
kubectl label nodes [node] [label_name]=[label_value]
```
**NOTE:** Do not include the brackets `[]`
## Show node labels
```bash
kubectl get nodes --show-labels
```
---
## Fetch your secrets
```bash
kubectl get secrets
```


## Fetch services
```bash
kubectl get services
```

## Show api resources
```bash
kubectl api-resources
```

## Show services in a specific namespace
```bash
kubectl get svc --namespace default
```

## Show ReplicaSets
```bash
kubectl get replicasets
```
## Show endpointslices for services
```bash
kubectl get endpointslices
```
**NOTE:** Used to show endpoints for services

---
# Ingress
## Show ingressclasses
```bash
kubectl get ingressclass
```
## Show ingress objects
```bash
kubectl get ingress
```

---
# Rollouts and roll backs
## Show rollout status for deployments
```bash
kubectl rollout status deployment [deployment-name]
```
## Pause a deployment rollout
```bash
kubectl rollout pause deployment [deployment-name]
```
## Resume a deployment rollout
```bash
kubectl rollout resume deployment [deployment-name]
```

## Show revision history for a deployment
```bash
kubectl rollout history deployment [deployment-name]
```

## Roll back to specific revision
```bash
kubectl rollout undo deployment [deployment-name] --to-revision=[revision-number]
```
---
## Get pod logs
```bash
kubectl logs [pod-name]
```
## Exectute command in a running container
```bash
kubectl exec [pod-name] -- [command-to-run]
```
## Get an interactive shell for a running container
```bash
kubectl exec -it [pod-name] -- [shell-type]
```
**Extra flag:**
- --container `This flag can be used if you are running a multi-container Pod` 
- --it `This flag can be used to get an interactive shell to a container`
---
## StorageClasses, PVs and PVCs
## Show your StorageClasses
```bash
kubectl get storageclasses
```
## Get the description of a storage class
```bash
kubectl describe storageclass [storage-class-name]
```
---
## ConfigMaps and Secrets
```bash
kubectl create configmap [configmap-name] --from-literal [key=value]
```

---
# Api resources
## List all API resources and their supported verbs
```bash
kubectl api-resources --sort-by name -o wide
```
## Get resources of specific apiGroup
```
kubectl api-resources --sort-by name -o wide | grep [api-group]
```
### Example to get resources of specific apiGroup
`kubectl api-resources --sort-by name -o wide | grep apps`

---
# kubectl flags
- `--watch` start watching updates to a particular object ex pods