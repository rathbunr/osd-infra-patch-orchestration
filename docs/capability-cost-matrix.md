# Per-Platform Capability and Cost Matrix (Lab)

| Platform / Asset type | Update discovery | Patch execution owner | Precheck / Recovery | Validation | Licensing / Cost / Limits | Notes / Gaps |
|-----------------------|------------------|-----------------------|---------------------|------------|---------------------------|--------------|
| RHEL / Linux (Satellite-managed) | Satellite errata (API + redhat.satellite) | Satellite content / errata install | Health, free space, repo provenance | Package versions, outstanding errata, service status | Existing Satellite 6.19 | Strongest native path |
| Windows Server / clients | ansible.windows.win_updates (or WSUS if present) | Ansible or native Windows Update | Free space, pending reboot; Hyper-V production checkpoint for VMs | Installed KBs, service health, WinRM | Existing Windows licenses; collection free | Prefer production checkpoints |
| Hyper-V hosts (WS 2025) | Host-level Windows Update | Ansible / Windows Update | Host health, free space for checkpoints | Host + guest reachability | Existing Windows Server 2025 | Checkpoints via PowerShell / win_powershell |
| Hyper-V guests | Per guest OS | Per guest OS | Production checkpoint before patch; verify + retention | Guest health + dependent services | Free (Hyper-V feature) | Checkpoint ≠ backup; define merge/delete window |
| Cisco / network | Vendor advisories / device API | Vendor or Ansible network collections | Supported config backup, OOB access | Reachability, config integrity | Device support contracts | Exact models/versions still unresolved |
| pfSense | Package/firmware advisories | Native or Ansible | Config backup | Connectivity, firewall rules | Free/open-source | Management path discovery required |
| Printers / WAPs | Vendor firmware | Vendor tools | Config capture if supported | Reachability | Device-dependent | Lowest priority |

All preferred paths use maintained collections or native mechanisms. Custom automation only after a documented gap.
