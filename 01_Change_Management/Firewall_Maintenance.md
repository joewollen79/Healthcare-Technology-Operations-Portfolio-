# Scheduled Firewall Maintenance and Verification
### Change Control, Security Updates & Infrastructure Validation

**Project type:** Preventive Infrastructure Maintenance  
**Environment:** Independently operated, production-use technology training environment  
**Date:** June 1, 2026  
**Maintenance window:** 20:01–20:26 (25 minutes)  
**Platform:** OPNsense firewall  
**Outcome:** Successful upgrade and post-maintenance validation  
**Role:** Infrastructure administrator — preparation, execution, monitoring, and verification

---

## 1. Executive Summary

Performed scheduled maintenance on the primary firewall platform supporting a segmented network environment with virtualization, containerized applications, wireless infrastructure, and monitoring services.

The maintenance objective was to apply security and software updates while preserving network functionality and validating recovery following the required reboot.

Rather than considering the update complete when the firewall restarted, I used a structured verification process to confirm that the network and its dependent services had returned to their expected operational state.

The procedure included:

- Reviewing relevant release notes.
- Confirming pre-maintenance configuration backups.
- Verifying infrastructure health before the change.
- Executing the firewall software upgrade.
- Observing the interruption and recovery through Grafana and Zabbix.
- Validating routing, WAN connectivity, DNS, and network segmentation.
- Confirming management access and dependent infrastructure services.
- Establishing a post-maintenance observation plan.

**Result:** The maintenance was completed successfully within a 25-minute session. All documented post-upgrade checks passed, and no rollback was required.

---

## 2. Technical Environment

The firewall provided network connectivity and security services for an independently managed infrastructure environment.

| Component | Technology | Function |
|---|---|---|
| Firewall | OPNsense | Routing, firewall enforcement, WAN connectivity |
| Managed switching | Aruba | Internal network connectivity and VLAN distribution |
| Wireless infrastructure | UniFi | Wireless access and network segmentation |
| Virtualization | Proxmox VE | Virtual infrastructure hosting |
| Container platform | Docker | Infrastructure and application services |
| Monitoring | Grafana | Telemetry visualization and historical event analysis |
| Monitoring | Zabbix | Infrastructure monitoring and event detection |

The environment included multiple segmented networks with different operational and security requirements.

**Security note:** Internal addresses, management endpoints, detailed firewall policies, and other sensitive configuration details are intentionally excluded.

---

## 3. Maintenance Objectives

The primary objective was to update the firewall platform without introducing persistent service disruption.

Specific goals included:

1. Apply the available firewall software updates.
2. Preserve the existing configuration.
3. Verify successful reboot and platform recovery.
4. Confirm WAN connectivity and routing.
5. Confirm DNS functionality.
6. Verify expected network interfaces and VLANs.
7. Validate management access.
8. Confirm that dependent infrastructure remained operational.
9. Verify that monitoring platforms detected the maintenance event.
10. Begin a post-maintenance stability observation period.

A successful package installation alone would not satisfy these objectives.

The operational requirement was to confirm that the **entire dependent environment** remained functional after the change.

---

## 4. Pre-Maintenance Preparation

### Infrastructure baseline

Before beginning the update, I verified that the environment was operating normally.

The pre-change checks included:

| System or service | Pre-maintenance state |
|---|---|
| WAN gateway | Operational |
| Firewall | Operational |
| Container host | Operational |
| Monitoring server | Operational |
| Hypervisor | Operational |
| Wireless controller | Operational |
| Wireless access points | Operational |
| DNS monitoring targets | Operational |
| Segmented network environment | Operational |

No active alerts indicated a preexisting wireless or critical communications problem.

### Configuration protection

Firewall configuration backups had been completed earlier in the day.

I confirmed their completion before beginning the maintenance window.

This provided a recovery reference if the update introduced a configuration or platform problem.

**Verification boundary:** The documented maintenance record confirms backup completion. It does not establish that a full firewall restoration was performed.

