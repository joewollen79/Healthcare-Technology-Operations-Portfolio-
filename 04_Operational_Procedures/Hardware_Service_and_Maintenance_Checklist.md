# Hardware Service and Maintenance — Operational Checklist
### Asset Accountability • Fault Isolation • Controlled Change • Verification • Lifecycle Closure

**Document type:** Reusable operational procedure / field-service checklist  
**Status:** Procedure derived from documented Training Center practices; not a claim of professional medical-device service qualification  
**Environment:** Independently operated technology training environment  
**Applicable to:** Network appliances, managed switches, wireless access points, compute hosts, UPS equipment, and supporting infrastructure  
**Revision:** October 26, 2026

> **Safety boundary:** Follow manufacturer procedures, site authorization, electrical safety requirements, and organizational policies. Do not open energized equipment or perform internal UPS/battery repairs without appropriate qualification. Healthcare or patient-connected equipment requires its own manufacturer-approved service procedures, infection-control requirements, safety testing, and release authorization; this checklist does **not** replace them.

## 1. Purpose and service outcome

Use a repeatable process to identify equipment, understand the reported failure or maintenance need, protect the existing operational state, isolate the smallest useful fault domain, execute authorized service, and **prove** the required function before closing work.

A successful service event may result in **restored**, **verified healthy**, **referred/escalated**, **deferred**, or **retired/replaced** equipment. A repair is not always required for a technically sound outcome.

## 2. Service record — complete before work

| Field | Entry |
|---|---|
| Ticket / work-order ID | |
| Date / start time / time zone | |
| Technician / responsible person | |
| Requestor / affected service | |
| Asset ID / manufacturer / model | |
| Serial / asset tag (private record only) | |
| Location / owner (private record only) | |
| Current operational state | |
| Reported symptom or planned maintenance | |
| Impact / priority / safety considerations | |
| Change authorization and maintenance window | |
| Related incident / maintenance record | |
| Expected normal behavior / acceptance criteria | |

**Asset rule:** Identify the physical device before modifying it. Confirm that the asset record matches the equipment and record any discrepancies. Virtual machines and hosted applications are linked dependencies, not additional physical hardware assets.

## 3. Initial safety, physical inspection and scope

- [ ] Confirm work authorization, service ownership and expected impact.
- [ ] Determine whether any patient-care, critical communications or other high-consequence service depends on this equipment; obtain the required coordination.
- [ ] Review manufacturer documentation and applicable safety procedures.
- [ ] Inspect for damage, liquid ingress, unusual heat/odor, damaged connectors or cables, loose power connections and abnormal LEDs **without exposing energized internals**.
- [ ] Confirm power source, UPS behavior, cable seating and link indicators where safe.
- [ ] Check for obvious environmental or upstream causes before assuming the device itself has failed.
- [ ] Identify connected systems, dependencies and potential blast radius.
- [ ] Define **in scope / out of scope**; avoid unrelated changes.

**Document:** What was observed, not merely what was suspected.

## 4. Establish a known-good baseline

- [ ] Record reported symptoms, timestamps and whether the problem is reproducible.
- [ ] Compare against a known-good device or earlier baseline when available.
- [ ] Record device reachability, physical link state, power state, service state, firmware/OS version and relevant logs.
- [ ] Check monitoring history for the event window and any preceding warning signs.
- [ ] Distinguish **physical connectivity**, **network reachability**, **management access** and **application function**; one does not prove another.
- [ ] State a measurable acceptance test before proceeding.

**Evidence sources:** Device LEDs and physical inspection; management interface; system logs; switch MAC table; DHCP/ARP/neighbor information; interface error counters; Grafana, Zabbix or LibreNMS; direct client or service testing.

**Diagnostic rule:** Each test should answer a question. Do not run commands or reset devices simply because those options exist.

## 5. Protect state and plan recovery

For significant changes, and whenever supported by the equipment:

