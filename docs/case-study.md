# Case study: evidence before conclusions

**Author: Kofi Williams** · Training environment · Prepared November 2, 2026

## Summary

Operation QUARTZFANG models a multi-stage intrusion involving a portal application, internal services and an agent-driven execution workflow. The central analytical problem was not merely identifying commands. It was determining which actions the telemetry supported, what could be attributed to the suspicious process chain, and where the available evidence contradicted a narrative of success.

The hunt reached full platform completion: 28 flags across eight sections. This report focuses on analytical lessons and supporting evidence categories. It deliberately avoids reproducing the exercise's answers and credentials.

## Questions that guided the analysis

1. Which activity belonged to the application and which descended from the suspicious shell?
2. Did the logs show an action being attempted, successfully executed, or merely claimed?
3. Did the same activity exist before the intrusion window?
4. Could a benign operational failure explain an apparent attack indicator?
5. Were timestamps and run identifiers consistent enough to correlate events?

## 1. Establishing process context

A file-read command alone was not a sufficient finding. Ordinary administrators and applications read configuration files too. The decisive context was the process relationship: the reading utility was a child of the suspicious interpreter, whose own parent was a scheduling service.

Process IDs linked the child to the parent within the observed host and time window. The result supported an association between the configuration read and the established shell activity. It did not, on its own, reveal every byte read from the file or every credential available to the process.

**Practical lesson:** describe the command, its parent and the scope of the correlation. Avoid using the executable name as the sole basis for a verdict.

## 2. Separating command execution from captured output

The process records included configuration discovery and an environment-enumeration command. They established that those commands ran. They did not include the environment command's output.

A separate agent log contained a narrative of configuration contents. That was useful as a lead, but required a different confidence level from raw process or API telemetry. In particular, an executed environment query did not establish that additional environment credentials had been discovered.

**Practical lesson:** keep “command executed,” “output captured,” and “sensitive data confirmed” as separate statements.

## 3. Comparing application SQL with broad collection

Database Syslog exposed differences in statement structure. Normal application queries often selected named columns and used scoped predicates, parameters or limits. Other statements selected entire tables or enumerated database catalogs.

The distinction was the content and purpose of the statements in context, rather than the service-account name alone. Both application and suspicious activity could use the same database login and originate from the portal host.

The observed statement set supported a finding of broad querying and discovery. SQL text alone did not prove how many records were returned or which records were later placed in an archive.

**Practical lesson:** a shared identity increases the need for lineage and behavioural context. It does not make every use of that identity malicious.

## 4. Testing success claims against independent records

The agent narrative reported successful retrieval of sensitive material from an administrative API. The host's API log instead recorded an access-denied response for the request associated with the suspicious activity.

That response contradicted the success claim for the observed request. The appropriate finding was a denied attempt, not confirmed extraction. It was not a basis to conclude that the entire environment was free of other successful attempts.

A second agent claim described finding privileged credentials in a registry image. Registry logs supported authentication and enumeration activity, including manifest and layer-related access. They did not expose credential-bearing image contents. Those operations supported a narrower finding than the narrative claimed.

**Practical lesson:** an agent statement is a source to evaluate. A successful HTTP request or an enumeration operation is not proof that a particular secret was found.

## 5. Following preparation and outbound activity

Endpoint process records showed an archive/compression/encryption sequence, followed by an upload command. A network event connected an external HTTPS destination to the established interpreter process and its privileged execution context.

Legitimate outbound HTTPS traffic also existed. Port 443 was therefore not the useful discriminator. Process identity, account context, command arguments and the established parent chain made the suspicious outbound activity distinct.

The evidence supported archive preparation, an upload command and an associated connection. The agent log additionally reported upload success, and the platform accepted the exfiltration finding. No packet capture, destination-side receipt or independent archive inspection was collected in this session; byte-level completeness of the transfer remains outside the evidence reviewed.

**Practical lesson:** specify the evidence for a transfer separately from the evidence for its exact contents and completion.

## 6. Assessing persistence-related changes

Process records showed a new account being created, privileged group membership being added, an SSH directory being prepared and a key being generated. Together these supported persistent-access preparation in the lab.

The inspected rows did not independently demonstrate that a key was installed for authentication or that a later login succeeded. Those would require file and authentication evidence beyond the commands already observed.

**Practical lesson:** distinguish creation and preparation from successful re-entry.

## 7. Testing automation and human involvement

The timeline contained short bursts of commands separated by much longer pauses. Within one sequence, commands were seconds apart; explicit operator inputs described strategic transitions between phases.

The combined evidence supported a human-directed workflow with automated execution inside phases. Timing alone would not prove use of a particular AI model. The agent records and operator directives supplied additional context, while the conflicting success narratives showed why those records could not substitute for independent event evidence.

This use of offensive AI capabilities is consistent with the subject described by [MITRE ATT&CK T1588.007](https://attack.mitre.org/techniques/T1588/007/). That reference provides a classification; it does not establish attribution to a real threat actor.

## 8. Disconfirming plausible false positives

Two final hypotheses required restraint:

| Hypothesis | Evidence reviewed | Bounded conclusion |
|---|---|---|
| A database backup using the same database login belonged to the intrusion | The same backup command, script parent and local destination appeared in lead-in records; a separate dated sample showed near-nightly execution | The reviewed pattern was consistent with established backup activity. Mixed-run data limited exact cadence claims |
| A connection spike was another enumeration burst | Database messages reported too many clients and reserved connection slots; the content differed from broad collection statements | The spike was consistent with capacity exhaustion. The platform identified an application concurrency problem |

The historical record count was not treated as a count of unique executions because duplicate records were present. A normal explanation for these observations also did not establish that all backup or database activity was benign.

## Outcome and next steps

The platform recorded full, on-time completion. The more useful portfolio outcome is an evidence chain that distinguishes observed behaviour, reported outcomes and missing visibility.

Potential follow-up work includes validating process-to-network correlation against stable process identifiers, checking authentication records for later re-entry, and testing detection ideas against a clean benign baseline. Those are proposed next steps, not completed remediation or deployed controls.

See the [evidence matrix](evidence-matrix.md), [query library](../queries/README.md) and [method limitations](method-and-limitations.md) for the supporting structure.

