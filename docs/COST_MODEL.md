# Cost Model Documentation

This document explains how the K8s Cost Optimization Dashboard calculates Kubernetes resource costs, waste, and savings potential.

---

## Pricing Model

The dashboard uses a **cloud-provider-agnostic hourly rate model** based on typical AWS/GCP/Azure on-demand pricing:

| Resource | Rate |
|---|---|
| CPU (per vCPU-hour) | $0.048 |
| Memory (per GB-hour) | $0.006 |
| Hours per month | 730 |

### Monthly Cost Formula

```
Monthly Cost = (CPU_requested_vCPU × $0.048 + Memory_requested_GB × $0.006) × 730
```

### Example: Single Pod
```
Pod requests: 500m CPU, 512Mi Memory

CPU cost:    0.5 vCPU × $0.048/hr × 730 hrs = $17.52/month
Memory cost: 0.5 GB  × $0.006/hr × 730 hrs = $2.19/month
Total:       $19.71/month
```

---

## Waste Calculation

Waste is the difference between **requested** resources and **actual usage**:

```
CPU Waste    = CPU_requested - CPU_actual_p95_usage
Memory Waste = Memory_requested - Memory_actual_p95_usage

Wasted Cost = (CPU_Waste × $0.048 + Memory_Waste × $0.006) × 730
```

> **p95 usage** = 95th percentile over a 7-day rolling window (from Prometheus)

### Waste Severity Levels

| Efficiency | Severity | Action |
|---|---|---|
| < 30% | 🔴 Critical | Immediate right-sizing needed |
| 30–50% | 🟠 High | Schedule right-sizing this sprint |
| 50–70% | 🟡 Medium | Review next planning cycle |
| 70–85% | 🟢 Low | Monitor only |
| > 85% | ✅ Optimal | No action needed |

---

## Right-Sizing Recommendations

Recommended resource values are calculated as:

```
Recommended CPU    = max(p95_cpu_usage × 1.2, minimum_cpu_floor)
Recommended Memory = max(p95_memory_usage × 1.3, minimum_memory_floor)
```

Headroom multipliers:
- **CPU:** 1.2× (20% headroom) — CPU throttling is recoverable
- **Memory:** 1.3× (30% headroom) — OOMKill is not recoverable

Minimum floors:
- CPU: 10m (millicores)
- Memory: 64Mi

---

## Savings Potential

```
Savings Potential = Current Monthly Cost - Projected Cost After Right-Sizing

Projected Cost = Sum of all pods using Recommended resources × rates × 730
```

---

## Efficiency Score

The overall cluster efficiency score (0–100%):

```
Efficiency Score = (Total Actual Usage / Total Requested Resources) × 100

Where:
  Total Actual Usage      = sum of p95 CPU and Memory across all pods
  Total Requested Resources = sum of all resource requests
```

A score of **65–80%** is considered healthy for production workloads.  
Scores below 50% indicate significant over-provisioning.

---

## Namespace Rollup

Namespace-level metrics are rolled up from individual pod metrics:

```
Namespace Monthly Cost     = sum of all pod costs in namespace
Namespace Waste            = sum of all pod waste in namespace
Namespace Utilization      = (Actual Usage / Requested) × 100
Namespace Pod Count        = count of running pods
```

---

## Savings Forecast

The 6-month savings forecast assumes:
1. **Month 1–2:** Teams implement critical (red) recommendations — 40% of savings unlocked
2. **Month 3–4:** Teams implement high (orange) recommendations — 75% of savings unlocked
3. **Month 5–6:** All recommendations implemented, baseline savings realized — 100%

```
Month N Savings = Savings_Potential × Implementation_Progress[N]
Cumulative Savings = sum(Month 1..N savings)
```

---

## Data Sources (Production Integration)

In a real deployment, this data would come from:

| Metric | Source | Query |
|---|---|---|
| CPU requests | Kubernetes API | `kube_pod_container_resource_requests{resource="cpu"}` |
| Memory requests | Kubernetes API | `kube_pod_container_resource_requests{resource="memory"}` |
| CPU usage (p95) | Prometheus | `quantile_over_time(0.95, rate(container_cpu_usage_seconds_total[5m])[7d:])` |
| Memory usage (p95) | Prometheus | `quantile_over_time(0.95, container_memory_working_set_bytes[7d:])` |
| Node costs | Cloud billing API or manually configured | Per-node hourly rate |
