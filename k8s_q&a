# 🚀 Kubernetes Real-Time Scenarios (Interview Guide)

A quick reference for common Kubernetes production issues with debugging steps and fixes.

---

## 🚨 Scenario 1: Pod in CrashLoopBackOff

**Q:** A pod keeps restarting and shows `CrashLoopBackOff`. What will you do?

### 🔍 Debug Steps

```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl logs <pod-name> -c <container-name>
```

### ⚠️ Common Causes

* Wrong command/arguments
* Missing environment variables
* ConfigMap/Secret issues

### 🛠 Fix

```yaml
restartPolicy: Never
```

---

## 🚨 Scenario 2: Pod Stuck in Pending

**Q:** A pod is not starting and stuck in `Pending`. Why?

### ⚠️ Common Causes

* Insufficient CPU/Memory
* Node selector / taints mismatch
* PVC not bound

### 🔍 Debug

```bash
kubectl describe pod <pod-name>
```

### 📌 Typical Errors

* `0/5 nodes available: insufficient memory`
* `pod has unbound immediate PersistentVolumeClaims`

### 🛠 Fix

* Increase cluster capacity
* Adjust resource requests
* Fix StorageClass / create PV

---

## 🚨 Scenario 3: Service Not Accessible

**Q:** Pods are running but Service is not reachable.

### 🔍 Debug Steps

```bash
kubectl get svc
kubectl describe svc <service-name>
kubectl get pods --show-labels
kubectl get endpoints <service-name>
```

### ⚠️ Common Issue

* Selector mismatch → No endpoints

### 🧠 Example

```
app: web   ❌
app: web-app ✅
```

---

## 🚨 Scenario 4: App Works Inside Pod but Not Outside

**Q:** App responds inside pod but not externally.

### 🔍 Check

* Service Type:

  * `ClusterIP` → internal only
  * `NodePort` / `LoadBalancer` → external

### 🔍 Ingress Debug

```bash
kubectl get ingress
kubectl describe ingress <ingress-name>
```

### ⚠️ Common Issue

* DNS not mapped to Ingress hostname

---

## 🚨 Scenario 5: High CPU / Memory Usage (OOMKilled)

**Q:** Pods are getting `OOMKilled`.

### 🔍 Debug

```bash
kubectl describe pod <pod-name>
```

### 📌 Look For

* `Last State: Terminated`
* `Reason: OOMKilled`

### 🛠 Fix

* Increase memory limits
* Optimize application

---

## 🚨 Scenario 6: Rolling Update Causes Downtime

**Q:** During deployment update, users see downtime.

### 🔍 Check Strategy

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```

### 🛠 Fix

* Ensure `maxUnavailable: 0`
* Maintain extra pod during update

---

## 🚨 Scenario 7: ConfigMap Not Updated in Pods

**Q:** Updated ConfigMap but app still uses old config.

### ⚠️ Reason

* ConfigMaps are not hot-reloaded

### 🛠 Fix

```bash
kubectl rollout restart deployment <deployment-name>
```

---

## ✅ Quick Debug Flow (Interview Tip)

```
1. kubectl get pods
2. kubectl describe pod
3. kubectl logs
4. Check Service & Endpoints
5. Check Node / Resources / Storage
```

---

## ⭐ Pro Tips

* Always check **Events section** in `describe`
* Most issues come from:

  * Label mismatch
  * Resource limits
  * Storage problems
* Think in layers:

  * Pod → Service → Ingress → Node → Cluster

---
