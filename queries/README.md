# KQL library

These six defensive templates generalize query approaches used in the investigation. They use the lab's custom-table naming conventions. **The sanitized versions have not been executed against a workspace.** They are not deployed detection rules.

| Query | Purpose |
|---|---|
| [01 — Inventory](01-scope-inventory.kql) | Discover available hunt/run/host groups before assuming a run label |
| [02 — Process lineage](02-process-lineage.kql) | Examine a child process and its candidate parent within one device/time scope |
| [03 — Network context](03-network-context.kql) | Compare outbound process IDs and accounts, rather than judging by port alone |
| [04 — Baseline](04-process-baseline.kql) | Compare a process's command and parent across days |
| [05 — API claim review](05-api-claim-review.kql) | Inspect API logs before accepting an external success narrative |
| [06 — Registry operations](06-registry-baseline.kql) | Summarize operations by account/source and compare them with approved CI behaviour |

## Before running

Set the time interval and generalized host/process names in each file. Replace the example `example-hunt` and `lab-web01` values with authorized dataset values. Confirm columns in the workspace schema; `Device*Events_CL` and registry logs here are custom tables, not standard XDR tables.

Dates and names in these templates are example configuration, not additional evidence from the exercise. The files intentionally omit the exercise's answer values and credentials.

## Interpretation

- A zero result can reflect a wrong scope, missing ingestion or an incorrect schema assumption.
- Counts represent rows unless a reliable unique event key is used.
- A process-name or account match is a lead, not a verdict.
- Triage joins on PIDs require extra validation for PID reuse; use creation-time/stable process identifiers if available.
- Do not treat a query result as permission to change a system or a credential.