- [ ] Export or verify the current device configuration.
- [ ] Confirm relevant snapshots/backups and off-host copies where appropriate.
- [ ] Identify the rollback method, dependencies and realistic restoration time.
- [ ] Confirm that backups **exist**; separately record whether restore testing has actually been performed.
- [ ] Ensure a recovery access path is available if normal management fails.
- [ ] Record version dependencies, vendor advisories and release notes for planned updates.
- [ ] Make a **go/no-go** decision based on impact, protection and recovery readiness.

**Do not store** raw configuration backups containing credentials, keys, certificates or sensitive addresses in a public repository.

## 6. Fault isolation — least disruptive first

Use the minimum testing needed to reduce uncertainty:

| Fault domain | Example questions / evidence | Escalation trigger |
|---|---|---|
| Power / physical | Utility power? UPS input/output? LEDs? Cable/port condition? | Unsafe condition, no power, suspected hardware damage |
| Link / Layer 2 | Link negotiated? Switch learns expected MAC? Correct VLAN? Interface errors? | Repeated physical errors or unexpected switching behavior |
| Network / Layer 3 | Valid address? Gateway? Route? Neighbor resolution? | Network path fails while device/link appears healthy |
| Management | SSH/web/controller reachable? Service running? Inform/registration target correct? | Device reachable but management function fails |
| Application / dependency | DNS, authentication, controller, API, HTTPS or client workflow succeeds? | Underlying transport works but service remains unavailable |
| Monitoring | Is telemetry current? Alert triggered? Expected service state known? | Missing or contradictory evidence |

- [ ] Compare failing and healthy paths when feasible.
- [ ] Form the smallest useful hypothesis.
- [ ] Change one relevant variable at a time.
- [ ] Record both confirming and disconfirming evidence.
- [ ] Do not factory-reset, reflash, replace or repeatedly power-cycle without justification and a recovery plan.

**Example from documented practice:** UniFi APs were reachable, had valid DHCP leases and stable switch MAC learning, yet failed controller adoption. Investigation isolated an incorrect controller inform target rather than blaming the AP hardware.

## 7. Perform authorized maintenance or recovery

### Planned maintenance

- [ ] Confirm approved change scope and maintenance window.
- [ ] Confirm pre-change baseline and backups.
- [ ] Apply only the intended update, replacement or configuration adjustment.
- [ ] Record version changes, unexpected prompts, restarts and deviations.
- [ ] Avoid bundling unrelated upgrades.

### Incident recovery

- [ ] Prefer reversible, non-destructive checks.
- [ ] Follow vendor-specific recovery sequence if ordinary methods fail.
- [ ] Stop before potentially destructive actions until data/configuration impact is understood.
- [ ] Escalate when specialist equipment, another owner or unfamiliar procedures are required.

### Apply **Enough**

Ask: **Will another troubleshooting attempt materially improve the likelihood of restoring the required capability?**

If recovery effort becomes disproportionate, preserve the evidence and evaluate repair, vendor escalation, replacement, reuse, or architectural simplification. The MikroTik CRS305 incident illustrates retirement of an unnecessary intermediary rather than indefinite recovery attempts.

## 8. Post-service verification — prove the dependency chain

Validate from the foundation outward, as applicable:

| Order | Verification | Pass criterion |
|---|---|---|
| 1 | Physical / power / device health | Expected indicators, stable power, no new hardware fault |
| 2 | Link and network identity | Correct interface/link state, address and gateway |
| 3 | Routing and infrastructure services | Required connectivity, DHCP/DNS/routing function |
| 4 | Management plane | Authorized administrative access works |
| 5 | Monitoring plane | Current telemetry and expected alerts/events visible |
| 6 | Dependent services | Hosted VMs, containers, controller or application functions restored |
| 7 | End-to-end client workflow | Real user/service operation passes |
| 8 | Negative testing, when relevant | Prohibited access remains blocked and isolation remains intact |

