# Resilient SOC Case File #003 — Operation Autopilot

## From Detection to Automated Response: SOAR Integration Lab

**Team:** Resilient SOC  
**Case File:** #003  
**Operation:** Autopilot  
**Date:** 03 September 2026  
**Environment:** Kali Linux / Docker / TheHive / Cortex / Shuffle

---

# 1. Executive Summary

## 1.1 Overview

Resilient SOC Case File #003 — Operation Autopilot is a defensive SOAR training lab designed to demonstrate how security observables can move through a connected investigation and response workflow.

The lab integrates:

- TheHive for case and observable management.
- Cortex for automated analysis and enrichment.
- Shuffle for workflow orchestration.
- Python/Flask for Human-in-the-Loop decision handling.
- Mailhog/Email for lab notification.
- Docker for isolated infrastructure.

## 1.2 Operational Goal

The target workflow is:

```text
TheHive
   ↓
Shuffle
   ↓
TheHive Observable Retrieval
   ↓
Dynamic Observable Mapping
   ↓
Cortex Enrichment
   ↓
Analysis Result
   ↓
Human Analyst Decision
   ↓
Simulated Response
   ↓
TheHive Case Update
   ↓
Notification
```

---

# 2. Architecture

| Component | Purpose |
|---|---|
| TheHive | Case and observable management (UI `:9000`) |
| Cortex | Analyzer execution and enrichment (UI `:9001/cortex`) |
| Shuffle | Workflow orchestration (UI `:3001`) |
| Cassandra / Elasticsearch | Backing stores |
| Flask | Human-in-the-Loop approve/reject endpoint |
| Mailhog | Lab email notification |

# 3. Implementation Summary

1. **Infrastructure** – Docker stack with Cassandra, Elasticsearch, TheHive, Cortex and Shuffle.
2. **TheHive** – case and observable (`8.8.8.8`) created for testing.
3. **TheHive ↔ Cortex** – Cortex server configured in TheHive with a Bearer API key; analyzers listed (HTTP 200) and `TestAnalyzer` executed.
4. **Analyzer investigation** – IPinfo analyzer reviewed; a local compatibility definition was used for the lab.
5. **Shuffle workflow `SOC-Auto-Response`** – TheHive trigger, observable retrieval, dynamic mapping with `$thehive_2.body.0.data`, Cortex_1 analyzer execution.
6. **Human-in-the-Loop** – approve and reject paths via a webhook receiver (webhook troubleshooting documented in the command history).
7. **Simulated response** – containment and escalation are simulated only.
8. **Close-out** – TheHive case update and notification.

# 4. Validation Matrix

| Check | Result |
|---|---|
| Cortex API status | HTTP 200 |
| TestAnalyzer run | Success |
| Dynamic observable mapping | Success |
| Cortex_1 analyzer run from Shuffle | Success |
| Approve path / Reject path | Validated |
| TheHive update + notification | Validated |

# 5. Security Notes

- No production containment; all response actions are simulated.
- Secrets are placeholders in this repository. Rotate any key that appeared in terminal history or screenshots (Evidence E-17).
- Default/weak lab passwords used during testing must not be reused outside the lab.

# 6. Evidence

See [../Evidence/Evidence_Index.md](../Evidence/Evidence_Index.md).

# 7. Handoff

See [../Team_Handoff/Team_Handoff.md](../Team_Handoff/Team_Handoff.md).

