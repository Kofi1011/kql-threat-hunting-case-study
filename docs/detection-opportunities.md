# Detection opportunities — proposed, not deployed

These are follow-up ideas suggested by the investigation. None has been tuned, deployed or measured in production.

| Idea | Useful signals | False positives and validation needed |
|---|---|---|
| Sensitive configuration access from an unusual parent | File path, reading command, process parent and execution account | Application startup, deployment tooling and authorized troubleshooting; validate known process chains |
| Broad database collection outside the application pattern | Unbounded reads, catalog access, account and request sequence | Reporting, migrations and DBA work; compare query content with approved jobs |
| Registry enumeration from a non-CI source | Account, source address, repository and operation | Build runners moving addresses and security scanners; maintain a current CI baseline |
| API success claim contradicted by access denial | Agent session/action plus API request identifier and status | Wrong request correlation, retries and asynchronous actions; match the actual request before classifying a contradiction |
| External connection from the suspicious interpreter | Device/process identity, account, parent and remote endpoint | Legitimate automation; stable process identifiers and a time-bounded join are needed |
| Account and SSH preparation sequence | New account, privileged membership, SSH-related file/authentication events | Provisioning and administration; confirm authorization and later logins |

## Validation plan

For each candidate, first define the suspicious behaviour and a benign comparison set. Replay authorized lab examples, inspect false matches and missing cases, then record coverage and query cost. Only after validation should a query be considered for an alert rule.

Monitoring does not replace remediation. Credential rotation, access review and host containment would require environment-specific authorization and operational planning; they were not performed in this exercise.

