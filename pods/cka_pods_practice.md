# Kubernetes Pods – CKA Practice Guide

## 1. Creating Pods

### 1.1 Imperative (Command-based) Approach

Create a pod directly via CLI:

```bash
kubectl run <pod-name> --image=<image-name>:<version> [--labels=key=value]
```

**Example:**

```bash
kubectl run nginx-pod --image=nginx:latest --labels=app=web
```

**Notes:**

* Labels are optional.
* Pods created this way **aren’t managed by controllers**, so you cannot scale, replicate, or automatically recover them.

### 1.2 Declarative (YAML-based) Approach

Create or apply pods via YAML files:

```bash
kubectl create -f <file.yaml>
kubectl apply -f <file.yaml>
```

**Example:**

```bash
kubectl create -f createpod.yaml
```

**Editing a running pod:**

```bash
kubectl edit pod <pod-name>
```

* Opens the pod YAML in your default editor (`vi` by default).
* Save changes with `:wq!`
* Exit without saving with `:q!`
* **No need to reapply** after editing using `kubectl edit pod`.

**Generating YAML for a pod (dry-run):**

```bash
kubectl run nginx-pod --image=nginx --dry-run=client -o yaml > podnew.yaml
```

**Export existing pod YAML:**

```bash
kubectl get pod nginx-pod -o yaml > getpod.yaml
```

## 2. Inspecting Pods

### 2.1 Describe Pod

Provides detailed information about pod status, events, node, containers, etc.

```bash
kubectl describe pod <pod-name>
```

**Example:**

```bash
kubectl describe pod nginx-pod
```

### 2.2 List Pods

Basic list of pods:

```bash
kubectl get pods
kubectl get pods -o wide  # shows node, IP, and other details
```

## 3. Debugging Pods

### 3.1 Exec into a Pod

```bash
kubectl exec -it <pod-name> -- sh
```

* Opens an interactive shell inside the container.

### 3.2 Deleting Pods

```bash
kubectl delete pod <pod-name>
kubectl delete pods --all  # deletes all pods in the current namespace
```

## 4. Notes / Tips

* **Imperative vs Declarative:**

  * Imperative: fast, temporary, CLI-driven.
  * Declarative: YAML-managed, better for version control and reproducibility.
* **Editing Pods:** changes made via `kubectl edit pod` are live and applied immediately.
* **YAML generation:** Use `--dry-run=client -o yaml` to generate YAML from commands for practice.
* Always check pod state with:

```bash
kubectl get pods
kubectl describe pod <pod-name>
```
