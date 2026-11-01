# Network Observability — LibreNMS, InfluxDB and Grafana

**Independent technical project | 2026 | Public portfolio case study**

## Objective
Implement a validated, maintainable telemetry path for network and host health in a self-managed training environment.

## Validated pipeline
Monitored Linux workstation and network infrastructure → LibreNMS → InfluxDB 2.x → Grafana.

Zabbix provides selected service and host checks; Telegraf is used for appropriate host metrics. Collection is deliberately proportional to operational needs.

## Verified work
- Confirmed an existing InfluxDB deployment was healthy before enabling metric export.
- Configured LibreNMS metric export into a dedicated InfluxDB bucket with defined retention.
- Separated write-only collector access from read-only visualization access.
- Ran a device poll and verified successful metric writes.
- Inspected measurements in InfluxDB before validating Grafana dashboards.
- Aligned dashboard time resolution with the underlying polling cadence.
- Collected supported availability, resource, interface and hardware measurements.

## Troubleshooting example
A missing disk-metric panel prompted source-to-dashboard investigation. The missing collector disk input was identified; the record does not claim a fix was completed without validation.

## Engineering approach
Verify dependencies, confirm each telemetry stage, use least-privilege access, avoid unnecessary duplicate collection, and distinguish completed work from plans.

## Transferable skills
Monitoring and observability, Linux and network diagnostics, time-series data, root-cause investigation, service validation and reproducible documentation.

## Scope and provenance
Independent technical training and operational work, **not** hospital IT or medical-device servicing. Adapted from internal May 25 and August 21, 2026 records. No internal addressing, private topology, credentials, device identities or live endpoints.
