# Advanced

[← Back to main index](../README.md)

10 scenario(s) in this category.

## 91. CRD Version Mismatch After Upgrade

**Symptom:** Operator reconciliation fails; error: no kind CustomResource is registered.

**Root Cause:** CRD schema version not upgraded after operator upgrade; stored versions mismatch.

**Fix:** Re-apply updated CRD manifests. Run storage migration if stored version deprecated.

```bash
kubectl get crd <name> -o jsonpath='{.status.storedVersions}'
```

---

## 92. Cluster Autoscaler Not Removing Idle Nodes

**Symptom:** Empty nodes with no pods still running; cluster autoscaler not scaling down.

**Root Cause:** System pods or DaemonSets prevent scale-down, or scale-down-unneeded-time not elapsed.

**Fix:** Check CA logs. Annotate non-scalable pods. Verify cluster-autoscaler parameters.

```bash
kubectl logs -n kube-system -l app=cluster-autoscaler | grep scale-down
```

---

## 93. KEDA ScaledObject Not Triggering Scale

**Symptom:** Workload not scaling despite metric threshold crossed.

**Root Cause:** KEDA operator not running, trigger auth missing, or metric source unavailable.

**Fix:** kubectl describe scaledobject <name>. Check KEDA operator pod logs. Verify TriggerAuthentication.

```bash
kubectl get scaledobject -n <ns> && kubectl describe scaledobject <name>
```

---

## 94. Istio Sidecar Injection Not Working

**Symptom:** Pods not getting Envoy sidecar; mesh traffic control not applied.

**Root Cause:** Namespace label istio-injection: enabled missing, or pod annotation sidecar.istio.io/inject: false set.

**Fix:** kubectl label namespace <ns> istio-injection=enabled. Re-create pods.

```bash
kubectl label namespace <ns> istio-injection=enabled
```

---

## 95. Velero Backup Failing

**Symptom:** Velero backup job shows Failed status; PVC snapshots missing.

**Root Cause:** Cloud snapshot permissions missing, Velero plugin not installed, or backup storage location unavailable.

**Fix:** Check velero describe backup. Verify IAM permissions for volume snapshot. Check BSL status.

```bash
velero backup describe <backup-name> --details
```

---

## 96. PodDisruptionBudget Blocking All Node Drains

**Symptom:** kubectl drain <node> hangs; cannot evict pod as it would violate PDB.

**Root Cause:** PDB minAvailable equals total replicas; no replica can be evicted.

**Fix:** Temporarily scale up deployment before drain. Or patch PDB. Plan PDB to allow at least 1 eviction.

```bash
kubectl get pdb -n <ns> && kubectl drain <node> --ignore-daemonsets
```

---

## 97. Vertical Pod Autoscaler Causing Restart Loop

**Symptom:** VPA keeps restarting pods to apply new resource recommendations.

**Root Cause:** VPA in Auto mode continuously updating limits; app takes time to stabilize.

**Fix:** Use VPA in Off or Initial mode first to observe recommendations without forced restarts.

```bash
kubectl describe vpa <name> -n <ns>
```

---

## 98. Namespace Stuck in Terminating

**Symptom:** kubectl delete namespace <ns> runs but namespace stays Terminating forever.

**Root Cause:** Finalizers on resources inside namespace not cleared, or API service unavailable.

**Fix:** Find stuck resources: kubectl api-resources --verbs=list -o name | xargs -I{} kubectl get {} -n <ns>. Patch finalizers.

```bash
kubectl get namespace <ns> -o json | jq '.spec.finalizers=[]' | kubectl replace --raw /api/v1/namespaces/<ns>/finalize -f -
```

---

## 99. Resource Quota Blocking New Deployments

**Symptom:** Pod creation denied: exceeded quota: requests.cpu, must be less than or equal to.

**Root Cause:** Namespace ResourceQuota exhausted; no more CPU/memory can be allocated.

**Fix:** Increase quota or reduce existing resource requests. Clean up unused workloads.

```bash
kubectl describe resourcequota -n <ns>
```

---

## 100. Job Pods Not Cleaning Up After Completion

**Symptom:** Completed job pods accumulating; cluster full of Completed pods.

**Root Cause:** ttlSecondsAfterFinished not set on Job spec.

**Fix:** Set ttlSecondsAfterFinished: 300 on Job spec. For existing jobs, delete manually.

```bash
kubectl delete pods --field-selector=status.phase==Succeeded -n <ns>
```

---
