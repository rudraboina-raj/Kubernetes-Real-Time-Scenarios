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


# 🚀 Kubernetes Application Failures: Complete Guide

A practical guide to common Kubernetes failures, reasons, and troubleshooting steps used in real-world production.

---

## 🔴 1. Pod-Level Failures

### Common Issues

* **CrashLoopBackOff** → Application crash, missing ENV variables
* **Pending** → No nodes, insufficient CPU/memory, PVC issues
* **ImagePullBackOff** → Wrong image name, tag, or registry access

### 🔍 Troubleshooting

```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

---

## 🟠 2. ConfigMap & Environment Variable Errors

### Reasons

* Wrong key names
* Incorrect mount paths
* Missing ConfigMaps or Secrets

### 🛠 Fix

```bash
kubectl describe cm <name>
```

* Verify mounted files inside the pod
* Restart pods after config changes

---

## 🟡 3. Resource Failures

### Common Problems

* **OOMKilled (Out of Memory)**
* CPU throttling

### 🛠 Solution

* Increase memory/CPU limits
* Optimize application resource usage

```bash
kubectl top pod
```

---

## 🔵 4. Networking Issues

### Failures

* Service not reachable
* No endpoints
* Ingress misconfiguration

### 🔍 Checks

```bash
kubectl get svc
kubectl get endpoints
```

* Verify labels, ports, and ingress rules

---

## 🟣 5. Storage Failures

### Issues

* PVC not bound
* StorageClass missing
* Volume mount errors

### 🔍 Troubleshooting

```bash
kubectl describe pvc
```

* Verify StorageClass and access modes

---

## 🔴 6. Node & Cluster Issues

### Reasons

* Node NotReady
* Disk or memory pressure
* Kubelet issues

### 🛠 Fix

```bash
kubectl describe node
```

* Check node resource usage

---

## 🟠 7. Security & Policy Failures

### Problems

* RBAC permission denied
* NetworkPolicy blocking traffic

### 🔍 Checks

* RoleBindings & ServiceAccounts
* NetworkPolicy rules

---

## ⭐ Golden Troubleshooting Steps (Always Follow This Order)

```text
1. kubectl get pods
2. kubectl describe pod <pod-name>
3. kubectl logs <pod-name>
4. Check ConfigMaps & Secrets
5. Verify Services & Ingress
6. Check Node & Resources
7. Review Events & Logs
```

---

## ✅ Key Takeaway

* Most Kubernetes failures are **misconfigurations**, not bugs
* Strong troubleshooting skills = **faster recovery + stable systems**

---

## 📌 Tags

#Kubernetes #DevOps #CloudComputing #Containerization
#K8s #SRE #DevOpsEngineering #CloudNative
#KubernetesTroubleshooting #CKA #CKAD #Microservices

---
