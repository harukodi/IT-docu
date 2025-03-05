```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: wordpress-deployment
  name: wordpress-deployment
  labels:
    app: wordpress-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress-deployment
  template:
    metadata:
      labels:
        app: wordpress-deployment
    spec:
      containers:
      - name: wordpress-deployment
        image: wordpress
        env:
        - name: WORDPRESS_DB_HOST
          valueFrom:
            configMapKeyRef:
              name: mysql-configmap
              key: WORDPRESS_DB_HOST
        - name: WORDPRESS_DB_NAME
          valueFrom:
            configMapKeyRef:
              name: mysql-configmap
              key: WORDPRESS_DB_NAME
        - name: WORDPRESS_DB_USER
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: MYSQL_USER
        - name: WORDPRESS_DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: MYSQL_PASSWORD
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: wordpress-service
spec:
  selector:
    app: wordpress-deployment
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: wordpress-service-nodeport
spec:
  type: NodePort
  selector:
    app: wordpress-deployment
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30001
```