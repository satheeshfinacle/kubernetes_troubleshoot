# Observability

[← Back to main index](../README.md)

5 scenario(s) in this category.

## 86. Prometheus Not Scraping Pod Metrics

**Symptom:** Pod metrics missing in Prometheus; targets show DOWN.

**Root Cause:** Missing prometheus.io/scrape: 'true' annotation, wrong port annotation, or NetworkPolicy blocking scrape.

**Fix:** Add annotations to pod/service. Verify metricsPort. Allow Prometheus namespace in NetworkPolicy.

```bash
kubectl annotate pod <pod> prometheus.io/scrape=true prometheus.io/port=8080
```

---

## 87. kubectl top Showing Unknown Metrics

**Symptom:** kubectl top nodes/pods shows unknown or no metrics.

**Root Cause:** Metrics Server not installed or pod not running.

**Fix:** kubectl apply -f metrics-server.yaml or install via Helm. Verify metrics-server pod is Running.

```bash
kubectl get pods -n kube-system | grep metrics-server
```

---

## 88. Grafana Dashboard Showing No Data

**Symptom:** Grafana panels display No Data despite Prometheus running.

**Root Cause:** Datasource URL wrong, time range mismatch, or PromQL query incorrect.

**Fix:** Test datasource in Grafana. Check time range. Validate PromQL with Prometheus UI directly.

```bash
kubectl port-forward svc/prometheus 9090:9090 -n monitoring
```

---

## 89. Log Aggregator Missing Container Logs

**Symptom:** Fluentd/Fluentbit not shipping all pod logs to Elasticsearch/Loki.

**Root Cause:** Log aggregator DaemonSet not on all nodes, log path mismatch, or buffer overflow.

**Fix:** Check DaemonSet coverage: kubectl get pods -n logging -o wide. Fix log path. Increase buffer size.

```bash
kubectl logs -n logging <fluentbit-pod> | grep -i error
```

---

## 90. Alert Manager Not Sending Notifications

**Symptom:** Prometheus alert fires but no notification received in Slack/PagerDuty.

**Root Cause:** AlertManager config YAML error, webhook URL wrong, or inhibit rule suppressing alert.

**Fix:** kubectl get secret alertmanager-config -n monitoring. Validate YAML. Check AlertManager UI silence/inhibit.

```bash
kubectl port-forward svc/alertmanager 9093:9093 -n monitoring
```

---
