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

---
## Volume access modes
`PVC=Persistent Volume Claim` A request from a pod for how much storage it needs. The user can also set the PVC's access mode.
`PV=Persistent Volume` A pice of storage allocated by an admin or via a StorageClass.
`SC=StorageClass` Represents storage tiers when creating volumes.
### ReadWriteOnce (RWO)
- A single PVC is allowed to bind to a volume with read-write mode (R/W). 
- If you try to bind it from multiple PVCs the bind will fail for that volume
### ReadWriteMany (RWM)
- Multiple PVCs are allowed to bind to a volume, with read-write(R/W) mode.
- File and object storage usually supports this mode.
- block storage usually dont allow this mode
### ReadOnlyMany (ROM)
- Allows multiple PVCs to bind to the volume with read-only(R/O) mode.

## Reclaim Policies
### Delete
- Is the defualt method when a PVC is released.
- Deletes the PV and associated external storage
- Deleting the PVC will also delete the PV and the external storage
- Use with caution
### Retain
- Will keep the PV and the external storage after the PVC is deleted.
- Safe as it wont delete the external storage and the volume when the PVC is released
- You will need to reclaim it manually after you release the PVC
**NOTE:** Steps to reclaim the volume according to the k8s docs
1. Delete the PersistentVolume. The associated storage asset in external infrastructure still exists after the PV is deleted.
2. Manually clean up the data on the associated storage asset accordingly.
3. Manually delete the associated storage asset.
---
## ConfigMaps and Secrets
### ConfigMaps
`ConfigMaps are injected in to containers at runtime`
- Stores non sensitive data
- Hostnames
- Service names and services ports
- Account names
- Environment variables
- Configuration files, web server configs and database configs
- Can be mounted as a volume
- **DO NOT USE ConfigMaps to store certs or passwords**
### Secrets
- Can be used to store passawords and certs
- Uses base64 to encode data `dataString` can be used if plain text is preferred
- Secrets are not encrypted in the cluster store or while in flight over the network
- `BASE64` is NOT an encryption
- Can be mounted as a volume
- Usually secrets are stored outside of kubernetes for example `Hashicorp Vault`

---
## StatefulSets
**NOTES:** Has the same features as a Deployment, like self-healing, rollouts, scaling, and more. However, if you are using a Deployment, you will not have the following three features of a StatefulSet.

- Allows predictable and persistent Pod names and DNS names
- Allows predictable and persistent volume bindings
- Allows predictable startup and shutdown order of the pods
- When scalling down the pod with the highest index number will get terminated

The three mentioned features will be kept if a pod fails, is scaled, or during other scheduling events.
### StatefulSet and Deployment order: creation and deletion of pods
- ``StatefulSet`` creates one pod at a time and waits for it to start up before moving on to creating and starting other pods
- ``Deployment`` Uses a ReplicaSet controller to start all the pods at the same time, this can result in race conditions
### StatefulSet and DNS resolution with headless service
- ``<pod-name>.<service-name>`` You can use this method to ping the endpoint from a jump container, but it only works if the service is in the **same namespace** as the `StatefulSet`.
- ``<object-name>.<service-name>.<namespace>.svc.cluster.local`` Use this method to ping the endpoint from a jump container when the `StatefulSet` and the service are in **different namespaces**.

**SELF NOTES:** I think we use a headless service because a ClusterIP shouldn't be given to the service, as the pods in a StatefulSet already have their own IPs. Therefore, it is not feasible to have an IP on a service, as load balancing will happen — but with StatefulSets, this may not be desired. We also just want the headless service to be a governor over the DNS names created for each pod and their own IPs.

---
# RBAC and Admission controller
## Roles and RoleBindings
### Roles
- Defines a set of permissions within a particular namespace. Roles does nothing unless you bind them to a user.
### RoleBindings 
- Binds the permissions to a user, when you want to bind a ``role`` you do it with a RoleBinding. A RoleBinding grants permissions within a specific namespace.
### Roles object
- ``verbs`` = Actions that are allowed on a kubernetes cluster like get watch and list.
- ``apiGroups`` = This can be seen as what groups of **API endpoints (resources)** the Role applies to. For example, ``apps`` is an API group, which contains endpoints like deployments, statefulsets, etc.
- ``resources`` = Specific resource(s) the role should apply to within the given apiGroup(s), like deployment or pods etc.
## ClusterRole and ClusterRoleBinding
### ClusterRole object
- Allows you to create a role for the whole cluster.
- Can be reused with a RoleBinding to have this ClusterRole on multiple namespaces.
### ClusterRoleBinding
- Grants permissions on the whole cluster.
- If you want to bind a ClusterRole to all the namespaces in your cluster, you use a ClusterRoleBinding.

