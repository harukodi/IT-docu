```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: wordpress-deployment
  name: mysql-deployment
  labels:
    app: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql-deployment
  template:
    metadata:
      labels:
        app: mysql-deployment
    spec:
      containers:
      - name: mysql-deployment
        image: mysql:8.0
        env:
        - name: MYSQL_DATABASE
          valueFrom:
            configMapKeyRef:
              name: mysql-configmap
              key: WORDPRESS_DB_NAME
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: MYSQL_USER
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: MYSQL_PASSWORD
        - name: MYSQL_RANDOM_ROOT_PASSWORD
          value: "1"
        ports:
        - containerPort: 3306
---
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  selector:
    app: mysql-deployment
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
```