# Deployment Issues

[← Back to main index](../README.md)

10 scenario(s) in this category.

## 21. Deployment Rollout Stuck

**Symptom:** kubectl rollout status shows Waiting for deployment rollout to finish.

**Root Cause:** New pod fails health checks, insufficient replicas available, or resource quota hit.

**Fix:** kubectl rollout status -w. Check describe on new pods. Roll back if needed.

```bash
kubectl rollout undo deployment/<name> -n <ns>
```

---

## 22. Deployment Not Scaling

**Symptom:** Replica count in spec is increased but pod count does not change.

**Root Cause:** ResourceQuota exceeded, node capacity insufficient, or HPA conflict with manual scale.

**Fix:** Check quota: kubectl describe quota. Check node capacity. Disable HPA if manual scaling needed.

```bash
kubectl describe resourcequota -n <ns>
```

---

## 23. Rolling Update Causes Downtime

**Symptom:** During update, service has zero available pods briefly.

**Root Cause:** maxUnavailable set too high relative to replica count, or readiness probe not configured.

**Fix:** Set maxUnavailable: 0, maxSurge: 1. Ensure readiness probes are correct so old pods only terminate after new ones are ready.

```bash
kubectl patch deployment <name> -p '{"spec":{"strategy":{"rollingUpdate":{"maxUnavailable":0}}}}'
```

---

## 24. ReplicaSet Pods Not Starting After Image Update

**Symptom:** New ReplicaSet created but pods fail to start after image change.

**Root Cause:** New image has a bug, wrong tag used (e.g., latest pointing to broken image), or registry auth issue.

**Fix:** Pin image tags. Check new pod logs. Roll back: kubectl rollout undo.

```bash
kubectl rollout history deployment/<name>
```

---

## 25. Deployment Shows 0/3 Ready

**Symptom:** All pods crash immediately; deployment shows 0 ready replicas.

**Root Cause:** Fatal application error in new version, bad env config, or missing secret/configmap.

**Fix:** Check pod events and logs. Verify all referenced Secrets and ConfigMaps exist.

```bash
kubectl get events -n <ns> --sort-by='.lastTimestamp'
```

---

## 26. HPA Not Scaling Deployment

**Symptom:** CPU usage high but HPA does not increase replicas.

**Root Cause:** Metrics server not installed, metrics.k8s.io API unavailable, or resource requests not set.

**Fix:** Install metrics-server. Set resource requests on containers. Check HPA describe for conditions.

```bash
kubectl describe hpa <name> -n <ns>
```

---

## 27. Old Pods Not Terminating During Update

**Symptom:** Both old and new pods running for an extended time during rollout.

**Root Cause:** Old pods have long terminationGracePeriodSeconds or preStop hook taking too long.

**Fix:** Reduce terminationGracePeriodSeconds. Fix preStop hook. Ensure SIGTERM is handled by app.

```bash
kubectl describe pod <old-pod> | grep TerminationGrace
```

---

## 28. Deployment Revision History Full

**Symptom:** Cannot rollback; error: no revision history found or RevisionHistoryLimit exhausted.

**Root Cause:** revisionHistoryLimit set to 0 or very low — old ReplicaSets pruned.

**Fix:** Set revisionHistoryLimit: 10 in deployment spec to retain rollback history.

```bash
kubectl patch deployment <name> -p '{"spec":{"revisionHistoryLimit":10}}'
```

---

## 29. Deployment Stuck After Node Drain

**Symptom:** Pod stays Pending after node drain with PodDisruptionBudget violated.

**Root Cause:** PDB minAvailable requires more pods than the cluster can currently schedule.

**Fix:** Add nodes or temporarily patch PDB. Check: kubectl get pdb -n <ns>.

```bash
kubectl describe pdb <name> -n <ns>
```

---

## 30. Environment Variable Not Available in Pod

**Symptom:** App fails to read env var; echo $VAR inside pod returns empty.

**Root Cause:** Typo in env name in deployment spec, or valueFrom referencing non-existent Secret/ConfigMap key.

**Fix:** kubectl exec into pod: env | grep VAR. Verify deployment env section and referenced resource.

```bash
kubectl exec <pod> -- env | grep MY_VAR
```

---
