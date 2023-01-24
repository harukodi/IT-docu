

## Adding the helm repo
```bash
helm repo add nginx-stable https://helm.nginx.com/stable
```


## Update helm repos
```bash
helm repo update
```


## Install nginx-ingress on to your Kubernetes cluster (my-release) is the name that you choose for Nginx

```bash
helm install my-release nginx-stable/nginx-ingress
```


## Install nginx-ingress on to your k3s cluster (my-release) is the name that you choose for Nginx

```bash
helm install my-release nginx-stable/nginx-ingress --kubeconfig=/etc/rancher/k3s/k3s.yaml
```