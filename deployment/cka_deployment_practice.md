# Kubernetes Deployments – CKA Practice Guide

---

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

1.2 Apply YAML
kubectl apply -f deployment.yaml


Note: -o wide is for get, not create. Use only -o yaml for file generation.



2. Scaling Deployments

kubectl scale deployment <deployment-name> --replicas=4


3. Updating Container Image
kubectl set image deployment/<deployment-name> \
  <container-name>=<image>:<new-tag> --record


Notes:

--record → saves the command in rollout history.

<container-name> is the name defined in the deployment spec, not necessarily the deployment name.



4. Rollout Management
4.1 Status

kubectl rollout status deployment/<deployment-name>


4.2 History

kubectl rollout history deployment/<deployment-name>


4.3 Undo (Rollback)

# To previous revision
kubectl rollout undo deployment/<deployment-name>

# To specific revision
kubectl rollout undo deployment/<deployment-name> --to-revision=1






5. One-Liner Cheat Sheet



# Create + Generate YAML
kubectl create deployment myapp --image=nginx:1.21 --replicas=3 --dry-run=client -o yaml > myapp.yaml

# Apply YAML
kubectl apply -f myapp.yaml

# Scale
kubectl scale deployment myapp --replicas=5

# Update Image
kubectl set image deployment/myapp nginx=nginx:1.22 --record

# Check Rollout Status
kubectl rollout status deployment/myapp

# View Rollout History
kubectl rollout history deployment/myapp

# Rollback
kubectl rollout undo deployment/myapp

# Rollback to specific revision
kubectl rollout undo deployment/myapp --to-revision=1






















TipCommand / RuleNever use kubectl run for DeploymentsDeprecated in CKAFastest creationcreate ... -o yaml > file.yamlAlways use --recordFor audit & rollbackRollback = 1 commandrollout undoCheck before leavingrollout status