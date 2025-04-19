```yaml
apiVersion: v1
kind: Service
metadata:
  name: python-small-deploy-lb
spec:
  type: LoadBalancer
  selector:
    app: small-app
  ports:
  - name: http
    protocol: TCP
    port: 8080
    targetPort: 8000
  loadBalancerSourceRanges:
    - 0.0.0.0/0
```