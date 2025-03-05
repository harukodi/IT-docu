```yaml
apiVersion: v1
kind: Secret
metadata:
  namespace: wordpress-deployment
  name: mysql-secret
type: Opaque
data:
  MYSQL_USER: redacted
  MYSQL_PASSWORD: redacted
```