### Release-note review

Before initiating the upgrade, I reviewed the target release notes.

Relevant changes included:

- WAN DHCP security fixes
- OpenSSH updates
- DNS-related updates
- VPN updates
- DHCP improvements
- General system and interface fixes

This review helped establish the operational relevance of the update and identify services requiring post-change verification.

---

## 5. Upgrade Execution

The firewall was running OPNsense 26.1.2 before the maintenance.

The upgrade was initiated through the platform's management interface.

### Recorded upgrade details

| Item | Observation |
|---|---|
| Starting version | OPNsense 26.1.2 |
| Packages updated | 111 |
| Download volume | Approximately 352 MiB |
| Reboot required | Yes |
| Upgrade completion | Successful |
| Rollback required | No |

The platform completed the update and rebooted.

The monitoring environment remained available to observe the interruption and recovery.

---

## 6. Monitoring During Maintenance

A key objective was to validate not only the firewall but also the monitoring systems responsible for detecting infrastructure events.

### Grafana observations

During the firewall reboot:

- The WAN gateway transitioned to an unavailable state.
- Firewall availability reflected the reboot.
- The monitoring platform continued operating.

Following recovery:

- WAN gateway availability returned.
- Firewall availability returned.
- Monitored infrastructure returned to its expected operational state.

These observations provided historical evidence of the maintenance interruption and recovery.

### Zabbix observations

Zabbix generated expected events associated with the firewall maintenance, including:

- Firewall restart
- Host information change

Monitoring and event processing remained operational throughout the maintenance session.

### Operational significance

The maintenance event provided a practical validation of the monitoring architecture.

```text
Planned Infrastructure Change
             |
             v
Monitoring Detects Interruption
             |
             v
Event Recorded
             |
             v
Infrastructure Recovers
             |
             v
Monitoring Confirms Recovery
             |
             v
Manual Service Validation
             |
             v
Maintenance Closed
```

The monitoring tools supplied supporting evidence, but they did not replace manual verification of the dependent services.

---

## 7. Post-Upgrade Verification

After the firewall restarted, I performed a structured set of checks.

### A. Platform and package state

The installed firewall version was verified through the management interface and console.

A subsequent update check reported that the installed packages were current.

No pending package updates or conflicts were identified.

**Result: PASS**

### B. WAN and gateway connectivity

The WAN gateway and external connectivity were tested.

Recorded results included:

- Successful gateway reachability
- Successful WAN connectivity
- 0% packet loss during the documented checks
- Stable observed latency

**Result: PASS**

### C. DNS functionality

DNS-dependent connectivity was tested.

Name resolution completed successfully.

**Result: PASS**

### D. Routing and network segmentation

The expected network interfaces were confirmed present.

The validation included the primary LAN, WAN, and segmented network interfaces.

Routing and VLAN-related checks did not identify a post-upgrade problem.

**Result: PASS**

### E. Administrative access

Secure administrative access to the firewall was tested successfully.

**Result: PASS**

### F. Virtualization and container services

The virtualization host and container infrastructure remained operational.

No abnormal hypervisor resource utilization was observed during the documented verification.

Administrative connectivity to the container host was successful.

**Result: PASS**

### G. Wireless infrastructure

The wireless controller was operational.

Access points were online, clients were connected, and wireless service was functioning.

**Result: PASS**

### H. Monitoring infrastructure

Grafana dashboards and telemetry collection remained operational.

Zabbix monitoring, event generation, and trigger processing were also confirmed.

**Result: PASS**

---

## 8. Validation Summary

| Validation area | Outcome |
|---|---|
| Firewall upgrade | PASS |
| Installed package state | PASS |
| Firewall reboot and recovery | PASS |
| WAN connectivity | PASS |
| Gateway connectivity | PASS |
| DNS resolution | PASS |
| Routing | PASS |
| Expected VLAN interfaces | PASS |
| Administrative access | PASS |
| Container infrastructure | PASS |
| Virtualization infrastructure | PASS |
| Wireless controller | PASS |
| Wireless access points | PASS |
| Grafana telemetry and dashboards | PASS |
| Zabbix monitoring and events | PASS |

