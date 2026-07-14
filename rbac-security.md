# RBAC & Security

[← Back to main index](../README.md)

5 scenario(s) in this category.

## 56. Forbidden Error When Accessing Resources

**Symptom:** kubectl or application gets Error: pods is forbidden: User cannot list resource.

**Root Cause:** ServiceAccount or user missing Role/ClusterRole binding for the required verb.

**Fix:** Create RoleBinding/ClusterRoleBinding. Use kubectl auth can-i to verify permissions.

```bash
kubectl auth can-i list pods --as=system:serviceaccount:<ns>:<sa>
```

---

## 57. ServiceAccount Token Not Automounted

**Symptom:** Application inside pod cannot call Kubernetes API — token file missing at /var/run/secrets/.

**Root Cause:** automountServiceAccountToken: false set on ServiceAccount or pod spec.

**Fix:** Set automountServiceAccountToken: true on pod or ServiceAccount.

```bash
kubectl get serviceaccount <sa> -n <ns> -o yaml
```

---

## 58. Pod Security Admission Blocking Pod Creation

**Symptom:** Pod creation fails: pod violates PodSecurity policy enforce level.

**Root Cause:** Pod spec uses privileged mode or hostPath mounts that violate namespace PSA policy.

**Fix:** Remove privileged/hostPath if not needed. Or update namespace label to appropriate PSA level.

```bash
kubectl get namespace <ns> -o yaml | grep pod-security
```

---

## 59. RBAC Roles Not Propagated Across Namespaces

**Symptom:** User has ClusterRole but cannot access resources in specific namespace.

**Root Cause:** ClusterRole bound with RoleBinding (namespace-scoped) instead of ClusterRoleBinding.

**Fix:** Use ClusterRoleBinding for cluster-wide access. Use RoleBinding per-namespace for scoped access.

```bash
kubectl get rolebindings,clusterrolebindings --all-namespaces | grep <user>
```

---

## 60. Audit Logs Showing Unauthorized API Calls

**Symptom:** Audit log shows 403s for service account trying to list/create resources.

**Root Cause:** Newly deployed application making API calls without proper RBAC setup.

**Fix:** Identify SA from audit log. Create Role + RoleBinding for required verbs. Apply least privilege.

```bash
kubectl logs -n kube-system <apiserver-pod> | grep audit
```

---
