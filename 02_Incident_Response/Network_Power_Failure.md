# Network Infrastructure Power Failure
### Incident Response, Root-Cause Analysis & Cost-Conscious Reliability Engineering

**Project type:** Infrastructure Incident Response  
**Environment:** Independently operated, production-use technology training environment  
**Incident date:** August 19, 2026  
**Severity:** Network infrastructure outage  
**Status:** Service restored; corrective-action planning documented  
**Service restoration:** Less than 10 minutes after active troubleshooting began  
**Role:** Infrastructure administrator — diagnosis, recovery, verification, and incident documentation

---

## 1. Executive Summary

On August 19, 2026, an unexpected network outage affected Internet connectivity and internal network services in an independently managed infrastructure environment.

The initial symptom appeared to be an endpoint connectivity failure. Independent testing established that the problem extended beyond the original device. By inspecting host IP configuration, routing, local gateway reachability, and external connectivity, I narrowed the investigation from application-level symptoms to the underlying infrastructure.

Physical inspection identified a tripped GFCI-protected electrical supply feeding the core network's uninterruptible power supply (UPS). The UPS had maintained operations temporarily, but loss of utility power was not remotely detected. After battery runtime was exhausted, the core network equipment shut down.

I restored utility power, allowed the infrastructure to restart, and verified network connectivity.

**Result:** Service was restored in less than 10 minutes after active troubleshooting began, without changing network configurations.

The incident exposed an observability gap: monitoring recorded the resulting outage but did not detect the earlier loss of utility power. The corrective-action approach followed a practical, cost-conscious philosophy: first determine whether existing equipment can close the gap safely and reliably before purchasing new hardware.

---

## 2. Technical Environment

| Component | Function |
|---|---|
| Firewall/router | Routing, firewall enforcement, gateway services |
| Managed switch | Core connectivity and VLAN distribution |
| Wireless infrastructure | Client network access |
| UPS | Temporary backup power for core networking equipment |
| Grafana | Historical connectivity and performance monitoring |
| LibreNMS | Device monitoring and uptime information |
| Administrative workstation | Independent network troubleshooting |

The environment supported technical training and reliability-sensitive production-use communications.

Internal addresses, management interfaces, and detailed security configurations have been omitted.

---

## 3. Initial Symptoms and Detection

A network-connected endpoint unexpectedly lost Internet connectivity. An attempt to reconnect the original device was unsuccessful.

Potential causes included endpoint malfunction, wireless failure, DHCP/addressing failure, gateway or routing failure, ISP outage, or core infrastructure failure.

Rather than assuming the endpoint was responsible, I tested connectivity independently.

---

## 4. Troubleshooting Sequence

### Step 1 — Independent gateway testing

A separate device tested reachability to the local default gateway.

**Result:** Gateway unreachable. The failure was not isolated to the original endpoint.

### Step 2 — Workstation IP configuration

```bash
ip a
```

**Observation:** The workstation did not have its expected IP address.

### Step 3 — Routing table inspection

```bash
ip route
```

**Observation:** The expected default route was absent.

### Step 4 — Local gateway connectivity

```bash
ping <gateway>
```

**Result:** Failed.

### Step 5 — External IP connectivity

```bash
ping <public-test-IP>
```

**Result:** Failed.

### Step 6 — DNS-dependent connectivity

```bash
ping <public-hostname>
```

**Result:** Failed.

### Diagnostic interpretation

| Observation | Diagnostic significance |
|---|---|
| Multiple endpoints affected | Reduced likelihood of a single-device fault |
| Expected IP address absent | Suggested local connectivity or address-assignment failure |
| Default route absent | Confirmed incomplete host network configuration |
| Local gateway unreachable | Directed attention toward local infrastructure |
| External IP test failed | Confirmed loss of external reachability |
| DNS-dependent test failed | Consistent with the broader connectivity outage |

**Decision:** Investigate physical infrastructure before modifying firewall, DHCP, routing, or DNS configurations.

---

## 5. Physical Infrastructure Investigation and Root Cause

ISP equipment showed power, but the core managed switch and firewall/router were powered off. Both depended on a common UPS-backed electrical supply. Inspection identified a tripped GFCI-protected outlet supplying AC power to the UPS.

