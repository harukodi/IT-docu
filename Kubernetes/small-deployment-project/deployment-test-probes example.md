```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: n-labb
  name: small-app-v3
  labels:
    app: small-app-v3
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 10%
      maxSurge: 20%
  replicas: 3
  selector:
    matchLabels:
      app: small-app-v3
  template:
    metadata:
      labels:
        app: small-app-v3
    spec:
      containers:
        - name: small-app-v3
          image: xia1997x/pub:small-app-v3
          imagePullPolicy: Always
          ports:
            - containerPort: 8000
          livenessProbe:
            httpGet:
              path: /health
              port: 8000
              httpHeaders:
                - name: Health-Status
                  value: OK
            initialDelaySeconds: 8
            periodSeconds: 15
            failureThreshold: 1
          readinessProbe:
            httpGet:
              path: /health
              port: 8000
              httpHeaders:
                - name: Health-Status
                  value: OK
            initialDelaySeconds: 5
            periodSeconds: 0

```