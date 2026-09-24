# Dependency-Aware Patch Order

Preserve management plane first.

1. **Critical infrastructure the workflow itself depends on** (very controlled windows):
   - DNS / identity (AD / IdM)
   - Network core / firewall that provides management access
   - Hyper-V hosts (before their guests when host patching is required)
   - Satellite and AAP controllers themselves (out-of-band recovery ready)

2. **Management and content sources** – Satellite (so errata remain available), then AAP if needed.

3. **Pilot wave** – One low-risk Windows VM + one low-risk Linux VM (non-DC, non-DB, non-identity, no critical services). Explicit approval, full precheck + checkpoint (Windows), recovery path documented.

4. **Later waves** – Group by dependency and risk (app tiers after infrastructure, stateful services with stricter recovery rules). Stop the wave on precheck or validation failure.

5. **Always** – One clear owner per patch action so Satellite, Windows Update, and Ansible do not race. Reboot handling and idempotent re-run built in.

Exact order refined after Phase 1 inventory & dependency map is complete.