The UPS had apparently maintained the network temporarily before its battery runtime was exhausted.

```text
GFCI-Protected Supply Trips
             |
             v
Utility Power to UPS Lost
             |
             v
UPS Supplies Battery Power
             |
             v
Network Continues Operating
             |
             v
No Remote Power-State Alert
             |
             v
UPS Battery Runtime Exhausted
             |
             v
Core Switch and Firewall Power Off
             |
             v
Network Connectivity Fails
```

**Root cause:** Loss of AC power to the core UPS following a GFCI trip, followed by exhaustion of UPS runtime. The investigation did not establish why the GFCI initially tripped.

---

## 6. Service Restoration

Recovery did not require network configuration changes.

1. Identified the interrupted electrical supply.
2. Reset the tripped GFCI.
3. Restored utility power to the UPS and core network equipment.
4. Allowed infrastructure to restart.
5. Verified normal network operation.

| Service or function | Result |
|---|---|
| Core infrastructure power | Restored |
| Local network connectivity | Pass |
| Expected host IP configuration | Pass |
| Default gateway availability | Pass |
| Routing | Pass |
| External Internet connectivity | Pass |
| DNS-dependent connectivity | Pass |
| Endpoint operation | Pass |

**Service restoration:** Less than 10 minutes after active troubleshooting began.  
**Configuration changes:** None.  
**Rollback:** Not required.

The total duration of the underlying power interruption was not established by the troubleshooting time.

---

## 7. Monitoring Evidence and Correlation

### Grafana

Historical monitoring showed simultaneous loss of responses from multiple independent upstream and external connectivity probes. Their correlated failure was consistent with a shared local infrastructure problem rather than an isolated external destination failure.

### LibreNMS

Device uptime information showed a firewall/router restart consistent with the incident window, supporting the physical observation that core equipment had lost power.

```text
Endpoint Evidence
- Expected IP address absent
- Default route absent
- Gateway unreachable
         |
         v
Monitoring Evidence
- Multiple probes failed together
- Firewall uptime consistent with restart
         |
         v
Physical Evidence
- Core equipment powered off
- GFCI-protected supply tripped
         |
         v
Root-Cause Determination
- Utility power loss
- UPS runtime exhaustion
```

**Evidence limitation:** Monitoring corroborated the outage and recovery; it did not directly record the UPS transition to battery power.

---

## 8. What Worked

**Structured fault isolation:** Independent device testing and host/network inspection directed troubleshooting toward the correct fault domain.

**Change discipline:** No speculative firewall, VLAN, DHCP, or routing changes were made during the outage.

**UPS resilience:** The UPS temporarily maintained service after utility power was lost; its fundamental backup function operated as intended.

**Historical monitoring:** Grafana and LibreNMS helped reconstruct the event after service restoration.

**Recovery verification:** Connectivity and dependent network functions were checked rather than assuming that powered-on equipment meant service was restored.

---

## 9. Observability Gap: Resilience Is Not Visibility

The UPS protected the network against an immediate power interruption, but the existing monitoring configuration did not detect that it had switched to battery operation.

```text
Utility Power Lost
        |
        v
UPS Maintains Network
        |
        v
No Remote Power Notification
        |
        v
UPS Runtime Exhausted
        |
        v
Network Offline
        |
        v
Monitoring Records Outage
```

The monitoring system recorded the **consequence**, not the **initiating condition**.

This led to a corrective-action objective: detect loss of utility power while the UPS is still keeping the network operational.

---

## 10. Engineering Philosophy — “Pretty, Not Practical”

My operating principle is **“Pretty, Not Practical”**: avoid purchasing attractive, prestigious, or unnecessarily sophisticated equipment when a simpler existing solution can satisfy the operational requirement.

For this incident, the objective was not to replace a functioning UPS or build a new monitoring stack merely because those solutions were available. The objective was to add **reliable, actionable power-state visibility** with the least justified cost and operational complexity.

The evaluation criteria were:

