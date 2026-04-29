# Incident Timeline Visualization

Incident: `INC-LW3-2026-04-17-001`
Window: `2026-04-17 10:15:00` to `2026-04-17 10:45:00`

```mermaid
timeline
    title RCA Incident Timeline
    2026-04-17 09:45:00 to 2026-04-17 10:14:00 : Normal state
    2026-04-17 10:15:00 : Anomaly start
    2026-04-17 10:30:00 : Peak incident
    2026-04-17 10:46:00 : Recovery
```

## Stage Summaries
- Normal state: Stable traffic and low error rate with baseline latency behavior.
- Anomaly start: Latency and error-rate thresholds exceed baseline expectations.
- Peak incident: Maximum combined impact on latency, failures, and request pressure.
- Recovery: Signals returned close to baseline and stabilized.
