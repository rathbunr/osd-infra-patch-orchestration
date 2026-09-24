# Small Pilot – One Low-Risk Windows VM + One Linux VM

## Assumptions (to be confirmed)

- Working AAP 2.7, Satellite 6.19, Hyper-V on Windows Server 2025, ServiceNow PDI.
- Two pilot VMs are non-critical, have known current versions, reachable management paths (WinRM/SSH), sufficient free space.
- Satellite already knows the Linux VM and has applicable errata visible.
- Windows VM is a Hyper-V guest (production checkpoint feasible).
- No competing patch tools actively managing these two VMs.
- Credentials available (or will be added under approval) in AAP.

## Unresolved (Phase 1)

Exact models/versions of non-VM assets, current maintenance windows, precise recovery methods, services that must stay up for the workflow itself, full dependency graph.

## Success criteria (after explicit approval)

- Discovery detects available updates/errata for both VMs.
- ServiceNow change request created with correct affected assets, identifiers, risk, order, prechecks, recovery notes.
- Explicit approval gate enforced.
- Prechecks pass (or clearly fail and stop).
- Windows VM: production checkpoint created and verified.
- Patch applied under designated owner, with reboot handling.
- Validation confirms intended versions + service health.
- Change record updated with evidence; checkpoint cleaned after observation window (or retained per policy).

## Failure paths to review before implementation

- Satellite/API unreachable or errata query fails.
- ServiceNow PDI reclaimed or API/auth fails.
- Checkpoint creation fails or is not production type.
- Patch applies but validation (service health) fails.
- Reboot hangs or connectivity lost mid-wave.
- Idempotent re-run needed after partial failure.

No implementation until these paths are reviewed with the owner.
