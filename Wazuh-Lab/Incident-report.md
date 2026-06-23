# 🔐 Incident Report — Brute Force Attack on Windows 11

**Author:** Adebola Odiaka  
**Date:** June 20, 2026  
**Severity:** 🔴 High (Rule Level 10)  
**Status:** Resolved  

---

## 📋 Incident Summary

A brute force attack was detected against local account **Debola** on the Windows-11 machine. Wazuh triggered a high severity alert after detecting 3+ failed login attempts within 60 seconds. The repeated attempts resulted in the account being locked out.

---

## 🖥️ Affected Asset

| Field | Value |
|---|---|
| Machine | Windows-11 |
| Agent IP | 172.20.10.4 |
| Agent ID | 001 |
| Targeted Account | Debola |
| Domain | WINDOWS11 |

---

## 🕐 Timeline

| Time | Event |
|---|---|
| Jun 20, 2026 @ 23:10:30 UTC | Wazuh alert fired |
| Jun 20, 2026 @ 23:10:31 UTC | Failed login — bad password (0xC000006D) |
| Jun 20, 2026 @ 23:10:33 UTC | Failed login — account locked out (0xC0000234) |

---

## 🔍 Evidence

| Field | Value |
|---|---|
| Event ID | 4625 |
| Source IP | 127.0.0.1 |
| Target User | Debola |
| Logon Type | 2 (Interactive) |
| Status Code | 0xC0000234 (Account Locked Out) |
| Rule ID | 100002 |
| Rule Level | 10 |
| Process | C:\Windows\System32\svchost.exe |
| Manager | Wazuh-Server |

---

## 🎯 MITRE ATT&CK Mapping

| Field | Value |
|---|---|
| Tactic | Credential Access |
| Technique | T1110 — Brute Force |

---

## 🛡️ Response Actions

1. **Investigate** — Reviewed all Event ID 4625 logs around the timestamp
2. **Triage** — Classified as True Positive (TP)
3. **Contain** — Block source IP 127.0.0.1
4. **Escalate** — Notify Tier 2 analyst
5. **Document** — Incident report filed

---

## 🔧 Detection Method

Detected via **Wazuh SIEM** using custom Rule ID 100002:
- Threshold: 3+ failed logins within 60 seconds
- Mapped to MITRE T1110 automatically
- Agent: Windows-11 (172.20.10.4)

---

## 📸 Screenshots

> See `/screenshots` folder for dashboard evidence

---

*Report generated as part of SOC Analyst Internship Pre-Onboarding Lab*  
*Tool: Wazuh 4.x | Index: wazuh-alerts-4.x-2026.06.20*
