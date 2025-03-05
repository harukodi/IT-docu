```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: wordpress-deployment
  name: mysql-configmap
data:
  WORDPRESS_DB_HOST: mysql-service
  WORDPRESS_DB_NAME: wordpress-db
```