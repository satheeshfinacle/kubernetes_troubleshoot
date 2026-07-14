# Networking

[← Back to main index](../README.md)

15 scenario(s) in this category.

## 31. Service Not Routing to Pods

**Symptom:** Requests to ClusterIP service return connection refused or timeout.

**Root Cause:** Service selector doesn't match pod labels, wrong targetPort, or pods not Ready.

**Fix:** kubectl get endpoints <svc>. If empty, fix selector. Verify pod labels match.

```bash
kubectl get endpoints <svc> -n <ns>
```

---

## 32. DNS Resolution Failing Inside Pod

**Symptom:** curl http://my-service returns name resolution failure from inside pod.

**Root Cause:** CoreDNS pod not running, NDOTS misconfiguration, or NetworkPolicy blocking DNS (port 53).

**Fix:** kubectl get pods -n kube-system | grep coredns. Check CoreDNS logs. Verify NetworkPolicy allows DNS.

```bash
kubectl exec <pod> -- nslookup my-service.default.svc.cluster.local
```

---

## 33. NodePort Service Not Accessible Externally

**Symptom:** Hitting NodeIP:NodePort from outside cluster returns connection refused.

**Root Cause:** Security group / firewall blocking the NodePort range (30000-32767), or service not correctly configured.

**Fix:** Add firewall rule for NodePort. Verify service type: NodePort. Check node IP is reachable.

```bash
kubectl get svc <name> -o wide
```

---

## 34. LoadBalancer Service Stuck in Pending

**Symptom:** Service of type LoadBalancer shows <pending> for EXTERNAL-IP indefinitely.

**Root Cause:** Cloud controller manager not running, insufficient cloud quota, or missing IAM permissions for LB creation.

**Fix:** Check CCM pods in kube-system. Verify cloud provider IAM role. Check cloud console for LB creation errors.

```bash
kubectl describe svc <name> -n <ns> | grep -A10 Events
```

---

## 35. Ingress Not Routing Traffic

**Symptom:** Ingress resource exists but requests return 404 or default backend.

**Root Cause:** Ingress class annotation missing/wrong, host mismatch, or ingress controller not deployed.

**Fix:** Verify ingressClassName. Confirm ingress controller pods are running. Check host field matches request.

```bash
kubectl describe ingress <name> -n <ns>
```

---

## 36. TLS Certificate Not Served by Ingress

**Symptom:** Browser shows certificate error; HTTPS returns wrong cert or HTTP fallback.

**Root Cause:** TLS secret missing, cert-manager not issuing certificate, or wrong secretName in ingress spec.

**Fix:** kubectl describe certificate -n <ns>. Check cert-manager logs. Ensure ClusterIssuer/Issuer exists.

```bash
kubectl get certificate -n <ns>
```

---

## 37. Pod-to-Pod Communication Failing

**Symptom:** Pod A cannot curl or ping Pod B on its pod IP.

**Root Cause:** CNI plugin misconfigured, NetworkPolicy blocking pod-to-pod traffic, or overlay network issue.

**Fix:** Check CNI pods (calico/flannel/cilium). kubectl describe networkpolicy. Test with netshoot debug pod.

```bash
kubectl run netshoot --rm -it --image nicolaka/netshoot -- bash
```

---

## 38. NetworkPolicy Accidentally Blocking All Traffic

**Symptom:** After applying NetworkPolicy, all pods lose connectivity.

**Root Cause:** Default-deny policy applied without proper allow rules for required traffic.

**Fix:** kubectl describe networkpolicy. Add explicit allow rules for required ingress/egress. Check DNS port 53 is allowed.

```bash
kubectl get networkpolicy -n <ns>
```

---

## 39. Intermittent Service Timeouts

**Symptom:** Service calls succeed most of the time but randomly timeout.

**Root Cause:** A subset of backend pods are unhealthy, kube-proxy iptables rules stale, or connection tracking table full.

**Fix:** Check readiness probes on all pods. Restart kube-proxy. Inspect conntrack table on node.

```bash
kubectl get pods -n <ns> -o wide | grep -v Running
```

---

## 40. ExternalName Service Not Resolving

**Symptom:** Service of type ExternalName returns NXDOMAIN or wrong IP.

**Root Cause:** CNAME chain too long, external DNS flapping, or CoreDNS not forwarding external queries.

**Fix:** kubectl exec into pod and nslookup the external name. Check CoreDNS ConfigMap forward rules.

```bash
kubectl get configmap coredns -n kube-system -o yaml
```

---

## 41. Headless Service Returning Wrong IPs

**Symptom:** DNS for headless service returns stale pod IPs after pod restarts.

**Root Cause:** DNS TTL caching in application or pod, or endpoints controller lag.

**Fix:** Reduce DNS TTL in CoreDNS or application DNS cache. Verify endpoint update latency.

```bash
kubectl get endpoints <svc> -n <ns> -w
```

---

## 42. Service Account Token Volume Projection Error

**Symptom:** Pod fails to start: token volume projection error in events.

**Root Cause:** TokenRequest API disabled, or projected service account token expiry misconfigured.

**Fix:** Ensure TokenRequest feature gate is enabled (default in 1.20+). Check RBAC for system:serviceaccounts.

```bash
kubectl describe pod <pod> | grep -i token
```

---

## 43. Cross-Namespace Service Access Failing

**Symptom:** Pod in namespace A cannot reach service in namespace B.

**Root Cause:** NetworkPolicy in namespace B blocking ingress from other namespaces, or FQDN not used.

**Fix:** Use FQDN: <svc>.<namespace>.svc.cluster.local. Add NetworkPolicy ingress rule from namespace A.

```bash
kubectl exec <pod-ns-a> -- curl http://svc-b.namespaceb.svc.cluster.local
```

---

## 44. Kube-Proxy Not Updating iptables Rules

**Symptom:** New service endpoints not reachable; old pod IPs still in use.

**Root Cause:** kube-proxy pod crashed or stuck, or iptables sync interval too high.

**Fix:** Check kube-proxy logs: kubectl logs -n kube-system. Restart kube-proxy DaemonSet.

```bash
kubectl rollout restart daemonset/kube-proxy -n kube-system
```

---

## 45. Egress Traffic Blocked from Pod

**Symptom:** Pod cannot reach external internet or external APIs.

**Root Cause:** NetworkPolicy with egress rules missing, or cloud security group/NAT gateway misconfigured.

**Fix:** Add egress allow rule in NetworkPolicy for external CIDR. Verify NAT gateway or IGW in cloud VPC.

```bash
kubectl exec <pod> -- curl -v https://google.com
```

---
