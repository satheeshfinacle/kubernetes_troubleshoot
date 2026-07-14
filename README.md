# 100 Kubernetes Troubleshooting Scenarios

A categorized, real-world reference of common Kubernetes failure modes — each entry documents the **symptom**, **root cause**, **fix**, and a ready-to-run **command**.

## Why this repo exists

Kubernetes failures tend to repeat across teams and clusters. This repository collects 100 of the most frequently encountered issues — from pod scheduling and networking to control-plane and GitOps failures — into a single, searchable diagnostic reference.

## Categories

| # | Category | Scenarios | File |
|---|----------|-----------|------|
| 1–20 | Pod Issues | 20 | [pod-issues.md](scenarios/pod-issues.md) |
| 21–30 | Deployment Issues | 10 | [deployment-issues.md](scenarios/deployment-issues.md) |
| 31–45 | Networking | 15 | [networking.md](scenarios/networking.md) |
| 46–55 | Storage | 10 | [storage.md](scenarios/storage.md) |
| 56–60 | RBAC & Security | 5 | [rbac-security.md](scenarios/rbac-security.md) |
| 61–68 | Node Issues | 8 | [node-issues.md](scenarios/node-issues.md) |
| 69–75 | Control Plane | 7 | [control-plane.md](scenarios/control-plane.md) |
| 76–80 | Helm & Config | 5 | [helm-config.md](scenarios/helm-config.md) |
| 81–85 | CI/CD & GitOps | 5 | [ci-cd-gitops.md](scenarios/ci-cd-gitops.md) |
| 86–90 | Observability | 5 | [observability.md](scenarios/observability.md) |
| 91–100 | Advanced | 10 | [advanced.md](scenarios/advanced.md) |

## Full Scenario Index