- [ ] Compare against pre-service baseline.
- [ ] Verify expected behavior from **more than one evidence source** when risk warrants it.
- [ ] Confirm that a service is actually usable—not merely marked “running.”
- [ ] Record any expected-but-inactive service separately from an actual failure.
- [ ] Confirm the status of monitoring/alerting after maintenance.
- [ ] Set a proportional observation/burn-in period for changes with delayed risk.
- [ ] Do not mark an observation period **completed** until the evidence exists.

**Examples from dated records:** The August 26 OPNsense major upgrade validated WAN, DNS, DHCP, packet filtering, remote-management VPN and monitoring. The June 3 Docker host maintenance validated host reboot and automatic container recovery while the network forwarding path remained operational.

## 9. Decision and closure

Select the accurate disposition:

- [ ] **Restored and validated**
- [ ] **Maintenance completed and validated**
- [ ] **Operational with known limitations**
- [ ] **Escalated / referred to owner or vendor**
- [ ] **Deferred pending evidence or parts**
- [ ] **Removed from service / retired**
- [ ] **Unresolved, safely documented**

Before closure:

- [ ] Record actions, versions, observations, test results and timestamps.
- [ ] Distinguish confirmed root cause from a working hypothesis.
- [ ] Record any remaining risks or planned corrective actions as **planned**, not implemented.
- [ ] Update the asset's lifecycle status and link the ticket, incident and maintenance record.
- [ ] Record replacement/return/disposal status **only when verified**.
- [ ] Provide an operational handoff: what works, what does not, what changed and who owns follow-up.
- [ ] Record end time and duration.

## 10. Copyable service closeout template

```text
Ticket / Asset ID:
Date / Time:
Reported issue / maintenance objective:
Impact / affected service:
Initial baseline:
Safety and scope checks:
Backup / rollback readiness:
Evidence collected:
Hypothesis (if applicable):
Actions performed:
Positive verification:
Negative testing (if applicable):
Monitoring / burn-in status:
Confirmed root cause (or "not established"):
Disposition:
Asset inventory updated:
Outstanding risks / next owner:
Start / stop / duration:
```

## 11. Source evidence and related portfolio work

This checklist consolidates procedures and decisions found in the private Training Center's:

- Canonical **Operating Doctrines, Protocols and Standards** (September 8, 2026), including **Enough**, **Harmonized Engineering**, **Deterministic Change & Validation**, and **Documentation Completeness**.
- **Backup Strategy** and **Recovery Procedures** (June 1, 2026).
- **UniFi U6 Enterprise Adoption Recovery Runbook** (May 13, 2026).
- **Docker Host Full Maintenance and Recovery Validation** (June 3, 2026).
- **Post-Maintenance Validation Lab** (June 4, 2026).
- **OPNsense Major Upgrade and Post-Upgrade Validation** (August 26, 2026).
- **Network Infrastructure Power Failure** incident report (August 19, 2026).
- **Basement Power Sentinel Deployment** (August 22, 2026), which validated independent loss-of-power alerting without placing the smart plug inline with production power.
- **Jira Incident Response Simulation** (August 2, 2026), emphasizing evidence, scope, ownership and appropriate escalation.

Related public case studies:

- [Hardware Inventory and Asset Lifecycle Management](../03_Hardware_and_Infrastructure/Asset_Management.md)
- [Hardware Recovery — “Enough” Principle](../02_Incident_Response/Hardware_Recovery.md)
- [Network Power Failure](../02_Incident_Response/Network_Power_Failure.md)
- [Scheduled Firewall Maintenance](../01_Change_Management/Firewall_Maintenance.md)
- [Fiber ISP Migration](../01_Change_Management/Fiber_ISP_Migration.md)

---

**Professional scope statement:** This is a reusable general infrastructure service procedure informed by independently performed technical operations. It does not imply prior employment servicing regulated medical equipment or replace employer/vendor-approved healthcare equipment procedures.