## Admission controller
The **Admission Controller** checks the API request before it’s persisted to the cluster. Policies are evaluated before a resource is created. These checks are run after authorization.
Admission controllers are used to enforce policies such as:
- A policy that requires images to be pulled always with the image policy set to `AlwaysPull`
- A policy that requires pods to have a specific label, such as `prod`

**SELF NOTES:** You can use **ClusterRoles** with **RoleBindings** to define common roles once and then reuse the same permissions across multiple namespaces.  
**Roles** are namespaced objects, meaning you can only reference them from a **RoleBinding** within the same namespace. **ClusterRoles** and **ClusterRoleBindings** are cluster-wide objects that can be applied across the entire cluster and all its namespaces.

---

# Threat modeling
## STRIDE:
- ``Spoofing`` = This attack vector involves impersonating a user to gain elevated permissions and bypass system restrictions.
- ``Tampering`` = To change a resource in a malicious way to cause a DOS or Elevation of privilege.
- ``Repudiation`` = Creates doubt about some action made to the Kubernets cluster. non-repudiation is a proof that an action was carried out by a specific person and their actions to the cluster.
	- What happened
	- When it happened
	- Who made it happen
	- Where it happened
	- Why it happened
	- How it happened
- ``Information disclosure`` = This occurs when sensitive data is leaked — for example, through an open API endpoint that shouldn't be accessible from the public internet or one that lacks proper authentication.
- ``Denial of service`` = This is when a service is made unavailable — for example, by repeatedly pinging the server until it crashes or becomes unresponsive.
- ``Elevation of privilege`` = This is when you gain higher privileges than what was originally granted. The aim is to cause damage or gain unauthorized access to resources that you shouldn't have access to.

## Extra tips for where to get audit logs
- Collect logs from container runtimes.
- Use agents with DaemonSet to collect metrics over the kubernetes nodes. This also helps to get your audit logs centralized in one place.
- Use the Kubernetes API to get audits on event that occurred.
- Kubelets also provides audit logs.
- In your applications have some sort of audit logs.
- You should also audit non-Kubernetes infrastructure, such as external components like your firewalls or CDNs.
## Reduce tampering attempts in Kubernetes.
- Restrict access to the server running k8s components.
- Restrict access to repos that holds k8s configs.
- Perform bootstrapping over SSH (Protect your SSH keys).
- Run SHA-2 on files that you download to check their integrity.
- Restrict access to image repos and other repos
- Set file system to read-only on a pod, use securityContext to configure more access rules for the filesystem of a pod.
### securityContext
- ``readOnlyRootFilesystem`` Makes the containers root file system read-only
- `allowedHostPaths` Is the same as the above but you can do this for other paths in the container.
- ``runAsUser`` Can be set to run a container as a non-root user
- `allowPrivilegeEscalation` This prevents privilege escalation within containers.

## Pod Security Standards (PSS)
Every Kubernetes cluster gets the following three PSS policies that are maintained and kept up-to-date by the community:
- Privileged
- Baseline
- Restricted

``Privileged`` is a wide-open allow-all policy. 

``Baseline`` implements sensible defaults. It’s more secure than the privileged policy but less secure than restricted. 

``Restricted`` is the gold standard that implements the current Pod security best practices. Be warned though, it’s highly restricted, and lots of Pods will fail to meet its strict requirements.

## Pod Security Admission (PSA)
Pod Security Admission (PSA) enforces your desired PSS policies. It works at the Namespace level and is implemented as a validating admission controller.

PSA offers three enforcement modes:
- ``Warn``: Allows violating Pods to be created but issues a user-facing warning
- ``Audit``: Allows violating Pods to be created but logs an audit event
- ``Enforce``: Rejects Pods if they violate the policy

### Apply the following labels to a namespace to enable the desired policy:
#### Restricted with warn
```bash
pod-security.kubernetes.io/warn: restricted
```
#### Restricted with enforce
```bash
pod-security.kubernetes.io/enforce: restricted
```
#### Baseline with warn
```bash
pod-security.kubernetes.io/warn: baseline
```
#### Baseline with enforce
```bash
pod-security.kubernetes.io/enforce: baseline
```
When using warn it allows pods that do not meet the policy requirements to run, but a user-facing warning will be generated. This can help identify and adjust pods or containers to comply with the policy.
### Some alternatives to Kubernetes' own Pod Security Admission (PSA):
- Kubewarden
- OPA Gatekeeper
- Kyverno

## ETC
`automountServiceAccountToken: False` Can be used if a pod in the kubernetes cluster do not need to talk to the k8s API. 