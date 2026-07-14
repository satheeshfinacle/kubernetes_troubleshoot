# Pod Issues

[← Back to main index](../README.md)

20 scenario(s) in this category.

## 1. Pod Stuck in Pending State

**Symptom:** Pod remains in Pending indefinitely with no node assignment.

**Root Cause:** Insufficient cluster resources (CPU/memory), node selector mismatch, or no nodes available.

**Fix:** kubectl describe pod <name> — check Events. Scale cluster or adjust resource requests. Verify nodeSelector/tolerations.

```bash
kubectl describe pod <pod-name> -n <namespace>
```

---

## 2. CrashLoopBackOff

**Symptom:** Pod starts, crashes, restarts in a loop with back-off delay.

**Root Cause:** Application error, missing env vars/config, misconfigured liveness probe, or OOMKilled.

**Fix:** Check logs with kubectl logs --previous. Fix app config, increase memory limits, or fix liveness probe thresholds.

```bash
kubectl logs <pod> --previous -n <ns>
```

---

## 3. OOMKilled — Pod Memory Limit Exceeded

**Symptom:** Pod terminates with exit code 137, reason OOMKilled.

**Root Cause:** Container exceeded its memory limit set in resources.limits.memory.

**Fix:** Increase memory limit or optimize application memory usage. Add HPA if load-driven.

```bash
kubectl describe pod <pod> | grep -i oom
```

---

## 4. ImagePullBackOff

**Symptom:** Pod shows ImagePullBackOff or ErrImagePull in status.

**Root Cause:** Wrong image name/tag, private registry missing imagePullSecrets, or registry unreachable.

**Fix:** Verify image exists. Create/attach imagePullSecret. Check network egress to registry.

```bash
kubectl describe pod <pod> | grep -A5 Events
```

---

## 5. Pod in Terminating State Forever

**Symptom:** Pod stays in Terminating for a very long time after deletion.

**Root Cause:** Finalizers not cleared, volume unmount hang, or unresponsive node.

**Fix:** Force-delete: kubectl delete pod --grace-period=0 --force. Check finalizers and node status.

```bash
kubectl delete pod <pod> --grace-period=0 --force -n <ns>
```

---

## 6. Init Container Failing

**Symptom:** Pod stuck in Init:0/1 or Init:CrashLoopBackOff.

**Root Cause:** Init container script fails — DB not ready, config missing, or wrong command.

**Fix:** kubectl logs <pod> -c <init-container-name>. Fix init logic or add retry/wait loops.

```bash
kubectl logs <pod> -c init-mycontainer -n <ns>
```

---

## 7. Container Exits with Code 1

**Symptom:** Container terminates immediately with exit code 1.

**Root Cause:** Application startup error — missing env variable, bad config file, or runtime exception.

**Fix:** kubectl logs to see stderr output. Fix application config or missing env vars.

```bash
kubectl logs <pod> -n <ns> --previous
```

---

## 8. Pod Not Matching Node Affinity

**Symptom:** Pod stays Pending; describe shows no nodes match affinity rules.

**Root Cause:** nodeAffinity requiredDuringSchedulingIgnoredDuringExecution rules don't match any node labels.

**Fix:** Check node labels: kubectl get nodes --show-labels. Fix affinity spec or label the correct node.

```bash
kubectl get nodes --show-labels | grep <label>
```

---

## 9. Pod Evicted Due to Disk Pressure

**Symptom:** Pod shows Evicted reason with message 'disk pressure'.

**Root Cause:** Node disk is full. Kubelet evicts pods when disk usage exceeds eviction threshold.

**Fix:** Free disk space — clean old images (crictl rmi --prune), remove logs. Add disk or add nodes.

```bash
kubectl describe node <node> | grep -i condition
```

---

## 10. Liveness Probe Failing

**Symptom:** Pod restarts periodically; describe shows 'Liveness probe failed'.

**Root Cause:** App takes too long to respond, probe path wrong, or app is genuinely unhealthy.

**Fix:** Adjust initialDelaySeconds, periodSeconds, failureThreshold. Verify probe endpoint is correct.

