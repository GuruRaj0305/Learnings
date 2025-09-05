# Deployment

A Deployment is a higher-level Kubernetes object that manages Pods and ReplicaSets. It ensures your application is always running in the desired state.

Deployment is just like a manager for Pods — it handles creation, updates, scaling, and rollback automatically.

---

## Key features
+ Declarative Updates
+ Auto Healing
+ Scaling
+ Rolling Updates
+ Rolling Updates


## How Deployments Work
+ You create a Deployment specifying the **desired state** (container image, number of replicas, labels).
+ Kubernetes creates a **ReplicaSet**, which in turn creates the desired number of Pods.
+ If Pods fail, the ReplicaSet ensures **new Pods are started**.
+ Updates to the Deployment trigger **rolling updates** of Pods, one at a time (configurable).

## Example Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
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
        image: nginx:1.25
        ports:
        - containerPort: 80

```

## commands
+ `kubectl get deploy` -> to see all the deploys
+ `kubectl delete delopy <name_of_deploy>` -> to delete
+ `kubectl apply -f <path to that yaml file>` -> create deployment
+ `kubectl get rs` -> to see all the replica sets
+ `kubectl get all` -> to see everything (deploy, nodes, pods, services ...)
+ `kubectl get pods -w` -> to watch the activities.