# Ingress, Routs and Ingress controllers

Ingress exposes HTTP and HTTPS routes from outside the cluster to services with in the cluster. Traffic routing is controlled by rules defined on the ingress resource.


Ingress has two parts:

1. Ingress controller
    YAML file that defines:
    + Domain
    + Path
    + Which service to route to

2. Ingress Controller (actual worker)
   A running component that:
   + Reads Ingress rules
   + Configures a real proxy

    Common controllers:

    + NGINX Ingress
    + AWS ALB Ingress 
    + Traefik 
    + HAProxy


```
Browser
  ↓
Domain (example.com)
  ↓
LoadBalancer / NodePort
  ↓
Ingress Controller (NGINX / ALB)
  ↓
Ingress Rules
  ↓
Service
  ↓
Pods
```

### Basic Ingress example (simple)

``` yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80

```

```yaml
# Path-based routing
rules:
- host: example.com
  http:
    paths:
    - path: /auth
      pathType: Prefix
      backend:
        service:
          name: auth-service
          port:
            number: 80

    - path: /payments
      pathType: Prefix
      backend:
        service:
          name: payment-service
          port:
            number: 80
```

``` yaml
# Host-based routing
rules:
- host: api.example.com
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
          name: api-service
          port:
            number: 80

- host: admin.example.com
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
          name: admin-service
          port:
            number: 80

```
