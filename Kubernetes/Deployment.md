```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: target-namespace-for-the-app
  name: app-name
  labels:
    app: app-name
spec:
  replicas: amount-of-containers
  selector:
    matchLabels:
      app: app-name
  template:
    metadata:
      labels:
        app: app-name
    spec:
      containers:
      - name: app-name
        image: image-to-use ##Example nginx:latest
        ports:
        - containerPort: port-of-container
```
