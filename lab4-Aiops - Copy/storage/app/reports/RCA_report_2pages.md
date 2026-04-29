# Root Cause Analysis Report (Lab Work 4)

## 1) Executive Summary
- **Incident ID:** `INC-LW3-2026-04-17-001`
- **Selected anomaly window:** `2026-04-17 10:15:00` to `2026-04-17 10:45:00`
- **Root cause endpoint:** `/api/error`
- **Primary signal:** `error_rate`
- **Confidence score:** `0.98`
- **Recommended action:** Prioritize database profiling for /api/error: inspect slow queries, lock waits, and pool limits.

This RCA correlates metrics and logs from the selected anomaly window. The strongest attribution points to one endpoint where traffic pressure, latency inflation, and failure expansion happened simultaneously. Evidence indicates that this endpoint amplified system-wide instability and dominated the incident profile.

## 2) Signal Analysis

### Latency
- Baseline average latency: **140.01 ms**
- Incident average latency: **493.95 ms**
- Peak latency during incident: **650.79 ms**

### Request Rate
- Baseline average request rate: **575.37 req/min**
- Incident average request rate: **727.42 req/min**
- Peak request rate during incident: **779.00 req/min**

### Error Rate
- Baseline average error rate: **0.62%**
- Incident average error rate: **8.18%**
- Peak error rate during incident: **13.55%**

### Endpoint Activity and Attribution
| Endpoint | Baseline Latency | Incident Latency | Baseline Error Rate | Incident Error Rate | Contribution |
|---|---:|---:|---:|---:|---:|
| /api/error | 136.10 ms | 931.77 ms | 0.70% | 20.14% | 2.787 |
| /api/slow | 208.77 ms | 458.58 ms | 0.63% | 2.04% | 0.454 |
| /api/orders | 121.63 ms | 171.87 ms | 0.44% | 1.08% | 0.200 |

The endpoint with the highest contribution score is identified as the likely root cause because it leads across the strongest failure indicators and has the highest weighted impact during the anomaly.

## 3) Error Category Analysis
Top error category during anomaly: **DATABASE_ERROR**

| Error Category | Baseline Count | Anomaly Count | Delta | Anomaly Share |
|---|---:|---:|---:|---:|
| DATABASE_ERROR | 8 | 246 | +238 | 44.32% |
| SYSTEM_ERROR | 30 | 162 | +132 | 29.19% |
| TIMEOUT_ERROR | 19 | 74 | +55 | 13.33% |
| VALIDATION_ERROR | 15 | 73 | +58 | 13.15% |

The error-category mix shifted from low, distributed noise to concentrated failure classes in the anomaly window. This distribution shift supports the endpoint attribution and narrows probable remediation paths.

## 4) Incident Timeline
- **Normal state:** 2026-04-17 09:45:00 to 2026-04-17 10:14:00
- **Anomaly start:** 2026-04-17 10:15:00
- **Peak incident:** 2026-04-17 10:30:00 (severity index 11.866)
- **Recovery:** 2026-04-17 10:46:00

Timeline interpretation:
- Normal state maintained stable traffic and low error behavior.
- The anomaly started when thresholds in latency and failure rate crossed baseline tolerances.
- The peak combines highest weighted pressure from latency and errors.
- Recovery is marked by return of key signals near pre-incident levels.

## 5) Root Cause Statement
The most likely root cause is **/api/error**, where high request load and rapid error escalation produced the strongest contribution score and aligned with the dominant error category (DATABASE_ERROR).

## 6) Recommended Corrective Actions
1. Prioritize database profiling for /api/error: inspect slow queries, lock waits, and pool limits.
2. Add per-endpoint SLO alerts for p95 latency, error-rate acceleration, and anomaly onset detection.
3. Introduce request shedding / circuit breaker policies around downstream failures.
4. Add runbook steps for rapid endpoint-level mitigation and post-incident verification.

## 7) Evidence Snippets
- Selected incident window from Lab Work 3 (2026-04-17 10:15:00 to 2026-04-17 10:45:00).
- Global latency moved from 140.01 ms baseline to 493.95 ms during incident.
- Global error rate moved from 0.62% baseline to 8.18% during incident.
- Endpoint /api/error has the highest contribution score (2.787).
- Top error category during anomaly was DATABASE_ERROR (246 events).

---
Generated automatically by `php artisan rca:analyze`.
