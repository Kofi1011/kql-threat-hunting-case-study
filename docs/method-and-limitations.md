# Method, provenance and limitations

## Working method

1. State the hypothesis before deciding a verdict.
2. Confirm the current workspace, table and schema.
3. Apply a bounded time filter and exact host filter.
4. Inspect result fields before adding projections that assume columns exist.
5. Trace relationships using host, process identifiers and time scope.
6. Compare the result with a lead-in baseline and a competing explanation.
7. Record whether the conclusion is observed, reported, inferred or unresolved.

## Timestamp discipline

The browser rendered some event dates in local time while the query filter used UTC. Explicit formatted UTC strings helped avoid interpreting a display offset as a different event.

Custom lab tables used `TimeGenerated`; they are not identical to standard Defender XDR schemas. Do not blindly replace `TimeGenerated` with `Timestamp` or assume that one always represents ingestion and the other occurrence. Inspect the table's documented meaning and compare both when available. Microsoft notes that differences can exist when Defender data is streamed to Log Analytics: [Advanced hunting with Sentinel data](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-microsoft-defender).

## Mixed runs

The inspected dataset contained September endpoint records and October agent/registry records with different run labels. Some rows had no visible run identifier. The old run filter excluded useful records, so a hunt-name/host inventory was used to identify available data.

This makes cross-run comparison useful for understanding the exercise, but insufficient to establish one clean forensic timeline. The report therefore separates observations by their evidence category rather than silently combining all timestamps.

## Duplicate rows and PIDs

Repeated projected rows appeared. `distinct` can remove identical projections for readability, but it can also collapse separate events that share those projected values. Preserve the raw record identity before counting unique events.

Process IDs can be reused. Correlation needs a stable device identity, a short time window and, when available, process creation time or a stable process identifier. The supplied lineage template is a triage starting point, not a production-grade attribution rule.

## Query status

The original investigation included successful executions, zero-result pivots and corrected syntax/schema errors. A query marked Completed proves it ran, not that it returned useful records.

Public `.kql` files are sanitized, parameterized adaptations prepared for this repository. They were reviewed for schema assumptions and structure but have not been rerun against a connected workspace in this form. No performance or detection-quality claim is made.

## Scope and claims

- Simulated lab; no production incident or customer data is claimed.
- Full platform score is not equivalent to independent forensic validation of every statement.
- Agent output is not automatically true, and a successful operation name does not establish its claimed result.
- No production detections were deployed, no system was remediated, and no reduced-risk percentage was measured.
- No raw file export, packet capture or destination-side transfer proof was collected in the recorded session.

## Publication boundaries

The exercise belongs to CyberRange. This package excludes the exact flag sequence, lab passwords, tenant identifiers, raw log exports and screenshots containing answer material. Host names and example configuration are generalized. A provider policy allowing full solution publication was not verified; this is a method-focused case study rather than a solution dump.

