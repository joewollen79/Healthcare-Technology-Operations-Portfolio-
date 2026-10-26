# Fiber ISP Migration
### Infrastructure Change Management, Backup Verification & Post-Cutover Validation

**Project type:** Network Infrastructure / Change Management  
**Environment:** Independently operated, production-use technology training environment  
**Date:** July 16, 2026, with subsequent cutover validation  
**Outcome:** Successful migration; no migration-induced service failures identified  
**Role:** Infrastructure administrator — planning, execution, verification, and documentation

---

## 1. Executive Summary

Migrated an independently managed network environment to a new fiber Internet service provider while preserving existing network segmentation, infrastructure services, and monitoring capabilities.

The migration required preparation across multiple interconnected systems, including firewall and routing, managed switching, virtualization, containerized services, and wireless infrastructure.

Rather than treating the ISP transition as a simple connectivity change, I approached it as a controlled infrastructure maintenance operation.

My priorities were to:

- Preserve known-good configurations before the change.
- Verify that backup artifacts were complete and accessible.
- Establish pre-change operational baselines.
- Limit the maintenance window to the ISP transition.
- Validate connectivity systematically across network layers.
- Confirm that dependent infrastructure services remained operational.
- Monitor stability after the initial cutover.

**Result:** The fiber migration was completed successfully. Network connectivity, DNS resolution, routing, monitoring, and internal infrastructure services were verified following the transition. No rollback was required.

---

## 2. Technical Environment

The environment consisted of interconnected infrastructure supporting network operations, virtualization, monitoring, and reliability-sensitive communications.

| Component | Technology | Operational responsibility |
|---|---|---|
| Firewall and routing | OPNsense | WAN connectivity, routing, firewall policies |
| Managed switching | Aruba | Layer 2 connectivity, VLAN distribution |
| Wireless infrastructure | UniFi | Wireless access and network segmentation |
| Virtualization | Proxmox VE | Virtual machine hosting and infrastructure services |
| Container platform | Docker | Containerized management and monitoring applications |
| Monitoring | Grafana, Zabbix, and associated telemetry services | Infrastructure health, connectivity, and operational verification |

The environment included production-use services alongside dedicated technical training resources.

**Security note:** Internal addressing, management endpoints, credentials, device identifiers, and detailed security policies have been omitted from this public case study.

---

## 3. Change Objective and Risk Assessment

### Objective

Transition the WAN connection to the new fiber ISP without introducing unrelated infrastructure changes or compromising the existing network configuration.

### Primary risks

| Risk | Potential impact | Mitigation |
|---|---|---|
| WAN connectivity failure | Loss of Internet access | Pre-change baseline and layered connectivity testing |
| Firewall configuration issue | Routing or service interruption | Verified configuration export |
| Switching or VLAN disruption | Internal network connectivity failure | Configuration capture and VLAN baseline |
| Management service disruption | Reduced ability to administer or monitor infrastructure | Platform backups and post-change service verification |
| Multiple simultaneous changes | Difficult fault isolation | Strict change-scope control |
| Undetected post-cutover instability | Intermittent service failures | Continued monitoring and follow-up observation |

A central decision was to **avoid combining the ISP migration with unrelated network redesign work**.

Additional routing, segmentation, VPN, NAT, and remote-management changes were deferred.

This reduced the number of variables involved in diagnosing any post-cutover failure.

---

## 4. Phase One — Pre-Cutover Protection

Before the transition, I created and verified configuration backups across the major infrastructure platforms.

The backup workflow followed a consistent sequence:

1. Export or capture the configuration.
2. Confirm that the artifact was created successfully.
3. Inspect the artifact for expected contents.
4. Transfer a copy to the administrative workstation.
5. Verify the transferred artifact where applicable.
6. Organize the files by platform and maintenance event.

A backup was not considered complete merely because a file existed.

### Backup and baseline coverage

| Infrastructure | Protection performed | Verification |
|---|---|---|
| Firewall | Native configuration export and interface/gateway reference captures | Export verified |
| Virtualization platform | Configuration archive and separate network configuration capture | Archive contents inspected; SHA-256 hashes generated |
| Docker platform | Container, volume, network, image, and deployment information | Archive contents verified |
| Wireless controller | Native controller configuration backup | Backup creation verified |
| Managed switch | Running configuration, software version, interface state, and VLAN baseline | Configuration and operational state captured |

These artifacts provided reference material for troubleshooting and recovery planning.

**Verification boundary:** Backup files and archive contents were checked. A complete end-to-end restoration from every backup was not performed as part of this workflow.

---

## 5. Phase Two — Controlled ISP Cutover

The physical transition to the new fiber service was completed.

During the cutover, the existing internal network architecture was preserved.

The change was deliberately limited to the ISP transition rather than expanding into additional configuration work.

The operational sequence was:

```text
Pre-Change Backup
        |
        v
Configuration and Health Baselines
        |
        v
Fiber ISP Transition
        |
        v
Local Network Verification
        |
        v
WAN and Gateway Verification
        |
        v
Internet and DNS Verification
        |
        v
Application and Service Verification
        |
        v
Monitoring and Stability Observation
```

