# Node Issues

[← Back to main index](../README.md)

8 scenario(s) in this category.

## 61. Node in NotReady State

**Symptom:** kubectl get nodes shows node as NotReady; pods on it not scheduling.

**Root Cause:** kubelet crashed/stopped, node network issue, or node is fully resource-exhausted.

**Fix:** SSH to node: systemctl status kubelet. Restart kubelet. Check /var/log/kubelet.log.

```bash
systemctl restart kubelet && journalctl -u kubelet -n 50
```

---

## 62. Node Under Memory Pressure

**Symptom:** Node shows MemoryPressure=True condition; pods being evicted.

**Root Cause:** Total memory consumed by pods exceeds eviction threshold.

**Fix:** Identify top consumers: kubectl top pods -n <ns>. Set memory limits. Add nodes.

```bash
kubectl top pods --all-namespaces --sort-by=memory | head -20
```

---

## 63. Node Under CPU Pressure

**Symptom:** Applications throttled; node CPU usage at 100%.

**Root Cause:** No CPU limits set; one workload consuming all CPU, throttling others.

**Fix:** Set CPU limits and requests. Use LimitRange to enforce defaults. Scale horizontally.

```bash
kubectl top nodes && kubectl top pods --all-namespaces --sort-by=cpu | head -20
```

---

## 64. DaemonSet Pod Not Scheduled on New Node

**Symptom:** New node added to cluster but DaemonSet pod not appearing on it.

**Root Cause:** New node has taint that DaemonSet doesn't tolerate, or nodeSelector doesn't match.

**Fix:** Add toleration for node taint in DaemonSet spec. Verify DaemonSet nodeSelector.

```bash
kubectl describe daemonset <name> -n <ns> | grep -i toleration
```

---

## 65. Node Cordoned — Pods Not Scheduling

**Symptom:** Pods stay Pending; describe shows node is cordoned (SchedulingDisabled).

**Root Cause:** Node was manually cordoned with kubectl cordon for maintenance and not uncordoned.

**Fix:** kubectl uncordon <node-name> to re-enable scheduling.

```bash
kubectl uncordon <node-name>
```

---

## 66. Node Disk I/O Saturation

**Symptom:** Node I/O wait high; pods experiencing slow disk reads/writes.

**Root Cause:** Multiple I/O-heavy workloads competing for disk, or underlying volume throughput limit hit.

**Fix:** Move I/O-intensive workloads to nodes with faster storage. Set I/O limits with cgroup blkio.

```bash
iostat -xz 1 && kubectl top nodes
```

---

## 67. Kubelet Certificate Expired

**Symptom:** Node shows NotReady; kubelet log shows certificate expired or x509 error.

**Root Cause:** TLS certificate used by kubelet expired and auto-renewal failed.

**Fix:** Renew certificates: kubeadm certs renew all. Restart kubelet and control plane components.

```bash
kubeadm certs check-expiration
```

---

## 68. Node Clock Skew Causing API Errors

**Symptom:** API server returns 401 errors; JWT validation fails; etcd clock skew warnings.

**Root Cause:** Node system clock drifted more than 5 minutes from API server time.

**Fix:** Sync NTP: timedatectl status. Configure chronyd or ntpd on all nodes.

```bash
timedatectl status && chronyc tracking
```

---
