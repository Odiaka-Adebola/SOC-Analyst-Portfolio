# LAB REPORT — Network Recon Detection
======================================
Analyst:     Odiaka Adebola Favour
Date:        2026-05-20
Environment: Kali Linux (Attacker VM) 
             Windows 11 (Target VM)
             VirtualBox — Host-Only/Bridged Network

## Objective
Simulate a network reconnaissance scan from an attacker machine (Kali Linux) and identify the specific telemetry artifacts left behind in the Windows Security log.

## Attack Parameters
- **Attacker IP:** 172.20.10.3 (Kali Linux)
- **Target IP:** 172.20.10.4 (Windows 11)

---

## Reconnaissance Activity (Attacker Side)
**Tool Used:** Nmap  
**Command Executed:** `nmap -sV 172.20.10.4`

**Scan Discovery:** The Nmap scan enumerated the target's attack surface, identifying **Port 3389** as open and actively probing it to fingerprint the version information of the underlying Remote Desktop Protocol (RDP) service.

---

## Detection & Telemetry (Defender Side)

Reviewing the Windows Event Viewer under the Security log, the reconnaissance scan triggered active logging of the connection attempts.

### Windows Security Telemetry Artifact:
![Windows Filtering Platform Connection Log](images/windows%205156.png)

### Core Log Attributes:
- **Event ID:** 5156 (Windows Filtering Platform permitted a connection)
- **Source Address:** 172.20.10.3
- **Destination Address:** 172.20.10.4
- **Destination Port:** 3389
- **Service Targeted:** `ms-wbt-server` (Remote Desktop Protocol / RDP)

---

## Incident Triage Analysis

- **WHO:** `172.20.10.3` (Internal network host running automated scanning software).
- **WHAT:** Directed rapid connection probes against destination port `3389`.
- **WHEN:** `5/20/2026 12:44:30 PM` to `5/20/2026 12:45:32 PM UTC`.
- **WHERE:** Windows 11 target workstation (`Windows11` hostname).
- **WHY SUSPICIOUS:** Host `172.20.10.3` executed systematic service enumeration against port 3389 rather than normal interactive user traffic. This indicates active mapping of network resources to find potential entry points.

---

## Key Takeaways
1. **Visibility Requires Configuration:** Windows does not log network connections by default. Audit object access and filtering platform policies must be enabled via `secpol.msc`—without them, reconnaissance is completely invisible to the SOC.
2. **High-Value Targeting:** Nmap scanning reveals active attack vectors. Port 3389 (RDP) is heavily targeted by real-world threat actors for credential stuffing and lateral movement.
3. **Telemetry Filtering:** Event ID 5156 is a high-volume log but serves as a critical detection source when filtered by specific ports (like 3389, 22, 445) or unauthorized source addresses.

---

## Analyst Conclusion & Recommendation
**Incident Classification:** **True Positive (TP)**

The captured logs clearly indicate active host reconnaissance and service version fingerprinting originating from `172.20.10.3`. 

**Recommendation:** Escalate this incident to **Tier 2 Senior Analysts** for deeper investigation. Recommend checking host `172.20.10.3` for potential compromise, reviewing network firewall restrictions on port 3389, and checking log files for any subsequent brute-force or authentication attempts following this scan.
