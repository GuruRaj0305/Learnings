# SSL / TLS Handling in Ingress (3 Types Only)

There are **only 3 valid SSL/TLS handling patterns** used with Ingress and Load Balancers.

---

## 1️⃣ SSL Pass-Through 🔐➡️🔐

**Ingress does NOT touch SSL at all**

### What happens
- Client sends **HTTPS**
- Ingress **forwards encrypted traffic as-is**
- Backend service **terminates SSL**

``` 
Client (HTTPS)
↓ encrypted
Ingress (just forwards)
↓ encrypted
Service / App (SSL ends here)
```


### Key points
- Ingress **cannot read HTTP headers**
- No routing by path (`/api`, `/user`)
- No auth, no rewrites, no WAF at ingress
- Backend must have **its own certificate**

### When to use
- You want **end-to-end encryption**
- App itself handles TLS (banking, compliance-heavy apps)
- You don’t need ingress-level features

### Pros
- ✅ True end-to-end security  
- ✅ App fully controls TLS  

### Cons
- ❌ No L7 routing  
- ❌ Harder cert management  
- ❌ Less observability  

---

## 2️⃣ SSL Termination / Offloading 🔓➡️📦

**Ingress decrypts SSL — backend gets HTTP**

### What happens
- Client sends **HTTPS**
- Ingress **terminates SSL**
- Ingress sends **HTTP** to service

```
Client (HTTPS)
↓ encrypted
Ingress (SSL ends here)
↓ HTTP
Service (no SSL)
```


### Key points
- SSL ends at ingress
- Backend traffic is **plain HTTP**
- Ingress can:
  - Route by path/host
  - Add authentication
  - Rate limit
  - Rewrite headers

### When to use
- Most **standard microservice setups**
- Traffic inside the cluster is trusted
- You want **simple services**

### Pros
- ✅ Easy certificate management  
- ✅ Full ingress features  
- ✅ Better performance  

### Cons
- ❌ No encryption inside the cluster  
- ❌ Not ideal for zero-trust environments  

---

## 3️⃣ SSL Bridge / Re-Encryption 🔐➡️🔓➡️🔐

**Ingress decrypts and encrypts again**

### What happens
- Client sends **HTTPS**
- Ingress **terminates SSL**
- Ingress creates **new SSL** to backend


```
Client (HTTPS)
↓ encrypted
Ingress (decrypt)
↓ re-encrypt
Service (HTTPS)
```



### Key points
- Two TLS connections:
  - Client → Ingress
  - Ingress → Service
- Ingress can inspect traffic
- Backend still receives HTTPS

### When to use
- **Zero-trust** clusters
- Compliance requires encryption everywhere
- You still want ingress-level features

### Pros
- ✅ Full ingress power  
- ✅ Encrypted internal traffic  
- ✅ Best security + flexibility  

### Cons
- ❌ More CPU usage  
- ❌ More certificates to manage  

---

## 🔥 Side-by-Side Comparison

| Feature | Pass-Through | Termination | Re-Encryption |
|-------|-------------|------------|--------------|
| SSL ends at Ingress | ❌ | ✅ | ✅ |
| SSL at Backend | ✅ | ❌ | ✅ |
| Path-based routing | ❌ | ✅ | ✅ |
| Internal encryption | ✅ | ❌ | ✅ |
| Complexity | High | Low | Medium |
| Most commonly used | ❌ | ✅✅ | ✅ |

---

## 🚀 Which one should you use?

**99% of applications → SSL Termination**

- **Pass-Through** → App must control TLS  
- **Termination** → Default, simple, efficient  
- **Re-Encryption** → Security + ingress features  



# OpenShift Routes + TLS Types

OpenShift **does NOT use Kubernetes Ingress by default**.  
Instead, it uses a resource called **`Route`**, handled by the **OpenShift Router (HAProxy)**.

So mentally replace:

> **Ingress Controller → OpenShift Router**

---

## Name Mapping (MOST IMPORTANT)

```
SSL Offloading == Edge Termination
SSL Bridge == Re-encrypt Termination
SSL Passthrough == Passthrough Termination
```


| General Term | OpenShift Term | Meaning |
|-------------|---------------|--------|
| SSL Termination / Offloading | **Edge** | TLS ends at router |
| SSL Re-encryption / Bridge | **Re-encrypt** | TLS ends + restarts |
| SSL Pass-through | **Passthrough** | TLS untouched |