**Overall documented maintenance result: PASS**

**Rollback required: No**

No persistent service failures were identified during the recorded post-maintenance verification.

---

## 9. Post-Maintenance Observation

Following the successful update, I established a planned 24–48-hour observation period.

The intended monitoring scope included:

- WAN stability
- Gateway latency
- DNS functionality
- Critical communications connectivity
- Wireless access-point stability
- Wireless controller availability
- Firewall service stability
- Monitoring-platform health

Additional infrastructure changes were deferred during this observation period to reduce diagnostic complexity.

**Documentation boundary:** The source maintenance record establishes that this observation period was planned and initiated. It does not independently establish the final results of the full 24–48-hour period.

---

## 10. Engineering Approach — “Pretty, Not Practical”

This maintenance reflected my broader engineering philosophy: **use existing capabilities effectively before adding unnecessary tools, devices, or complexity**.

The environment already had monitoring platforms capable of recording the firewall's interruption and recovery.

Rather than introducing another monitoring product solely for this maintenance event, I used the existing infrastructure to:

- Establish the pre-change baseline.
- Observe the expected service interruption.
- Record recovery.
- Correlate events with the maintenance timeline.
- Support post-upgrade validation.

This approach reduced unnecessary expenditure and avoided introducing additional software dependencies or management surfaces.

However, operational simplicity was not treated as a substitute for verification.

Existing tools were considered sufficient for this task because they provided useful observations, while manual checks confirmed the functions that monitoring alone could not establish.

**Principle:** Choose the least complex solution that reliably satisfies the operational requirement—not the most elaborate solution available.

---

## 11. Lessons Learned

### A successful update is not the same as a successful maintenance event

The firewall reporting an updated version did not independently prove that routing, DNS, wireless connectivity, and dependent infrastructure were functioning.

Those services required separate verification.

### Baselines make post-change interpretation more reliable

Confirming infrastructure health before the upgrade made it easier to distinguish maintenance-related problems from preexisting conditions.

### Monitoring is most useful when paired with manual testing

Grafana and Zabbix demonstrated that they could record the maintenance interruption and recovery.

Manual validation established that critical network functions were operating as expected.

### Recovery planning should precede execution

Confirming backups and reviewing release notes before the maintenance window reduced avoidable operational risk.

### Scope control reduces uncertainty

Deferring unrelated infrastructure changes preserved a clearer relationship between the upgrade and any subsequent symptoms.

### Operational simplicity has measurable value

Reusing existing monitoring infrastructure avoided unnecessary expenditure, additional dependencies, and new administrative overhead.

---

## 12. Professional Skills Demonstrated

This project demonstrates hands-on experience in:

- Scheduled infrastructure maintenance
- Software and security patch management
- Release-note review
- Configuration backup preparation
- Change-risk assessment
- Maintenance-window execution
- Network connectivity validation
- DNS and routing verification
- Network segmentation verification
- Infrastructure service checks
- Monitoring and event correlation
- Post-maintenance documentation
- Cost-conscious operational engineering

The maintenance was performed in an independently operated environment rather than a commercial enterprise or healthcare facility.

The underlying methods—preparation, controlled execution, service verification, monitoring, and documentation—are directly transferable to technical support and field-service operations.

---

## 13. Closing Assessment

The firewall upgrade was completed successfully within the documented maintenance window.

The most significant outcome was not simply that the operating system and packages were updated.

It was that the change was performed within a controlled process, the monitoring environment captured the interruption and recovery, and dependent infrastructure services were verified afterward.

**A maintenance task is complete only when the affected system and its operational dependencies have been checked—not merely when the update process reports success.**

---

**Documentation:** Adapted from a June 1, 2026 maintenance record maintained in a private infrastructure repository. Sensitive network details have been excluded from this public version.
