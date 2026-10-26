# Hardware Inventory and Asset Lifecycle Management

**Project type:** Hardware Asset Management / Infrastructure Operations  
**Environment:** Independently operated technology training environment  
**Inventory revision:** October 26, 2026  
**Scope:** Physical network, compute and power assets; linked virtual platforms and maintenance records  
**Verification:** Consolidated from dated operational documentation; not a new physical inventory or live firmware audit.

## Overview

I maintain an asset register to track equipment roles, operational status, maintenance events, incident history and lifecycle decisions. This register helps distinguish **physical equipment** from the virtual machines and software services it hosts.

The full operational inventory remains private. This portfolio version excludes serial numbers, internal addressing, precise locations, management endpoints, credentials and sensitive network topology.

## Physical hardware register

| Asset ID | Equipment | Role | Documented lifecycle state |
|---|---|---|---|
| NET-001 | Protectli appliance / OPNsense | Firewall and routing | Active |
| NET-002 | Aruba 2930F 24G PoE | Managed core switching | Active |
| NET-003 | UniFi U6 Enterprise access points | Wireless connectivity | Active; 3 |
| SRV-001 | HP Z2 Tower G4 | Virtualization host | Active |
| END-001 | Lenovo ThinkPad T14 Gen 3 | Primary administrator workstation | Active |
| END-002 | Lenovo ThinkPad T480 | Secondary workstation / test system | Active |
| PWR-001 | Core-network UPS | Network backup power | Active; model and telemetry to verify |
| NET-004 | MikroTik CRS305 | Former network intermediary | Retired from active service |

**Note:** Asset IDs are documentation identifiers, not serial numbers or confirmed physical asset tags. “Active” reflects the latest reviewed operating records, not an October 26 physical count.

### Virtual and software inventory (separate from physical assets)

| Platform or service | Relationship | Documented function |
|---|---|---|
| Proxmox VE | Runs on HP Z2 | Virtualization |
| Docker host VM | Hosted on Proxmox | Container platform |
| Zabbix VM | Hosted in virtual environment | Infrastructure monitoring |
| UniFi Network Application | Containerized management service | Wireless management |
| Grafana / InfluxDB / Telegraf | Containerized services | Telemetry and visualization |
| Portainer | Containerized service | Container management |
| LibreNMS | Monitoring service | Network device monitoring |

## Maintenance and incident history

Dated maintenance evidence is linked to the relevant physical or virtual asset rather than counted as additional hardware.

| Date | Asset / system | Work performed | Verified result |
|---|---|---|---|
| April 1, 2026 | Lenovo T14 | Hardware profile and system inventory | Operational profile documented |
| June 1, 2026 | Zabbix VM | Version 7.0.23 → 7.0.26, reboot and service checks | Services recovered |
| June 1, 2026 | Firewall / OPNsense | 111-package platform update and dependency checks | [Maintenance validation passed](../01_Change_Management/Firewall_Maintenance.md) |
| June 1, 2026 | Aruba switch / UniFi | Switch configuration capture; controller backup and version check | Baselines and backups documented; **not** a switch firmware upgrade |
| June 1, 2026 | Proxmox | Update inventory and risk assessment | Update **deferred**; not recorded as completed |
| June 3, 2026 | Docker host VM | OS, container runtime and kernel updates; host reboot | Containers and management services recovered |
| July 16, 2026 | Network infrastructure | Fiber ISP migration, configuration protection and validation | [Cutover validated](../01_Change_Management/Fiber_ISP_Migration.md) |
| August 19, 2026 | Firewall, switch, core UPS | Utility-power failure, network outage and recovery | [Service restored and root cause documented](../02_Incident_Response/Network_Power_Failure.md) |
| Date not recorded | MikroTik CRS305 | Vendor recovery attempts | Unsuccessful; removed from active topology |

**Version discipline:** Recorded software versions describe the **date of maintenance**, not necessarily the currently installed version.

## Asset management workflow

```text
Operational Requirement
        ↓
Check Existing Inventory / Reuse
        ↓
Acquire Only If Justified
        ↓
Register Asset and Baseline
        ↓
Deploy and Verify
        ↓
Monitor / Maintain / Link Incident Records
        ↓
Repair, Reassign or Retire
        ↓
Update Inventory and Disposition
```

The private register also accommodates serial number, purchase date, warranty, precise location, firmware, maintenance reference, incident reference, retirement date and disposal status when those details are verified.

## Engineering principles

### “Pretty, Not Practical” — equipment selection

Choose equipment for demonstrated requirements, compatibility, reliability, security, maintainability and total cost. Reuse existing equipment when it meets the need without introducing avoidable overhead or vulnerabilities. Brand prestige or visual polish is not a procurement requirement.

### “Enough” — repair and lifecycle decisions

Continued troubleshooting has a cost. For the MikroTik CRS305, vendor-supported recovery attempts were unsuccessful. Continuing to invest time in the switch became an operational sinkhole when the connectivity objective could be met through a simpler direct **10 Gb island** design without keeping the switch as an intermediary.

The device was removed from the active topology. The exact underlying hardware or bootloader fault was **not** conclusively established. The direct 10 Gb design is presented as the chosen architectural direction, not as proof that every planned link has already been deployed.

## Accuracy and reconciliation

This inventory is based on a consolidated review of existing documentation. It does **not** claim a completed physical count, current warranty audit or current firmware verification.

Next reconciliation items include equipment quantities, UPS models and battery condition, current OS/firmware, retired-device disposition and actual deployment status of proposed 10 Gb links.

## Professional skills demonstrated

Hardware inventory organization, asset classification, equipment lifecycle tracking, dated maintenance history, incident-to-asset traceability, repair-versus-replace judgment, documentation control, and cost-conscious infrastructure operations.

---

*Sanitized portfolio summary derived from private asset and maintenance records. This is an independently operated environment, not a commercial hospital or medical-device fleet.*
