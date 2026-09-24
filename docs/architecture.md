# Proposed Architecture

## Overview

Mono-repo orchestration layer on Ansible Automation Platform 2.7.

- **Discovery** feeds assessment only (never auto-install).
- **ServiceNow Change Request** captures affected assets, update identifiers, risk, planned order, maintenance window, prechecks, recovery instructions, and results.
- **Explicit approval gate** before any disruptive action.
- **Prechecks + recovery preparation** (including Hyper-V production checkpoints for guests).
- **Ordered execution** with single owner per patch action.
- **Validation** distinguishes successful installation from healthy service.
- **Failure handling** stops the wave; recovery options are documented, not assumed.

## Key design decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Repo layout | Mono-repo for POC | Single owner, tightly coupled workflow, simpler AAP project management |
| Event source | Scheduled AAP jobs first | More reliable for lab than EDA event sources initially |
| Ticketing | ServiceNow PDI | Exercise only; durable evidence outside PDI |
| RHEL patch owner | Satellite | Native content lifecycle and errata |
| Windows patch owner | Ansible `win_updates` or native | Avoid competing tools |
| VM recovery | Hyper-V production checkpoint (short-lived) | Application-consistent; not a substitute for backup |
| Credentials | AAP credential types + vault | Scoped, audited, never extra-vars as connection identity |

## ServiceNow PDI realities

- Reclaimed after ~10 days of inactivity on the developer portal.
- No production/business use; no support.
- All durable artifacts (playbooks, evidence, workflow definition) live in this repo + AAP + local evidence store.
- If PDI is reclaimed, recreate CR against a new instance or continue with local evidence only.

## Future split criteria

Consider multi-repo only when:
- Multiple maintainers with independent release cadences, or
- Platform-specific content becomes large enough to justify separate AAP projects.
