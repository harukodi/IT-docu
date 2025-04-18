```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
 name: python-small-deploy
 labels:
   app: small-app
spec:
 replicas: 10
 revisionHistoryLimit: 5
 selector:
   matchLabels:
     app: small-app
 template:
   metadata:
     labels:
       app: small-app
   spec:
     containers:
     - name: small-app
       image: xia1997x/pub:small-app-v2
       imagePullPolicy: Always
       ports:
       - containerPort: 8000
 strategy:
   type: RollingUpdate
   rollingUpdate:
     maxSurge: 2
     maxUnavailable: 2
```