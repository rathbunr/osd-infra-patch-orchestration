# OSD Infra Patch Orchestration

Reusable patch workflow for personal infrastructure lab.

**Goal:** Automate the full process that would otherwise be performed manually: detect available update → assess relevance → open/update change record → prepare recovery path → patch in controlled order → validate → record evidence.

**Status:** Design and read-only discovery only. No automation or system changes until specific changes are discussed and explicitly approved.

## Architecture (POC-first mono-repo)

Single repository for the entire orchestration project. Suitable for personal lab / single owner. Structure supports later split if needed.

```
osd-infra-patch-orchestration/
├── README.md
├── requirements.yml          # Pinned collections
├── ansible.cfg               # Project-local paths
├── playbooks/
│   ├── discovery/             # Update / errata discovery (read-only)
│   ├── change_request/        # ServiceNow CR create/update
│   ├── precheck/              # Health, applicability, recovery prep
│   ├── execute/               # Platform-specific patch actions
│   ├── validate/              # Version + service health checks
│   └── close/                 # Evidence, checkpoint cleanup, CR close
├── roles/                     # Shared helper roles (future)
├── inventories/               # Static / dynamic inventory sources
├── group_vars/ / host_vars/   # Platform and host specifics
├── vars/                      # Common variables, risk models
├── docs/                      # Architecture, matrix, failure paths, inventory map
├── evidence/                  # Local durable evidence templates (outside ServiceNow PDI)
└── .gitignore
```

**Orchestration layer:** Ansible Automation Platform 2.7  
**Content source (RHEL):** Red Hat Satellite 6.19  
**Ticketing (exercise):** ServiceNow Personal Developer Instance  
**Hypervisor:** Hyper-V on Windows Server 2025

## Phases (work through with owner)

1. Inventory and dependency map  
2. Update discovery (Satellite errata, Windows Update data, vendor advisories)  
3. Change record and decision (ServiceNow CR + explicit approval gate)  
4. Prechecks and recovery preparation (including Hyper-V production checkpoints)  
5. Execution (pilot → waves, single owner per action)  
6. Validation (installed version + dependent service health)  
7. Failure and closure (stop wave, document recovery, clean checkpoints)

## Constraints (non-negotiable)

- Prefer maintained Ansible collections, supported APIs, vendor update mechanisms, native backup features.
- Keep TLS validation enabled.
- Use scoped credentials; preserve full audit trail.
- Never assume a credential injected as an Ansible extra variable becomes the connection identity for a delegated host.
- Treat advisories as input for assessment, never as authority to install.
- ServiceNow PDI is ephemeral (reclaimed after ~10 days inactivity). Durable workflow definition and all patch evidence live in Git + AAP artifacts + local evidence store.
- Explicit approval gate before any disruptive work.

## Pilot scope (first controlled execution, after review)

- One low-risk Windows VM (Hyper-V guest)  
- One low-risk Linux / RHEL VM (Satellite-managed)  

Success criteria and failure paths are documented in `docs/`.

## Collections (see requirements.yml)

- `redhat.satellite` – Satellite API / errata / hosts  
- `servicenow.itsm` – Change Request lifecycle  
- `ansible.windows` – Windows updates, Hyper-V interaction via PowerShell  
- Others added only when a documented gap requires them

## Next steps

1. Complete Phase 1 inventory & dependency map (see `docs/inventory-template.md`).  
2. Review architecture, matrix, patch order, and pilot failure paths with owner.  
3. Only after explicit approval: implement read-only discovery playbooks, then controlled pilot.

---
*This repository starts as design scaffolding. No playbooks that modify systems will be committed without prior discussion and approval.*
