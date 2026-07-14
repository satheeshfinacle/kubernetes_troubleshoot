# Storage

[← Back to main index](../README.md)

10 scenario(s) in this category.

## 46. PVC Stuck in Pending

**Symptom:** PersistentVolumeClaim stays Pending; no PV bound.

**Root Cause:** No StorageClass available, provisioner not running, or access mode mismatch.

**Fix:** kubectl describe pvc. Check StorageClass exists. Verify provisioner pod is running.

```bash
kubectl get storageclass && kubectl describe pvc <name> -n <ns>
```

---

## 47. PV Not Releasing After PVC Deletion

**Symptom:** PersistentVolume stuck in Released state; cannot be rebound.

**Root Cause:** PV reclaim policy is Retain — manual intervention required to rebind.

**Fix:** Edit PV: remove claimRef to make it Available, or delete and re-create PV.

```bash
kubectl patch pv <pv-name> -p '{"spec":{"claimRef":null}}'
```

---

## 48. Volume Mount Permission Denied

**Symptom:** Application gets permission denied accessing mounted volume path.

**Root Cause:** fsGroup/runAsUser not set; mounted volume owned by root, app runs as non-root user.

**Fix:** Set securityContext.fsGroup to GID that owns the files, or use initContainer to chmod the mount.

```bash
kubectl exec <pod> -- ls -la /data
```

---

## 49. EBS Volume Stuck Attaching on AWS EKS

**Symptom:** Pod pending with AttachVolume.Attach failed; volume stuck attaching.

**Root Cause:** Volume still attached to previous node (not detached cleanly), or AZ mismatch between pod and EBS volume.

**Fix:** Manually detach EBS in AWS console. Ensure PV and node are in same AZ using topologyKey.

```bash
kubectl describe pod <pod> | grep -A5 AttachVolume
```

---

## 50. StatefulSet Pod Can't Find PVC

**Symptom:** StatefulSet pod starts but crashes with volume not found error.

**Root Cause:** VolumeClaimTemplate name changed, PVC manually deleted, or namespace mismatch.

**Fix:** Restore PVC with exact naming convention: <template-name>-<statefulset-name>- <ordinal>.

```bash
kubectl get pvc -n <ns> | grep <statefulset-name>
```

---

## 51. CSI Driver Not Installed

**Symptom:** PVC Pending with error: no plugin found for CSI driver.

**Root Cause:** CSI driver (EBS CSI, EFS CSI, etc.) not installed on cluster.

**Fix:** Install the CSI driver via Helm or manifests. Verify DaemonSet pods are running.

```bash
kubectl get pods -n kube-system | grep csi
```

---

## 52. Disk Full on Node Causing Pod Eviction

**Symptom:** Multiple pods evicted; node shows DiskPressure condition.

**Root Cause:** Logs, images, or application writes filling node disk.

**Fix:** Clean images: crictl rmi --prune. Set log rotation. Add log size limits to container spec.

```bash
crictl rmi --prune && df -h
```

---

## 53. ReadWriteMany PVC Not Supported

**Symptom:** Deployment with multiple replicas fails to start all pods; PVC can only bind once.

**Root Cause:** StorageClass (e.g., EBS gp2) only supports ReadWriteOnce access mode.

**Fix:** Use NFS or EFS-based StorageClass that supports ReadWriteMany. Or use one pod writing to shared storage.

```bash
kubectl get storageclass -o yaml | grep allowVolumeExpansion
```

---

## 54. Volume Expansion Not Taking Effect

**Symptom:** PVC size patched to larger value but filesystem inside pod still shows old size.

**Root Cause:** Volume expanded at PV level but filesystem inside pod not resized.

**Fix:** Restart pod to trigger online resize, or mount with allowVolumeExpansion: true in StorageClass.

```bash
kubectl patch pvc <name> -p '{"spec":{"resources":{"requests":{"storage":"20Gi"}}}}'
```

---

## 55. emptyDir Volume Data Lost After Pod Restart

**Symptom:** Application data lost after pod restart; files written to emptyDir are gone.

**Root Cause:** emptyDir is ephemeral and tied to pod lifecycle — data is wiped on pod termination.

**Fix:** Use PVC for persistent data. emptyDir is only for scratch space or inter-container sharing.

```bash
kubectl describe pod <pod> | grep -A5 Volumes
```

---
