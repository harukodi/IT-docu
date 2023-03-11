## RBAC ClusterRole example file
>**NOTE** This example file gives access to all resources in the k8s cluster **BE CAUTIOUS**
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: admin-role #NAME SHOULD BE THE SAME
rules:
- apiGroups: [""]
  resources: ["*"]
  verbs: ["*"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-role-binding
subjects:
- kind: Group
  name: admin-group
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: admin-role #NAME SHOULD BE THE SAME
  apiGroup: rbac.authorization.k8s.io
```
> ## ClusterRole
> - Used to give cluster wide permissions over multiple namespaces in a k8s cluster

&nbsp;
&nbsp;

## RBAC Role example file
>**NOTE** This example file gives access to all resources in the k8s cluster **BE CAUTIOUS**
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: <NAMESPACE-NAME>
  name: admin-role #NAME SHOULD BE THE SAME
rules:
- apiGroups: ["*"]
  resources: ["*"]
  verbs: ["*"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: admin-role-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: admin-role #NAME SHOULD BE THE SAME
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: admin-group
```
> ## Role
> - Used to give permissions over a specific namespace in a k8s cluster