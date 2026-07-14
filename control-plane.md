# Control Plane

[← Back to main index](../README.md)

7 scenario(s) in this category.

## 69. API Server Not Responding

**Symptom:** kubectl commands hang or return connection refused.

**Root Cause:** kube-apiserver pod crashed, etcd unreachable, or control plane node overloaded.

**Fix:** On control plane: check /var/log/kube-apiserver.log. Restart static pod. Check etcd health.

```bash
kubectl get --raw='/healthz' && crictl ps | grep apiserver
```

---

## 70. etcd High Latency / Slow Writes

**Symptom:** API server slow; etcd logs show slow heartbeats and leader election warnings.

**Root Cause:** Slow disk on etcd nodes, large etcd database, or too many watchers.

**Fix:** Use SSD for etcd data dir. Compact/defrag etcd. Reduce number of CRD instances.

```bash
etcdctl endpoint status --write-out=table -- endpoints=https://127.0.0.1:2379
```

---

## 71. Scheduler Not Placing Pods

**Symptom:** Pods stay Pending even with available nodes; no scheduling events in describe.

**Root Cause:** kube-scheduler crashed, misconfigured scheduling profile, or scheduler plugin conflict.

**Fix:** Check scheduler pod: kubectl get pods -n kube-system | grep scheduler. Review logs.

```bash
kubectl logs -n kube-system <kube-scheduler-pod>
```

---

## 72. Controller Manager Not Reconciling

**Symptom:** Deployments not creating pods; ReplicaSets not scaling; Jobs not starting.

**Root Cause:** kube-controller-manager is down or stuck in leader election.

**Fix:** kubectl get pods -n kube-system | grep controller-manager. Check logs for panic/errors.

```bash
kubectl logs -n kube-system kube-controller-manager-<node>
```

---

## 73. etcd Member Out of Quorum

**Symptom:** etcd cluster has no leader; API server returns etcd cluster is unavailable.

**Root Cause:** One or more etcd members crashed, reducing quorum below majority (need >n/2 healthy).

**Fix:** Restore crashed etcd member from snapshot. Or remove failed member and re-add.

```bash
etcdctl member list --endpoints=https://127.0.0.1:2379
```

---

## 74. Certificate Signing Request Stuck Pending

**Symptom:** New node cannot join; CSR shows Pending in kubectl get csr.

**Root Cause:** kube-controller-manager not approving CSRs (auto-approval disabled), or RBAC issue.

**Fix:** Manually approve: kubectl certificate approve <csr-name>. Or enable controller-based auto-approval.

```bash
kubectl get csr && kubectl certificate approve <name>
```

---

## 75. Admission Webhook Blocking Pod Creation

**Symptom:** Pod creation fails with admission webhook rejected or webhook timeout error.

**Root Cause:** Custom admission webhook returning reject, or webhook server unreachable causing timeout.

**Fix:** kubectl get validatingwebhookconfigurations. Check webhook service health. Set failurePolicy: Ignore temporarily.

```bash
kubectl get validatingwebhookconfigurations -o yaml
```

---
