# Pods

A Pod is the smallest deployable unit in Kubernetes. it basically a wrapper that kubernetes has created for a container to make the life of the devops Eng. easy.


## Key Characteristics

1. One or more containers
    + Usually 1 container per Pod.
    + Multiple containers can run in the same Pod if they need to share resources tightly.

2. Shared resources
    + Network: All containers in a Pod share the same IP address and port space.
    + Storage: Containers can access shared volumes.

3. Lifecycle
    + Pods are ephemeral: they can be created, destroyed, or replaced.
    + Kubernetes manages Pods via Controllers like Deployments, ReplicaSets.

## Why Pods?

+ Containers are lightweight, but Kubernetes needs a way to manage them as a unit.
+ Pods allow multiple containers to work together (e.g., helper container + main app container).
+ They provide networking, storage, and namespace isolation for containers.


### Steps to create pod 

* run `minikube start --memory=2048 --driver=kvm2` (this drive is for linux).
* run `kubectl get nodes` -> will show all running nodes
* create **name.yaml** file with 
  ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: my-first-pod
    spec:
    containers:
        - name: nginx-container
        image: nginx:latest
        ports:
            - containerPort: 80

  ``` 
* `kubectl create -f name.yaml` -> to create pods
* `kubectl get pods` -> to get all running pods
* `kubectl get pods -o wide` -> to get all the pods with ip address.
* `kubectl get pods <pod name >` -> to get specific pod.
* `kubectl delete pods <pod name >` -> to delete the pod.