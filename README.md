# Healthcare Technology Operations Portfolio

**Infrastructure operations • Hardware asset management • Incident response • Change control • Technical documentation**

This portfolio documents hands-on technical work performed in an **independently operated, production-use technology training environment**. It is intended to demonstrate practical, transferable skills relevant to healthcare technology support, field service, technical training, and infrastructure operations.

My professional background includes clinical practice, healthcare education, and operating a small healthcare business. I am transitioning into healthcare technology with the goal of supporting the clinicians and care teams who deliver patient care.

> **Scope:** These are independently managed infrastructure projects, **not** claims of paid enterprise IT employment, hospital IT administration, or medical-device repair experience. Case studies have been sanitized to remove sensitive network and household details.

## Featured work

| Project | Competencies demonstrated | Case study |
|---|---|---|
| Fiber ISP Migration | Planned change, backups, controlled cutover, verification | [Read case study](01_Change_Management/Fiber_ISP_Migration.md) |
| Scheduled Firewall Maintenance | Security patching, maintenance windows, service validation, monitoring | [Read case study](01_Change_Management/Firewall_Maintenance.md) |
| Network Power Failure | Incident response, Linux diagnostics, root-cause analysis, recovery | [Read case study](02_Incident_Response/Network_Power_Failure.md) |
| MikroTik CRS305 Hardware Recovery | Hardware recovery, vendor procedures, repair-versus-retire judgment | [Read case study](02_Incident_Response/Hardware_Recovery.md) |
| Hardware Inventory & Asset Lifecycle | Asset register, maintenance history, lifecycle status, equipment accountability | [View asset inventory](03_Hardware_and_Infrastructure/Asset_Management.md) |

## How the work is organized

- **[01 — Change Management](01_Change_Management/):** Planned infrastructure changes, maintenance, backups and post-change verification.
- **[02 — Incident Response](02_Incident_Response/):** Troubleshooting, root-cause analysis, service restoration and recovery decisions.
- **[03 — Hardware & Infrastructure](03_Hardware_and_Infrastructure/):** Sanitized equipment inventory, lifecycle tracking and dated maintenance history.
- **[04 — Operational Procedures](04_Operational_Procedures/):** Operational procedures and runbooks as they are reviewed for publication.

## Operational approach

### Verify the system, not just the task

An update is not complete simply because a package installer reports success. A recovery is not complete merely because a device powers on. I verify dependent services, observe monitoring data, and document the actual result.

### “Enough”

Technical troubleshooting has diminishing returns. When repeated recovery attempts are no longer justified—and a simpler architecture meets the need—I stop the unproductive work and document the lifecycle decision. The [MikroTik case study](02_Incident_Response/Hardware_Recovery.md) illustrates this approach.

### “Pretty, Not Practical”

Choose equipment and tools based on demonstrated functional value, reliability, compatibility, security, maintainability and total cost—not branding or visual appeal. Reuse existing capabilities where they satisfy the requirement without adding avoidable overhead.

### Preserve evidence and distinguish certainty

Case studies distinguish completed actions from plans, confirmed findings from hypotheses, and dated observations from current-state claims. Public documents omit credentials, private addresses, device serial numbers and sensitive configuration details.

## Technology experience

**Networking:** OPNsense, Aruba managed switching, UniFi wireless, VLANs, routing, DNS, DHCP and VPN connectivity.  
**Systems:** Ubuntu Linux, Proxmox VE, Docker and containerized infrastructure services.  
**Observability:** Zabbix, LibreNMS, Grafana, InfluxDB and Telegraf.  
**Operations:** Asset inventories, change windows, configuration backups, incident documentation, maintenance verification and lifecycle decisions.

## Why healthcare technology

My career has centered on clinical work, education and helping people navigate complex health-related problems. I am applying that experience to a technical role focused on supporting healthcare professionals, maintaining dependable systems, and helping care teams deliver effective patient care.

---

**Portfolio note:** The public asset inventory is a curated, sanitized view of a more detailed private master register. It is based on dated records and does not imply that every listed firmware version or physical asset has been independently re-audited on the publication date.
