# Services

A **Service** in Kubernetes is an **abstraction that defines a logical set of Pods** and a **policy to access them**.  
It provides **stable networking** for Pods, even if the Pods are created or destroyed dynamically.  

---

## 🔹 Key Points

1. **Purpose**  
   - Pods are ephemeral and can be recreated anytime.  
   - A Service gives Pods a **stable IP address and DNS name**.  
   - It allows **load balancing** across multiple Pods.  

2. **Types of Services**  

| Type | Description |
|------|------------|
| **ClusterIP** | Default. Exposes the service on a cluster-internal IP. Only accessible inside the cluster. |
| **NodePort** | Exposes the service on each Node’s IP at a static port. Accessible externally. |
| **LoadBalancer** | Creates a cloud load balancer to expose service externally. Usually used in cloud environments. |
| **ExternalName** | Maps the service to an external DNS name. |

3. **Selectors**  
- Services use **labels and selectors** to group Pods.    
  > When a Pod is deleted and recreated, its IP address may change.
    A Service provides a stable endpoint (IP or DNS) and uses labels/selectors to route traffic to the correct Pods.
    This mechanism ensures that changes in Pod IPs do not affect communication and is called service discovery.

- Example: a Service selects all Pods with `app: nginx`.  

---

## Example Service YAML

```yaml
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
  type: ClusterIP
```



# Steps to Setup complete application 

+ Start minikube (in local machine) or KOps (in cloud service provider) 
+ Build the docker image which you want to run.
+ Create Deployment yaml file using example.
+ `kubectl apply -f auth-Deployment.yaml` -> Create Deployment.
+ `kubectl get deploy ` -> check this is running with all the instances.
+ for now wou can access that pods using `minikube ssh`.
+ 