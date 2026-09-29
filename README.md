# Resilient SOC Case File #003 — Operation Autopilot

> From detection to automated response: a defensive SOAR lab built with open-source tools.

Case File #003 scales the manual investigations of Case Files #001 and #002 into a fully orchestrated Incident Response pipeline: a case in **TheHive** triggers a **Shuffle** workflow that enriches observables with **Cortex**, asks a human analyst to approve or reject, runs a *simulated* response, and writes the outcome back to TheHive.

**Status:** ✅ Completed — see [COMPLETED_TASKS.md](COMPLETED_TASKS.md)

## Lab Architecture

| Component | Role | Lab URL |
|---|---|---|
| TheHive | Case & observable management | `http://<LAB_IP>:9000` |
| Cortex | Automated analysis / enrichment | `http://<LAB_IP>:9001/cortex` |
| Shuffle | Workflow orchestration (SOAR) | `http://<LAB_IP>:3001` |
| Cassandra + Elasticsearch | Storage backends for TheHive / Cortex | internal |
| Flask app | Human-in-the-Loop approve / reject receiver | lab-local |
| Mailhog | Lab notification sink | lab-local |
| Docker | Isolated infrastructure | — |

## Workflow

```
TheHive → Shuffle → TheHive (observable retrieval) → dynamic mapping
   → Cortex analyzer → analysis result → human decision (approve / reject)
   → simulated response → TheHive case update → notification
```

Key dynamic expression in Shuffle: `$thehive_2.body.0.data`
Primary test observable: `8.8.8.8`

## Repository Layout

```
.
├── README.md
├── COMPLETED_TASKS.md
├── Documentation/        Full technical write-up
├── Commands/             Commands reference + sanitized terminal history
├── Configuration/        Sanitized configuration notes
├── Evidence/             Evidence index + screenshots
└── Team_Handoff/         Handoff notes
```

## Safety Notes

- This is a **training lab**. No production containment actions are performed — response steps are simulated.
- All credentials, API keys and webhook IDs have been replaced by placeholders such as `<CORTEX_API_KEY>`, `<THEHIVE_PASSWORD>`, `<ELASTIC_PASSWORD>`.
- Never commit real secrets. Any key that ever appeared in a terminal history or screenshot must be **rotated** (see Evidence E-17).

## Documentation

Start with [Documentation/Case_File_003_Operation_Autopilot.md](Documentation/Case_File_003_Operation_Autopilot.md), then [Team_Handoff/Team_Handoff.md](Team_Handoff/Team_Handoff.md).
