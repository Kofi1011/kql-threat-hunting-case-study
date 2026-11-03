# Evidence matrix

This table summarizes observations reviewed in the lab. Host names, credentials and exact answer values are omitted. “Observed” refers to the recorded browser investigation; it does not imply that raw exports are included here.

| ID | Observation | Source and fields | Supported conclusion | What remains unproven |
|---|---|---|---|---|
| E01 | Configuration-reading utility descended from the established interpreter | `DeviceProcessEvents_CL`: `ProcessId`, `InitiatingProcessId`, `ProcessCommandLine` | File-read command belongs to the scoped process chain | Complete file contents and every secret read |
| E02 | Environment-enumeration command executed | `DeviceProcessEvents_CL.ProcessCommandLine` | Enumeration attempted | Environment output and additional credential discovery |
| E03 | Broad table queries and catalog enumeration differed from scoped application queries | `Syslog.SyslogMessage` | Suspicious statement structure and discovery behaviour | Returned row counts and final archive membership |
| E04 | Registry operations differed from normal CI operations and source | `ContainerRegistryLog_CL`: `SrcIpAddr`, `Operation`, `Resource`, `HttpStatusCode` | Authenticated registry enumeration | Claimed embedded privileged credentials |
| E05 | Administrative API returned access denied for the suspicious request | `Syslog.SyslogMessage` | This request did not support the agent's secret-extraction claim | Whether any other attempt succeeded |
| E06 | Archive, compression and encryption commands appeared in sequence | `DeviceProcessEvents_CL`: `TimeGenerated`, `ProcessCommandLine` | Staging commands executed | Actual archive contents and successful decryption |
| E07 | External HTTPS event linked to the suspicious privileged process | `DeviceNetworkEvents_CL`: `RemoteIP`, `RemotePort`, `InitiatingProcessId`, `InitiatingProcessAccountName` | Outbound connection associated with the chain | Destination-side receipt and exact bytes transferred |
| E08 | New account, group and SSH-key preparation commands appeared | `DeviceProcessEvents_CL.ProcessCommandLine` | Persistent-access preparation | Key installation and subsequent authenticated login |
| E09 | Scan sources had different signatures from the known internal scanner | `PortalAccessLog_CL`: `SrcIpAddr`, `UserAgent`, `UrlOriginal`, `TimeGenerated` | Contextual separation of external probing and internal scanning | Actor identity based on user agent alone |
| E10 | Operator directives accompanied rapid command bursts | `LLMAgentLogs_CL.user_input`; `DeviceProcessEvents_CL.TimeGenerated` | Human strategic control and automated intra-phase execution | Fully autonomous campaign or model identity from timing alone |
| E11 | Backup command, parent and destination existed in lead-in data | `DeviceProcessEvents_CL`: `ProcessCommandLine`, `InitiatingProcessCommandLine`, `AccountName` | Pre-existing backup pattern | Uniform cadence across every mixed-run record |
| E12 | Spike included database connection-capacity errors | `Syslog.SyslogMessage` | Capacity-exhaustion explanation supported | Full production root-cause analysis or remediation |
| E13 | Hunt UI showed full completion | CyberRange completion header | 28 flags, 8 sections, full score, on-time completion | Certification, production impact or an independently graded skill assessment |

The original records contained mixed run identifiers, dates and duplicate-looking rows. This matrix is a collection of scoped observations, not a claim that every row belongs to one deduplicated incident timeline.

