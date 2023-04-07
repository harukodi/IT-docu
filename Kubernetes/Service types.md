## An abstract way to expose an application running on a set of [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) as a network service.
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.


## NodePort
```yaml
apiVersion: v1
kind: Service
metadata:
  namespace: <NAMESPACE>
  name: service-name-for-the-app
spec:
  type: NodePort
  selector:
    app: app-name-selector
  ports:
      # By default and for convenience, the `targetPort` is set to the same value as the `port` field.
    - port: 80
      targetPort: 80
      # Optional field
      # By default and for convenience, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
      nodePort: 30007
```


## ClusterIP
```yaml
apiVersion: v1
kind: Service
metadata:
  namespace: <NAMESPACE>
  name: service-name-for-the-app
spec:
  type: ClusterIP
  # Uncomment the below line to create a Headless Service
  # clusterIP: None
  selector:
    app: app-name-selector
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```


## LoadBalancer
```yaml
apiVersion: v1
kind: Service
metadata:
  namespace: <NAMESPACE>
  name: service-name-for-the-app
spec:
  type: LoadBalancer
  selector:
    app: app-name-selector
  ports:
  - protocol: TCP
    port: 60000
    targetPort: 50001
```