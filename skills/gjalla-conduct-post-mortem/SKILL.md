---
name: gjalla-conduct-post-mortem
description: Run a blameless incident analysis with a timeline, 5 Whys root-cause analysis, and action items. Use after a production incident to find the root cause and prevent recurrence.
---

# Blameless Post-Mortem

Conduct a structured incident analysis:

## Process
1. **Identify affected components**: Determine which parts of the system were impacted and how they connect. (If you use gjalla, `gjalla state show -c architecture` surfaces affected elements and their connections.)
2. **Gather timeline**: Collect timestamps from logs, alerts, and team communications.

## Post-Mortem Template

### Incident Summary
- **Date/Time**: When did it start and end?
- **Duration**: Total impact time.
- **Severity**: P1-P4.
- **Affected components**: List the components involved.

### Timeline
Chronological list of events from detection to resolution.

### 5 Whys Analysis
1. Why did the incident occur? (Proximate cause)
2. Why did that happen? (Contributing factor)
3. Why did that happen? (Deeper cause)
4. Why did that happen? (Systemic issue)
5. Why did that happen? (Root cause)

### Action Items
For each action item:
- Description of the fix or improvement
- Owner
- Priority (P1 = this week, P2 = this sprint, P3 = this quarter)
- Whether it should become a new project rule

### Lessons Learned
- What went well in the response?
- What could be improved?
- New rules or constraints to add to the project.