This approach established a logical progression for fault isolation if any validation stage failed.

---

## 6. Phase Three — Post-Cutover Verification

After the ISP transition, connectivity was tested progressively rather than relying on a single successful Internet connection.

### A. External IP connectivity

A public IP endpoint was tested independently of DNS.

**Recorded result:**

```text
Packets transmitted: 4
Packets received:    4
Packet loss:         0%
Average round-trip:  ~10 ms
```

This confirmed successful external IP reachability through the new ISP connection at the time of testing.

### B. DNS and application connectivity

DNS-dependent access and HTTPS connectivity were tested separately.

Both were successful.

This distinction matters because successful IP connectivity does not independently establish that DNS resolution or application-layer communication is functioning.

### C. Gateway and routing validation

Post-cutover checks included:

- Default gateway availability
- WAN routing
- Upstream path behavior
- Latency
- Internet connectivity stability

No persistent routing problem was identified.

### D. Local network verification

Local network behavior was checked for indications of connectivity problems introduced during the migration.

The documented observations included:

- Stable neighbor reachability
- No persistent ARP resolution failures
- No observed migration-associated MAC flapping

### E. Infrastructure services

Dependent infrastructure services were checked following the transition.

The validation included firewall operations, virtualization infrastructure, container services, wireless management, and monitoring availability.

The broader environment remained operational following the ISP change.

---

## 7. Monitoring and Operational Observation

The existing monitoring environment provided an additional verification method beyond manual connectivity tests.

Monitoring covered the availability and performance of multiple internal and external targets.

This allowed the post-cutover review to distinguish among several possible fault domains:

```text
Local Network Failure
         |
         |-- Switching / VLAN connectivity
         |-- Firewall / routing
         |-- Internal service availability
         |
         v
ISP or Upstream Failure
         |
         |-- ISP gateway
         |-- External IP reachability
         |-- WAN latency / packet loss
         |
         v
External Service Failure
         |
         |-- DNS-dependent services
         |-- Application endpoints
```

Following the migration, monitoring indicated stable upstream connectivity without sustained packet loss.

Approximately one day after the initial cutover, the environment was reassessed and continued to demonstrate normal operation.

**Important distinction:** Monitoring provided evidence of observed stability during the validation period; it does not establish that future connectivity failures are impossible.

---

## 8. Results

| Validation area | Recorded outcome |
|---|---|
| Pre-cutover backup preparation | Pass |
| Backup artifact verification | Pass |
| Firewall configuration preservation | Pass |
| Virtualization configuration preservation | Pass |
| Container configuration preservation | Pass |
| Wireless controller backup | Pass |
| Managed switch baseline | Pass |
| Fiber ISP transition | Pass |
| WAN connectivity | Pass |
| Gateway operation | Pass |
| External IP reachability | Pass |
| DNS resolution | Pass |
| HTTPS/application connectivity | Pass |
| Routing and path validation | Pass |
| Monitoring availability | Pass |
| Internal infrastructure operation | Pass |
| Follow-up stability observation | Pass |

**Final outcome:** The migration was completed successfully, with no migration-induced infrastructure failure identified during the documented validation period.

**Rollback required:** No.

---

## 9. Operational Lessons

### Backup verification is a separate task from backup creation

The existence of a configuration file does not establish that it contains the expected information or has been transferred successfully.

Inspecting archives, checking contents, and generating integrity hashes increased confidence in the recovery artifacts.

### Change scope affects troubleshooting complexity

Deferring unrelated modifications made the migration easier to evaluate.

If a problem had occurred, the number of potential causes would have been substantially smaller.

### Layered testing improves fault isolation

Testing external IP connectivity, DNS resolution, routing, and applications separately helps identify where a failure occurs.

A successful ping alone does not validate an entire production environment.

### Monitoring complements manual testing

Manual tests provide immediate observations.

Monitoring provides historical and ongoing evidence that can reveal instability beyond the initial maintenance window.

### Recovery planning must precede the change

Preparing configuration backups, capturing operational baselines, and identifying potential recovery paths before the cutover reduced unnecessary operational risk.

---

## 10. Professional Skills Demonstrated

This project demonstrates practical experience in:

- Infrastructure change planning
- Configuration backup and verification
- Operational baseline collection
- Network connectivity testing
- Structured fault isolation
- DNS and routing verification
- Infrastructure monitoring
- Service validation
- Risk reduction through change-scope control
- Technical documentation
- Post-maintenance observation

The project was completed in an independently operated environment, not as a commercial enterprise deployment or a medical-device service engagement.

Nevertheless, the underlying practices—preparation, controlled execution, verification, documentation, and recovery readiness—are directly transferable to technical support and field-service work.

---

## 11. Closing Assessment

The most important outcome was not simply that the new fiber connection worked.

The project demonstrated a repeatable approach to infrastructure change:

**Protect the existing state, control the scope of work, verify each dependent system, and continue observing the environment after the change.**

That approach remains central to how I perform technical maintenance and troubleshoot operational systems.

---

**Documentation:** Adapted from an original operational record dated July 16, 2026, maintained in a private infrastructure repository. Sensitive environment details have been excluded from this public version.
