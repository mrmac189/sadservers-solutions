https://sadservers.com/newserver/bilbao

There's a Kubernetes Deployment with an Nginx pod and a Load Balancer declared in the manifest.yml file. The pod is not coming up. Fix it so that you can access the Nginx container through the Load Balancer.

```bash
kubectl get deployment
kubectl describe deployment nginx-deployment
kubectl describe pod nginx-deployment-67699598cc-zrj6f
# Warning  FailedScheduling  344d   default-scheduler  0/2 nodes are available: 1 node(s) didn't match Pod's node affinity/selector, 1 node(s) had untolerated taint {node.kubernetes.io/unreachable: }. preemption: 0/2 nodes are available: 2 Preemption is not helpful for scheduling..
vim manifest.yml
```
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: localhost:5000/nginx
        ports:
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  clusterIP: 10.43.216.196
  type: LoadBalancer
```
```bash
kubectl apply -f manifest.yml
kubectl describe pod nginx-deployment-6f756d9c6c-4tqgp
curl 10.43.216.196
```