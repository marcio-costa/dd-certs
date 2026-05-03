# Domain 3 – Security Monitoring and Threat Detection

> Sources: Detection Rules docs, Custom Detection Rules guides, Security Signals Explorer, MITRE ATT&CK Map, Cloud SIEM Investigator.

## 3.1 Detection rule components

A detection rule has the following components, regardless of type:
- **Query** (root + cases) — Log Explorer search syntax. Example: `source:cloudtrail @evt.name:ConsoleLogin`.
- **Group by** — attributes used to bucket events (e.g., `@usr.id`, `@network.client.ip`).
- **Trigger / threshold** — count, rolling window, "for each new value", etc.
- **Rule cases** — boolean expressions on the aggregated counts that produce signals at different severities. A rule can have multiple cases (e.g., 1 failure = Info, 5 in 5 min = Medium, 20 in 5 min = High).
- **Severity** — Info, Low, Medium, High, Critical.
- **Suppression / Exclusion** — list of conditions where the rule should not fire (e.g., known service accounts).
- **MITRE ATT&CK tactic + technique** mapping.
- **Notification message** with template variables — e.g.,
  ```
  {{@usr.id}} just logged in without MFA from {{@network.client.ip}}.
  ```
- **Playbook** — free-text remediation steps shown next to the signal.

## 3.2 Detection rule types

Cloud SIEM supports several rule types (these are the ones you must recognize):

| Type | Behavior |
|---|---|
| **Log Detection — Threshold** | Standard count over a rolling window. |
| **Log Detection — New Value** | Fires when a previously-unseen value appears for a chosen attribute (e.g., new IP for a user, new region for an IAM role). |
| **Log Detection — Anomaly** | Statistical baseline; fires when a metric deviates significantly. |
| **Log Detection — Impossible Travel** | Fires when the same user logs in from two geographies in a window too short to be physically possible. |
| **Signal Correlation** | Combines two or more existing signals into a higher-fidelity signal. |
| **Third Party** | Constructs a root query from third-party logs/events; in the *Trigger for each new* dropdown you select attributes that each generate a signal per new value over a 24-hour roll-up window. |
| **Application Security Rule** | (AAP, mostly out of SIEM scope but appears as a category.) |
| **Cloud Configuration / Infrastructure Configuration** | Used by Cloud Security Misconfigurations, NOT Cloud SIEM. Don't confuse them. |

## 3.3 Out-of-the-Box (OOTB) Rule Management

- Cloud SIEM ships **hundreds of OOTB rules** automatically applied after Cloud SIEM is enabled and Content Packs activated.
- OOTB rules are read-only by default but can be **cloned** to create an editable copy, or **edited in place** to extend the query, change severity, or modify the playbook.
- Rules can be **enabled/disabled**, scoped to specific environments or accounts via tags.
- Each OOTB rule lists its source integrations and required log fields.

### Tuning
- **Suppress** a rule for a list of identities (e.g., known automation users) instead of disabling.
- **Adjust severity thresholds** to match your environment risk tolerance.
- Always **test changes** by running the rule's query against historical logs in the Log Explorer first.

## 3.4 Custom Rule Development and Enhancement

Two starting points:
1. **Clone an OOTB rule** and modify — preserves MITRE annotations and structure.
2. **Create from scratch** in Security → Detection Rules → New Rule.

Best-practice workflow:
1. Identify a real attack pattern (or a gap in OOTB coverage shown by the MITRE ATT&CK Map).
2. Write the **root query** in Log Explorer first; verify it returns expected logs.
3. Add **group-by attributes** that scope the detection to entities that matter (user, role, IP).
4. Pick a **trigger** that minimizes noise: threshold for known counts, new-value for first-time activity, anomaly for statistical drift.
5. Define **rule cases** in increasing severity.
6. Add **MITRE tactic + technique**.
7. Write a clear **notification message** with template variables.
8. Set the **playbook** with concrete first-response steps.
9. **Validate** with historical data using the rule preview / "Test Now" — see §3.6.

## 3.5 Signal Investigation and Triage

Each Security Signal includes:
- The matching logs and aggregated values
- Severity and MITRE annotations
- Linked **Investigator** view (graph of related entities)
- **Status** — Open → Under Review → Archived (Resolved or False Positive)
- **Assignee** for collaboration
- Playbook and any related signals (correlation parents/children)

### Investigator
A graphical interface that lets you pivot from a signal to the affected user/resource and visualize all related activity over time. Use it to answer: *what did this user/role do before and after the signal fired?*

### Bits AI Security Analyst
- Automatically pre-investigates new signals and labels them **Investigating → Benign / Suspicious**.
- Available in the Security Signals Explorer via the **Bits AI Security Analyst** tab.
- For Slack/Jira notifications generated from Cloud SIEM, Bits AI replies on the same thread with its conclusion and a link to the full investigation.
- From the Bits AI Investigation side panel you can: create a Case with pre-populated context, run a SOAR workflow (blueprint), declare an incident, add a rule suppression, archive the signal, or open the standard Cloud SIEM view. You can also leave feedback to improve future investigations.

## 3.6 Rule Testing and Validation

- **Run a query against historical logs** to estimate signal volume before enabling a new rule.
- Use **rule preview** in the rule editor to see synthetic matches.
- After enabling, monitor the rule's **signal count** over the first 24–72h. If a new rule produces more than a few signals/day per typical environment, it's almost certainly noisy and needs threshold tuning.
- Verify **template variables** render correctly in the notification by triggering a test event.

## 3.7 Threat Hunting and Detection Coverage

### MITRE ATT&CK Map
- Found in Security → Cloud SIEM → MITRE ATT&CK Map.
- Maps your active OOTB and custom rules onto the ATT&CK tactics × techniques matrix.
- Use it to **identify gaps** (techniques with no rules) and prioritize new rule creation.

### Threat hunting
- Use Log Explorer with security-relevant queries (CloudTrail, GuardDuty findings, VPC Flow) to investigate hypothesis-driven activity.
- Pivot via the **Investigator** on any user/role/IP of interest.
- Save successful hunts as detection rules to operationalize them.
