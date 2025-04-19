```yaml
apiVersion: v1
kind: Service
metadata:
  name: small-app-svc
spec:
  type: ClusterIP
  selector:
    app: small-app
  ports:
  - protocol: TCP
    port: 8000
    targetPort: 8000
```