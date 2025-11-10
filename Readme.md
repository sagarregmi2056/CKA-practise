# 🧑‍💻 CKA Exam Practice Repository

This repository is designed for **practical hands-on practice** for the **Certified Kubernetes Administrator (CKA)** exam. It contains YAML files, scripts, and notes covering all key domains. Use this README as a guide for your study workflow.

---

## 📌 Repository Structure (Recommended)

```
cka-practice/
├── README.md                  # This guide
├── pods/                      # Pod creation and management exercises
├── deployments/               # Deployments, ReplicaSets
├── services/                  # Services (ClusterIP, NodePort, LoadBalancer)
├── configmaps-secrets/        # ConfigMaps & Secrets practice
├── volumes-storage/           # PersistentVolumes, PersistentVolumeClaims, StorageClasses
├── networking/                # NetworkPolicies, Ingress
├── rbac/                      # Users, Roles, RoleBindings
├── troubleshooting/           # Debugging & cluster maintenance exercises
└── manifests/                 # Utility YAMLs for quick testing
```

---

## 1️⃣ Pods – Core Practice

### 1.1 Create Pods

#### Imperative

```bash
kubectl run <pod-name> --image=<image-name>:<version> [--labels=key=value]
kubectl run nginx-pod --image=nginx:latest --labels=app=web
```

* Labels optional
* Pods created this way **aren’t managed by controllers** (no scaling/replication)

#### Declarative

```bash
kubectl create -f <file.yaml>
kubectl apply -f <file.yaml>
```

* Example YAML: `pods/createpod.yaml`
* Edit live pod:

```bash
kubectl edit pod <pod-name>
```

* Generate YAML from command (dry-run):

```bash
kubectl run nginx-pod --image=nginx --dry-run=client -o yaml > podnew.yaml
```

* Export existing pod YAML:

```bash
kubectl get pod nginx-pod -o yaml > getpod.yaml
```

### 1.2 Inspect & Debug Pods

```bash
kubectl get pods
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl exec -it <pod-name> -- sh
kubectl delete pod <pod-name>
kubectl delete pods --all
```

---

## 2️⃣ Deployments & ReplicaSets

* Create a deployment declaratively:

```bash
kubectl create -f deployments/nginx-deployment.yaml
kubectl apply -f deployments/nginx-deployment.yaml
```

* Scale:

```bash
kubectl scale deployment nginx-deployment --replicas=3
```

* Rollout status & history:

```bash
kubectl rollout status deployment/nginx-deployment
kubectl rollout history deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment
```

---

## 3️⃣ Services & Networking

* Expose pods or deployments:

```bash
kubectl expose pod nginx-pod --type=NodePort --port=80
kubectl expose deployment nginx-deployment --type=ClusterIP --port=80
```

* Check services:

```bash
kubectl get svc
kubectl describe svc <service-name>
```

* Network policies: create restrictive ingress/egress rules in `networking/` folder

---

## 4️⃣ ConfigMaps & Secrets

* Create ConfigMap:

```bash
kubectl create configmap app-config --from-file=config.properties
```

* Create Secret:

```bash
kubectl create secret generic db-secret --from-literal=password='mypassword'
```

* Mount ConfigMap/Secret in Pod via `spec.containers[].envFrom` or `volumeMounts`

---

## 5️⃣ Volumes & Storage

* PersistentVolume (PV) + PersistentVolumeClaim (PVC)

```bash
kubectl create -f volumes-storage/pv.yaml
kubectl create -f volumes-storage/pvc.yaml
kubectl get pv,pvc
```

* StorageClass usage for dynamic provisioning

---

## 6️⃣ RBAC & Access Control

* Create Roles and RoleBindings:

```bash
kubectl create -f rbac/role.yaml
kubectl create -f rbac/rolebinding.yaml
kubectl auth can-i <action> <resource> --as <user>
```

* Practice `ClusterRole` and `ClusterRoleBinding`

---

## 7️⃣ Debugging & Maintenance

* Check node status:

```bash
kubectl get nodes
kubectl describe node <node-name>
```

* Check pod logs:

```bash
kubectl logs <pod-name>
kubectl logs -f <pod-name>
```

* Access cluster shell:

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

---

## 8️⃣ Tips for CKA Practice

1. **Hands-on > Theory** — always create, delete, edit, scale resources manually.
2. **Use dry-run & YAML generation** for practice:

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
```

3. **Namespaces** — practice switching and creating namespaces:

```bash
kubectl create namespace test
kubectl get pods -n test
```

4. **Shortcuts**:

```bash
kubectl get all
kubectl get pods -o wide
kubectl delete pods --all
```

5. **Backup YAMLs** — save working configurations to reuse during practice.

---

This README serves as the **central guide for your CKA repo**, and each folder contains examples & exercises to reinforce learning.
