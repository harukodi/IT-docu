## Scaling methods
### CA= Cluster Autoscaler
- Remove and adds nodes automatically to the kubernetes cluster
- Helps to increase the cluster resources for pods and deployments/etc
### HPA= Horizontal Pod Autoscaler
- Adds and removes pods to meet current demand, this scales horizontaly
### VPA= Vertical Pod Autoscaler
- Helps to increase the memory and cpu of pods.
- Has some limitations. The pod need to be recreated for the new resource limits to change for the current demand
- Pods might also be moved to different nodes
- Not commonly used due to this limitations
### multi-dimensional autoscaling
- It’s when you use multiple methods for scaling, like HPA and CA, to meet the current demand

---
## Understanding States and Management Methods

- Desired state = the state you want your deployment or pods in
- Observed state = current state of deployment or pods
- Reconciliation = When desired state does not match the observed state
### Imperative
- You have to give all instructions and how to get to the desired state
- If you are using the imperative method you will need to provide all of the kubernetes commands to get to your desired state
- Prone to more errors
### Declarative
- You tell what desired state you want without needing to tell how to get to the desired state
- When using the declarative method you give the `how` to get to the desired state to kubernetes
- How you get to your desired state is managed by kubernetes
- Less errors easier to manage deployments and pods
---

# Init and sidecar containers
### Init containers
**NOTE:** Defined under `spec.initContainers` in the yaml file
- Are run before the main container/s
- Multiple init containers can be defined before the main container starts
- Init containers are run in the order they are specified in the yaml file
- Uses cases for init containers (Waiting for a redis cluster to come up)/(DB migration)
### Sidecar containers
**NOTE:** Defined under `spec.containers` in the yaml file
- Sidecar container always run alongside your main app container
- To make it a sidecar container set the `restartPolicy: Always` on the pod-level or container-level
- Sidecar container is like a (sidecar motorcycle)
- Uses cases for sidecar containers, Loggin agents, Proxies, File syncers, Metric exporters

## Deployment with ReplicaSet
- `spec.revisionHistoryLimit` Tells Kubernetes to keep a number of previous ReplicaSets so you can roll back. Keep in mind, storing too many ReplicaSets can bloat the object, especially in larger clusters with frequent releases.

- `spec.progressDeadlineSeconds` tells Kubernetes to give each new replica a start window before reporting the replica as stalled. All replicas get their own window, meaning each replica has its own start up window to come up properly (progress).

- `spec.strategy` Tells Kubernetes how to update the pods when doing a rollout

- `spec.template` Defines the pod for the deployment
---
### Pods are ephemeral and gets deleted when the following events occur
- Scale-down operations
- Rolling updates
- Rollbacks
- Node eviction/maintenance
- Failures
---
## Services
- Services helps to forward traffic to the right pods. 
- Services are used becuase connecting directly to running pods is not reliable.
- A service object sits in front of one or more identical pods to give them a reliable DNS name, IP address and a port.
- Services also knows which pods are healthy and which ones that aren't, if pods are unhealthy the service will not forward traffic to them.
- Service names must be unique within the same namespace for dns to work.
- Same service name is allowed if you seperate your services in their own namespaces.
- To access the same service name but on a different namespace you do as following when you set up your dns endpoint `<object-name>.<namespace>.svc.cluster.local`.

### Service types
#### ClusterIP
- Provides a reliable endpoint (name, ip, and port)
- Only works for internal pod traffic (Internal pod network)
- The IP provided is only routable within the internal pod network
- Public access is not possible with ClusterIP
#### NodePort
- NodePort builds on top of ClusterIP
- Allow external access by publishing a dedicated port on every cluster node
- External traffic is allowed when trying to access the app/pods on the cluster
- Allows you to use an external load balancer if you wish to do that
- Per default uses simple round robin for load balancing between the service and the pods
- Need to know the cluster nodes ips and port(disadvantage)
- Must also check if cluster nodes are healthy
- Uses high-numbered ports between `30000-32767`
#### LoadBalancer
- Used with Kubernetes clusters in the cloud
- Puts a cloud loadbalancer in front of your service
- Easier to expose your services to the public
#### Endpointslices
- Allows your services to maintain a list of healthy pods
- Supports IPv4, IPv6 and FQDN
- Allows the service to scale more efficiently for example to your backends
- Each endpointslice allows up to 100 endpoints per default

---
## liveness and readiness probes
### liveness probe
- A `liveness probe` checks if the pod is still alive. If the pod does not respond to the probe, Kubernetes will restart it to ensure the application stays available.
### readiness probe
- A `readiness probe` checks if the pod is ready to receive traffic. If the pod is not ready, Kubernetes will not route traffic to it, ensuring that only healthy pods receive traffic.

---
### Wasm VS Containers
#### Wasm
- Build once run everywhere
- Application is run in a deny default manner
- You need to specify what permissions you want.
- Permissions are not wide open like on a docker container
- Has its own bytecode and needed runtime to run the applications
- Smaller faster and actually portable
#### Containers
- Wide open permissions
- Images needs to be built for different archs
- Not really portable due to the need of different arch builds
- More images to handle than Wasm binary