# CI/CD & GitOps

[← Back to main index](../README.md)

5 scenario(s) in this category.

## 81. ArgoCD App Out of Sync After Deployment

**Symptom:** ArgoCD shows application OutOfSync; manual changes in cluster not matching Git.

**Root Cause:** Manual kubectl edit or patch bypassed GitOps workflow, causing drift.

**Fix:** argocd app sync <app> --force to reset to Git state. Enforce GitOps — disable direct kubectl edits.

```bash
argocd app sync <app-name> --force
```

---

## 82. ArgoCD Stuck in Progressing State

**Symptom:** ArgoCD shows Progressing for a long time; deployment never completes.

**Root Cause:** Pod health check failing, deployment rollout stuck, or ArgoCD health check misconfigured.

**Fix:** Check deployment status in ArgoCD UI. Check pod events. Fix deployment issue.

```bash
argocd app get <app> --show-operation
```

---

## 83. FluxCD Kustomization Fails to Apply

**Symptom:** Flux shows reconciliation failed; resources not created in cluster.

**Root Cause:** YAML syntax error in kustomization, missing dependency, or RBAC insufficient for Flux SA.

**Fix:** flux get kustomizations. Check flux logs: kubectl logs -n flux-system deploy/kustomizecontroller.

```bash
flux get kustomizations --all-namespaces
```

---

## 84. Pipeline Fails on Docker Build Step

**Symptom:** CI pipeline fails at docker build: permission denied to push or image pull rate limit.

**Root Cause:** Registry auth token expired, missing imagePullSecret, or Docker Hub rate limit.

**Fix:** Refresh registry credentials. Use private registry. Add imagePullSecret to pipeline service account.

```bash
kubectl create secret docker-registry regcred --docker-server=... -n <ns>
```

---

## 85. Canary Deployment Not Splitting Traffic

**Symptom:** All traffic goes to stable version; canary pods exist but receive no requests.

**Root Cause:** Service mesh (Istio/Linkerd) VirtualService not configured, or Ingress weights not set.

**Fix:** Configure Istio VirtualService with weight: 90/10. Or use Argo Rollouts canary strategy.

```bash
kubectl get virtualservice -n <ns> -o yaml
```

---
