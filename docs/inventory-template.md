# Phase 1 – Inventory and Dependency Map Template

For each asset capture:

| Field | Description |
|-------|-------------|
| Asset name / ID | |
| Owner / source of inventory | Satellite, Hyper-V, manual, etc. |
| Current version / build | |
| Update channel | Satellite CV, Windows Update, vendor, etc. |
| Role | e.g. domain controller, app server, Hyper-V host, switch |
| Maintenance window | |
| Management path | SSH, WinRM, HTTPS API, console, OOB |
| Dependencies | What this asset depends on; what depends on it |
| Recovery method | Checkpoint, backup, config export, rebuild, etc. |
| Must remain available for workflow? | Yes/No – especially AAP, Satellite, DNS, identity, network, Hyper-V hosts |

## Critical services that must stay up to run or validate the workflow

- AAP controller / execution environments
- Satellite
- DNS
- Identity (AD / IdM)
- Network access / management plane
- Hyper-V hosts

Fill this document collaboratively before any discovery automation is written.