```bash
kubectl describe pod <pod> | grep -A10 Liveness
```

---

## 11. Readiness Probe Failing

**Symptom:** Pod is Running but not receiving traffic; endpoints list is empty.

**Root Cause:** Readiness probe returns non-200 or times out — app not yet ready or misconfigured probe.

**Fix:** Check probe path and port. Increase initialDelaySeconds. Verify app is listening on the probe port.

```bash
kubectl describe pod <pod> | grep -A10 Readiness
```

---

## 12. Pod Pending Due to Taint/Toleration Mismatch

**Symptom:** Describe shows: 0/3 nodes are available: 3 node(s) had taint.

**Root Cause:** All nodes have taints that the pod spec does not tolerate.

**Fix:** Add correct tolerations to pod spec or remove taint from target node.

```bash
kubectl describe node <node> | grep Taints
```

---

## 13. Multi-Container Pod — Wrong Container Targeted

**Symptom:** kubectl logs shows data from wrong container; exec targets wrong process.

**Root Cause:** Default container used when pod has multiple containers.

**Fix:** Always specify -c <container-name> in kubectl logs/exec commands.

```bash
kubectl logs <pod> -c <container-name> -n <ns>
```

---

## 14. Pod Running but Application Not Accessible

**Symptom:** Pod is Running/Ready but requests fail or connection refused.

**Root Cause:** App listening on wrong port, service port mismatch, or NetworkPolicy blocking traffic.

**Fix:** kubectl exec into pod and curl localhost:<port>. Fix containerPort, service targetPort.

```bash
kubectl exec -it <pod> -- curl localhost:8080
```

---

## 15. High Restart Count on Pod

**Symptom:** Pod shows 15+ restarts in kubectl get pods output.

**Root Cause:** Recurring crash — OOM, bad health probes, or intermittent application failure.

**Fix:** kubectl logs --previous for last crash reason. Profile memory/CPU, fix app stability.

```bash
kubectl get pods -n <ns> --sortby='.status.containerStatuses[0].restartCount'
```

---

## 16. ConfigMap Not Reflected in Pod

**Symptom:** Environment variable or mounted file has old value after ConfigMap update.

**Root Cause:** Pods cache ConfigMap values at startup; mounted files update eventually, env vars never auto-update.

**Fix:** Restart pod after ConfigMap change. For dynamic config, use projected volumes with rolling updates.

```bash
kubectl rollout restart deployment/<name> -n <ns>
```

---

## 17. Secret Not Mounted in Pod

**Symptom:** Application reports missing credential or file; volume mount empty.

**Root Cause:** Secret name mismatch, namespace mismatch, or secret key name typo in volumeMount.

**Fix:** kubectl describe pod — check volume mount errors. Verify secret exists in same namespace.

```bash
kubectl get secret <name> -n <ns> -o yaml
```

---

## 18. Pod Cannot Write to Volume

**Symptom:** Application errors: read-only file system or permission denied on mount path.

**Root Cause:** PVC mounted ReadOnlyMany mode, or securityContext.readOnlyRootFilesystem: true.

**Fix:** Check accessModes on PVC. Set readOnlyRootFilesystem: false or add emptyDir for write paths.

```bash
kubectl describe pvc <name> -n <ns>
```

---

## 19. Startup Probe Causing Early Termination

**Symptom:** Pod killed before app finishes startup; describe shows startupProbe failure.

**Root Cause:** failureThreshold * periodSeconds too short for slow-starting applications.

**Fix:** Increase failureThreshold to allow enough startup time (e.g., 30 * 10s = 5 min).

```bash
kubectl describe pod <pod> | grep -A10 Startup
```

---

## 20. Pod Scheduled on Wrong Node

**Symptom:** Pod lands on a node with wrong hardware/zone, causing performance issues.

**Root Cause:** Missing nodeAffinity, nodeSelector, or pod topology spread constraints.

**Fix:** Add nodeSelector or nodeAffinity rules. Use topologySpreadConstraints for zone distribution.

```bash
kubectl get pod <pod> -o wide
```

---