- **Reuse before purchase:** Check whether already-owned, locally monitorable equipment can serve as a utility-power sentinel.
- **No new single point of failure:** The sentinel must not carry production network traffic or be required for the network to operate.
- **Security first:** Avoid unnecessary cloud dependencies, inbound exposure, privileged access, or additional attack surface.
- **Low maintenance burden:** Favor a straightforward design that can be documented, tested, and supported.
- **Evidence-based acceptance:** A lower-cost solution is acceptable only if testing shows that it detects the relevant failure and delivers actionable alerts.
- **Spend when justified:** If existing hardware cannot meet reliability or security requirements, purpose-built UPS telemetry or other equipment may be warranted.

**The aim was not to avoid spending money at any cost. It was to avoid spending money without a measurable operational benefit.**

This principle shaped the proposed corrective action: investigate whether an existing locally observable device could indicate the loss of AC power upstream of the UPS while the protected network remained online.

---

## 11. Corrective-Action Design

### A. Evaluate an existing device as a utility-power sentinel

A locally monitored device powered from the utility side of the UPS could provide an early indication of electrical power loss.

```text
Utility / GFCI Supply
       |
       +---- UPS ---- Core Network
       |
       +---- Existing Power Sentinel (candidate)
```

A possible alert condition:

```text
Utility-Power Sentinel: Unreachable
Core Network:           Reachable
Monitoring Platform:    Reachable

Interpretation:
Possible utility-power loss while
core infrastructure remains on UPS.
```

Because loss of network reachability can also result from wireless or device failure, a sentinel-based approach requires persistence thresholds, corroboration where available, and controlled testing before being treated as reliable utility-power detection.

### B. Evaluate UPS telemetry where appropriate

Potential telemetry includes utility-power state, on-battery status, charge, runtime, load, and low-battery conditions.

The original incident did not justify replacing a UPS that had performed its battery-backup function correctly.

### C. Verify before accepting the solution

A proposed monitoring solution should be tested to confirm:

1. It identifies the intended failure condition.
2. The network remains operational while the alert is generated.
3. The alert reaches the administrator.
4. Transient device/network interruptions do not produce unacceptable false alarms.
5. The solution introduces no unnecessary access paths or dependencies.

**Implementation boundary:** The original incident record documents this design and evaluation plan, not a completed and independently validated power-monitoring deployment. Any later implementation should be documented as a separate follow-up.

---

## 12. Lessons Learned

1. **Diagnose from evidence rather than assumptions.** An endpoint symptom may originate in shared infrastructure.
2. **Local gateway testing helps isolate the fault domain.** A failed gateway test redirects attention away from remote services.
3. **Avoid speculative configuration changes.** Unnecessary changes increase recovery complexity.
4. **Power protection and power monitoring are different capabilities.** A UPS can work correctly while leaving an observability gap.
5. **Monitor dependencies, not only devices.** Electrical power is a shared dependency across the network.
6. **Correlated telemetry supports incident reconstruction.** Historical observations can independently corroborate physical findings.
7. **Recovery requires verification.** Equipment returning to service must be followed by connectivity and function checks.
8. **Practical engineering includes cost and security discipline.** Reusing existing equipment is preferable when it can satisfy a verified requirement without adding avoidable complexity or exposure.

---

## 13. Professional Skills Demonstrated

- Incident response and structured troubleshooting
- Linux networking diagnostics
- Gateway and routing verification
- Physical infrastructure fault isolation
- Root-cause analysis and service restoration
- Monitoring-data interpretation
- Post-recovery verification
- Corrective-action planning
- Asset reuse and cost-conscious engineering
- Security-aware design tradeoffs
- Technical incident documentation

This incident occurred in an independently operated environment, not a commercial enterprise or medical-device service engagement. The methods demonstrated are transferable to technical support, infrastructure operations, and field-service work.

---

## 14. Closing Assessment

The immediate problem was resolved by restoring power. The broader lesson was that a resilient system still needs visibility into the dependencies that sustain it.

The incident reinforced a repeatable operational approach:

**Identify the fault, restore service without unnecessary changes, verify the result, and improve reliability using the simplest secure and effective solution.**

---

**Documentation:** Adapted from an August 19, 2026 incident report maintained in a private infrastructure repository. Sensitive infrastructure details and household information have been excluded from this public version.
