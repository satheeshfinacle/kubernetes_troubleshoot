# Helm & Config

[← Back to main index](../README.md)

5 scenario(s) in this category.

## 76. Helm Release Stuck in Pending-Install

**Symptom:** helm status shows PENDING-INSTALL; resources deployed but release not finalized.

**Root Cause:** Helm hook job failed, or previous failed install left partial state.

**Fix:** helm rollback or helm uninstall --no-hooks. Delete stuck hook jobs manually.

```bash
helm history <release> -n <ns>
```

---

## 77. Helm Values Not Applied to Deployment

**Symptom:** Helm upgrade succeeds but pod environment has old values.

**Root Cause:** Cached deployment not rolling — pods not restarted after config-only helm upgrade.

**Fix:** Use helm upgrade --force or add checksum annotation to deployment to trigger rollout on config change.

```bash
helm upgrade <release> . --set image.tag=new --force
```

---

## 78. Namespace Not Created Before Helm Install

**Symptom:** helm install fails: namespace <ns> not found.

**Root Cause:** --create-namespace flag not used and namespace does not exist.

**Fix:** kubectl create namespace <ns> first, or use helm install --create-namespace.

```bash
helm install <name> <chart> -n <ns> --create-namespace
```

---

## 79. ConfigMap Key Not Found Error

**Symptom:** Pod fails with error: key not found in ConfigMap; envFrom producing empty env.

**Root Cause:** ConfigMap key name typo in valueFrom.configMapKeyRef, or ConfigMap was recreated with different keys.

**Fix:** kubectl describe configmap <name>. Verify exact key names. Update deployment env section.

```bash
kubectl get configmap <name> -n <ns> -o jsonpath='{.data}'
```

---

## 80. Duplicate Resource Conflict After Helm Upgrade

**Symptom:** Helm upgrade fails: resource already exists or cannot patch immutable field.

**Root Cause:** Resource was manually modified or created outside Helm, causing ownership conflict.

**Fix:** kubectl annotate or label the resource with Helm ownership, or use --force flag.

```bash
helm upgrade <release> <chart> --force -n <ns>
```

---
