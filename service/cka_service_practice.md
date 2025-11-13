

## 1. Creating Deployments

### 1.1 Imperative (Command-based) – Dry-run + YAML

```bash
# With replicas
kubectl create deployment <deployment-name> \
  --image=<image>:<tag> \
  --replicas=3 \
  --dry-run=client -o yaml > deployment.yaml

# Without replicas (default: 1)
kubectl create deployment <deployment-name> \
  --image=<image>:<tag> \
  --dry-run=client -o yaml > deployment.yaml
```

> **Note:** `-o wide` is for `kubectl get`, not `create`. Use only `-o yaml` for generating YAML files.

### 1.2 Apply YAML

```bash
kubectl apply -f deployment.yaml
```

---

## 2. Scaling Deployments

```bash
kubectl scale deployment <deployment-name> --replicas=4
```

---

## 3. Updating Container Image

```bash
kubectl set image deployment/<deployment-name> \
  <container-name>=<image>:<new-tag> --record
```

**Notes:**

* `--record` → saves the command in rollout history
* `<container-name>` is the container name in the deployment spec, not necessarily the deployment name

---

## 4. Rollout Management

### 4.1 Check Status

```bash
kubectl rollout status deployment/<deployment-name>
```

### 4.2 View History

```bash
kubectl rollout history deployment/<deployment-name>
```

### 4.3 Undo (Rollback)

```bash
# Rollback to previous revision
kubectl rollout undo deployment/<deployment-name>

# Rollback to specific revision
kubectl rollout undo deployment/<deployment-name> --to-revision=1
```

---

## 5. Exposing Services

### 5.1 Imperative Service Creation

```bash
# Create ClusterIP service (default)
kubectl expose deployment <deployment-name> --port=80 --target-port=80 --name=<service-name>

# Create NodePort service
kubectl expose deployment <deployment-name> --type=NodePort --port=80 --target-port=80 --name=<service-name>

# Create LoadBalancer service (cloud only)
kubectl expose deployment <deployment-name> --type=LoadBalancer --port=80 --target-port=80 --name=<service-name>
```

### 5.2 Service YAML (Declarative)

```bash
kubectl expose deployment <deployment-name> --port=80 --target-port=80 --name=<service-name> --dry-run=client -o yaml > service.yaml
kubectl apply -f service.yaml
```

### 5.3 Verify Service

```bash
kubectl get svc <service-name>
kubectl describe svc <service-name>
```

---

## 6. One-Liner Cheat Sheet

```bash
# Deployment
kubectl create deployment myapp --image=nginx:1.21 --replicas=3 --dry-run=client -o yaml > myapp.yaml
kubectl apply -f myapp.yaml
kubectl scale deployment myapp --replicas=5
kubectl set image deployment/myapp nginx=nginx:1.22 --record
kubectl rollout status deployment/myapp
kubectl rollout history deployment/myapp
kubectl rollout undo deployment/myapp
kubectl rollout undo deployment/myapp --to-revision=1

# Service
kubectl expose deployment myapp --port=80 --target-port=80 --name=myservice
kubectl get svc myservice
kubectl describe svc myservice
```

---

## 7. Tips & Rules

* **Never use `kubectl run` for Deployments** – deprecated in CKA
* **Fastest creation:** `kubectl create ... -o yaml > file.yaml`
* **Always use `--record`** for audit & rollback
* **Rollback = 1 command** (`rollout undo`)
* **Check rollout status** before leaving (`kubectl rollout status`)
* **Expose services** to connect deployments and check endpoints
                      ┌─────────────────────────────┐
                      │        Client / Browser     │
                      └───────────────┬─────────────┘
                                      │ External Access
                                      │
                        ┌─────────────▼─────────────┐
                        │     NodePort / LoadBalancer│
                        │   (NodeIP:NodePort / LB IP)│
                        └─────────────┬─────────────┘
                                      │ Routes traffic to
                                      ▼
           ┌─────────────────────────────────────────────┐
           │ Kubernetes Cluster (Internal Network)        │
           │                                             │
           │  ┌───────────────┐        ┌───────────────┐ │
           │  │ Service (80)  │        │ Service (5432)│ │
           │  │ Type: NodePort│        │ Type: ClusterIP│ │
           │  └───────┬───────┘        └───────┬───────┘ │
           │          │                        │         │
           │          ▼                        ▼         │
           │  ┌───────────────┐        ┌───────────────┐ │
           │  │ Pod1          │        │ DB Pod1       │ │
           │  │ App: Node.js  │        │ PostgreSQL    │ │
           │  │ Port: 8080    │        │ Port: 5432   │ │
           │  └───────────────┘        └───────────────┘ │
           │  ┌───────────────┐        ┌───────────────┐ │
           │  │ Pod2          │        │ DB Pod2       │ │
           │  │ App: Node.js  │        │ PostgreSQL    │ │
           │  │ Port: 8080    │        │ Port: 5432   │ │
           │  └───────────────┘        └───────────────┘ │
           │                                             │
           └─────────────────────────────────────────────┘

Legend:
- NodePort / LoadBalancer → External access to web apps
- ClusterIP → Internal access only (DBs, internal APIs)
- Service port → Port other pods or external clients use
- TargetPort → Container port inside pods
# Kubernetes Deployments & Services – CKA Practice Guide