| # | Title | Category |
|---|-------|----------|
| 1 | [Pod Stuck in Pending State](scenarios/pod-issues.md#1-pod-stuck-in-pending-state) | Pod Issues |
| 2 | [CrashLoopBackOff](scenarios/pod-issues.md#2-crashloopbackoff) | Pod Issues |
| 3 | [OOMKilled — Pod Memory Limit Exceeded](scenarios/pod-issues.md#3-oomkilled-pod-memory-limit-exceeded) | Pod Issues |
| 4 | [ImagePullBackOff](scenarios/pod-issues.md#4-imagepullbackoff) | Pod Issues |
| 5 | [Pod in Terminating State Forever](scenarios/pod-issues.md#5-pod-in-terminating-state-forever) | Pod Issues |
| 6 | [Init Container Failing](scenarios/pod-issues.md#6-init-container-failing) | Pod Issues |
| 7 | [Container Exits with Code 1](scenarios/pod-issues.md#7-container-exits-with-code-1) | Pod Issues |
| 8 | [Pod Not Matching Node Affinity](scenarios/pod-issues.md#8-pod-not-matching-node-affinity) | Pod Issues |
| 9 | [Pod Evicted Due to Disk Pressure](scenarios/pod-issues.md#9-pod-evicted-due-to-disk-pressure) | Pod Issues |
| 10 | [Liveness Probe Failing](scenarios/pod-issues.md#10-liveness-probe-failing) | Pod Issues |
| 11 | [Readiness Probe Failing](scenarios/pod-issues.md#11-readiness-probe-failing) | Pod Issues |
| 12 | [Pod Pending Due to Taint/Toleration Mismatch](scenarios/pod-issues.md#12-pod-pending-due-to-taint-toleration-mismatch) | Pod Issues |
| 13 | [Multi-Container Pod — Wrong Container Targeted](scenarios/pod-issues.md#13-multi-container-pod-wrong-container-targeted) | Pod Issues |
| 14 | [Pod Running but Application Not Accessible](scenarios/pod-issues.md#14-pod-running-but-application-not-accessible) | Pod Issues |
| 15 | [High Restart Count on Pod](scenarios/pod-issues.md#15-high-restart-count-on-pod) | Pod Issues |
| 16 | [ConfigMap Not Reflected in Pod](scenarios/pod-issues.md#16-configmap-not-reflected-in-pod) | Pod Issues |
| 17 | [Secret Not Mounted in Pod](scenarios/pod-issues.md#17-secret-not-mounted-in-pod) | Pod Issues |
| 18 | [Pod Cannot Write to Volume](scenarios/pod-issues.md#18-pod-cannot-write-to-volume) | Pod Issues |
| 19 | [Startup Probe Causing Early Termination](scenarios/pod-issues.md#19-startup-probe-causing-early-termination) | Pod Issues |
| 20 | [Pod Scheduled on Wrong Node](scenarios/pod-issues.md#20-pod-scheduled-on-wrong-node) | Pod Issues |
| 21 | [Deployment Rollout Stuck](scenarios/deployment-issues.md#21-deployment-rollout-stuck) | Deployment Issues |
| 22 | [Deployment Not Scaling](scenarios/deployment-issues.md#22-deployment-not-scaling) | Deployment Issues |
| 23 | [Rolling Update Causes Downtime](scenarios/deployment-issues.md#23-rolling-update-causes-downtime) | Deployment Issues |
| 24 | [ReplicaSet Pods Not Starting After Image Update](scenarios/deployment-issues.md#24-replicaset-pods-not-starting-after-image-update) | Deployment Issues |
| 25 | [Deployment Shows 0/3 Ready](scenarios/deployment-issues.md#25-deployment-shows-0-3-ready) | Deployment Issues |
| 26 | [HPA Not Scaling Deployment](scenarios/deployment-issues.md#26-hpa-not-scaling-deployment) | Deployment Issues |
| 27 | [Old Pods Not Terminating During Update](scenarios/deployment-issues.md#27-old-pods-not-terminating-during-update) | Deployment Issues |
| 28 | [Deployment Revision History Full](scenarios/deployment-issues.md#28-deployment-revision-history-full) | Deployment Issues |
| 29 | [Deployment Stuck After Node Drain](scenarios/deployment-issues.md#29-deployment-stuck-after-node-drain) | Deployment Issues |
| 30 | [Environment Variable Not Available in Pod](scenarios/deployment-issues.md#30-environment-variable-not-available-in-pod) | Deployment Issues |
| 31 | [Service Not Routing to Pods](scenarios/networking.md#31-service-not-routing-to-pods) | Networking |
| 32 | [DNS Resolution Failing Inside Pod](scenarios/networking.md#32-dns-resolution-failing-inside-pod) | Networking |
| 33 | [NodePort Service Not Accessible Externally](scenarios/networking.md#33-nodeport-service-not-accessible-externally) | Networking |
| 34 | [LoadBalancer Service Stuck in Pending](scenarios/networking.md#34-loadbalancer-service-stuck-in-pending) | Networking |
| 35 | [Ingress Not Routing Traffic](scenarios/networking.md#35-ingress-not-routing-traffic) | Networking |
| 36 | [TLS Certificate Not Served by Ingress](scenarios/networking.md#36-tls-certificate-not-served-by-ingress) | Networking |
| 37 | [Pod-to-Pod Communication Failing](scenarios/networking.md#37-pod-to-pod-communication-failing) | Networking |
| 38 | [NetworkPolicy Accidentally Blocking All Traffic](scenarios/networking.md#38-networkpolicy-accidentally-blocking-all-traffic) | Networking |
| 39 | [Intermittent Service Timeouts](scenarios/networking.md#39-intermittent-service-timeouts) | Networking |
| 40 | [ExternalName Service Not Resolving](scenarios/networking.md#40-externalname-service-not-resolving) | Networking |
| 41 | [Headless Service Returning Wrong IPs](scenarios/networking.md#41-headless-service-returning-wrong-ips) | Networking |
| 42 | [Service Account Token Volume Projection Error](scenarios/networking.md#42-service-account-token-volume-projection-error) | Networking |
| 43 | [Cross-Namespace Service Access Failing](scenarios/networking.md#43-cross-namespace-service-access-failing) | Networking |
| 44 | [Kube-Proxy Not Updating iptables Rules](scenarios/networking.md#44-kube-proxy-not-updating-iptables-rules) | Networking |
| 45 | [Egress Traffic Blocked from Pod](scenarios/networking.md#45-egress-traffic-blocked-from-pod) | Networking |
| 46 | [PVC Stuck in Pending](scenarios/storage.md#46-pvc-stuck-in-pending) | Storage |
| 47 | [PV Not Releasing After PVC Deletion](scenarios/storage.md#47-pv-not-releasing-after-pvc-deletion) | Storage |
| 48 | [Volume Mount Permission Denied](scenarios/storage.md#48-volume-mount-permission-denied) | Storage |
| 49 | [EBS Volume Stuck Attaching on AWS EKS](scenarios/storage.md#49-ebs-volume-stuck-attaching-on-aws-eks) | Storage |
| 50 | [StatefulSet Pod Can't Find PVC](scenarios/storage.md#50-statefulset-pod-can-t-find-pvc) | Storage |
| 51 | [CSI Driver Not Installed](scenarios/storage.md#51-csi-driver-not-installed) | Storage |
| 52 | [Disk Full on Node Causing Pod Eviction](scenarios/storage.md#52-disk-full-on-node-causing-pod-eviction) | Storage |
| 53 | [ReadWriteMany PVC Not Supported](scenarios/storage.md#53-readwritemany-pvc-not-supported) | Storage |
| 54 | [Volume Expansion Not Taking Effect](scenarios/storage.md#54-volume-expansion-not-taking-effect) | Storage |
| 55 | [emptyDir Volume Data Lost After Pod Restart](scenarios/storage.md#55-emptydir-volume-data-lost-after-pod-restart) | Storage |
| 56 | [Forbidden Error When Accessing Resources](scenarios/rbac-security.md#56-forbidden-error-when-accessing-resources) | RBAC & Security |
| 57 | [ServiceAccount Token Not Automounted](scenarios/rbac-security.md#57-serviceaccount-token-not-automounted) | RBAC & Security |
| 58 | [Pod Security Admission Blocking Pod Creation](scenarios/rbac-security.md#58-pod-security-admission-blocking-pod-creation) | RBAC & Security |
| 59 | [RBAC Roles Not Propagated Across Namespaces](scenarios/rbac-security.md#59-rbac-roles-not-propagated-across-namespaces) | RBAC & Security |
| 60 | [Audit Logs Showing Unauthorized API Calls](scenarios/rbac-security.md#60-audit-logs-showing-unauthorized-api-calls) | RBAC & Security |
| 61 | [Node in NotReady State](scenarios/node-issues.md#61-node-in-notready-state) | Node Issues |
| 62 | [Node Under Memory Pressure](scenarios/node-issues.md#62-node-under-memory-pressure) | Node Issues |
| 63 | [Node Under CPU Pressure](scenarios/node-issues.md#63-node-under-cpu-pressure) | Node Issues |
| 64 | [DaemonSet Pod Not Scheduled on New Node](scenarios/node-issues.md#64-daemonset-pod-not-scheduled-on-new-node) | Node Issues |
| 65 | [Node Cordoned — Pods Not Scheduling](scenarios/node-issues.md#65-node-cordoned-pods-not-scheduling) | Node Issues |
| 66 | [Node Disk I/O Saturation](scenarios/node-issues.md#66-node-disk-i-o-saturation) | Node Issues |
| 67 | [Kubelet Certificate Expired](scenarios/node-issues.md#67-kubelet-certificate-expired) | Node Issues |
| 68 | [Node Clock Skew Causing API Errors](scenarios/node-issues.md#68-node-clock-skew-causing-api-errors) | Node Issues |
| 69 | [API Server Not Responding](scenarios/control-plane.md#69-api-server-not-responding) | Control Plane |
| 70 | [etcd High Latency / Slow Writes](scenarios/control-plane.md#70-etcd-high-latency-slow-writes) | Control Plane |
| 71 | [Scheduler Not Placing Pods](scenarios/control-plane.md#71-scheduler-not-placing-pods) | Control Plane |
| 72 | [Controller Manager Not Reconciling](scenarios/control-plane.md#72-controller-manager-not-reconciling) | Control Plane |
| 73 | [etcd Member Out of Quorum](scenarios/control-plane.md#73-etcd-member-out-of-quorum) | Control Plane |
| 74 | [Certificate Signing Request Stuck Pending](scenarios/control-plane.md#74-certificate-signing-request-stuck-pending) | Control Plane |
| 75 | [Admission Webhook Blocking Pod Creation](scenarios/control-plane.md#75-admission-webhook-blocking-pod-creation) | Control Plane |
| 76 | [Helm Release Stuck in Pending-Install](scenarios/helm-config.md#76-helm-release-stuck-in-pending-install) | Helm & Config |
| 77 | [Helm Values Not Applied to Deployment](scenarios/helm-config.md#77-helm-values-not-applied-to-deployment) | Helm & Config |
| 78 | [Namespace Not Created Before Helm Install](scenarios/helm-config.md#78-namespace-not-created-before-helm-install) | Helm & Config |
| 79 | [ConfigMap Key Not Found Error](scenarios/helm-config.md#79-configmap-key-not-found-error) | Helm & Config |
| 80 | [Duplicate Resource Conflict After Helm Upgrade](scenarios/helm-config.md#80-duplicate-resource-conflict-after-helm-upgrade) | Helm & Config |
| 81 | [ArgoCD App Out of Sync After Deployment](scenarios/ci-cd-gitops.md#81-argocd-app-out-of-sync-after-deployment) | CI/CD & GitOps |
| 82 | [ArgoCD Stuck in Progressing State](scenarios/ci-cd-gitops.md#82-argocd-stuck-in-progressing-state) | CI/CD & GitOps |
| 83 | [FluxCD Kustomization Fails to Apply](scenarios/ci-cd-gitops.md#83-fluxcd-kustomization-fails-to-apply) | CI/CD & GitOps |
| 84 | [Pipeline Fails on Docker Build Step](scenarios/ci-cd-gitops.md#84-pipeline-fails-on-docker-build-step) | CI/CD & GitOps |
| 85 | [Canary Deployment Not Splitting Traffic](scenarios/ci-cd-gitops.md#85-canary-deployment-not-splitting-traffic) | CI/CD & GitOps |
| 86 | [Prometheus Not Scraping Pod Metrics](scenarios/observability.md#86-prometheus-not-scraping-pod-metrics) | Observability |
| 87 | [kubectl top Showing Unknown Metrics](scenarios/observability.md#87-kubectl-top-showing-unknown-metrics) | Observability |
| 88 | [Grafana Dashboard Showing No Data](scenarios/observability.md#88-grafana-dashboard-showing-no-data) | Observability |
| 89 | [Log Aggregator Missing Container Logs](scenarios/observability.md#89-log-aggregator-missing-container-logs) | Observability |
| 90 | [Alert Manager Not Sending Notifications](scenarios/observability.md#90-alert-manager-not-sending-notifications) | Observability |
| 91 | [CRD Version Mismatch After Upgrade](scenarios/advanced.md#91-crd-version-mismatch-after-upgrade) | Advanced |
| 92 | [Cluster Autoscaler Not Removing Idle Nodes](scenarios/advanced.md#92-cluster-autoscaler-not-removing-idle-nodes) | Advanced |
| 93 | [KEDA ScaledObject Not Triggering Scale](scenarios/advanced.md#93-keda-scaledobject-not-triggering-scale) | Advanced |
| 94 | [Istio Sidecar Injection Not Working](scenarios/advanced.md#94-istio-sidecar-injection-not-working) | Advanced |
| 95 | [Velero Backup Failing](scenarios/advanced.md#95-velero-backup-failing) | Advanced |
| 96 | [PodDisruptionBudget Blocking All Node Drains](scenarios/advanced.md#96-poddisruptionbudget-blocking-all-node-drains) | Advanced |
| 97 | [Vertical Pod Autoscaler Causing Restart Loop](scenarios/advanced.md#97-vertical-pod-autoscaler-causing-restart-loop) | Advanced |
| 98 | [Namespace Stuck in Terminating](scenarios/advanced.md#98-namespace-stuck-in-terminating) | Advanced |
| 99 | [Resource Quota Blocking New Deployments](scenarios/advanced.md#99-resource-quota-blocking-new-deployments) | Advanced |
| 100 | [Job Pods Not Cleaning Up After Completion](scenarios/advanced.md#100-job-pods-not-cleaning-up-after-completion) | Advanced |

## How to use this repo

1. Find your symptom in the index above or browse by category.
2. Open the linked scenario for the root cause explanation and fix.
3. Run the provided `kubectl`/`helm`/`etcdctl` command to confirm the diagnosis.

## Contributing

Found a new failure mode or a better fix? Open a PR adding a new entry to the relevant file in `scenarios/`, following the existing Symptom / Root Cause / Fix / Command format.

## License

MIT — see [LICENSE](LICENSE).
