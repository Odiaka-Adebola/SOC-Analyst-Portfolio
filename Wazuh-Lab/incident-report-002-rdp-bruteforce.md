# 🔐 Incident Report — RDP Brute Force Attack

**Author:** Adebola Odiaka  
**Date:** March 07, 2024  
**Severity:** 🔴 High  
**Status:** True Positive — Escalated  
**Platform:** LetsDefend SOC Simulator (SOC176)

---

## 📋 Incident Summary

A brute force attack was detected against an internal Windows machine 
via RDP (port 3389). An external IP from China made 30 rapid connection 
attempts within one minute. The firewall allowed the traffic through, 
exposing the machine directly to the internet. Threat intelligence 
confirmed the source IP as malicious with a history of brute force attacks.

---

## 🖥️ Affected Asset

| Field | Value |
|---|---|
| Destination IP | 172.16.17.148 |
| Hostname | Matthew |
| Protocol | RDP |
| Destination Port | 3389 |

---

## 🌍 Attacker Information

| Field | Value |
|---|---|
| Source IP | 218.92.0.56 |
| Country | China 🇨🇳 |
| Firewall Action | Allowed |
| Total Attempts | 30 in 1 minute |

---

## 🕐 Timeline

| Time | Event |
|---|---|
| Mar 07, 2024 @ 11:44 AM | 30 RDP connection attempts detected |
| Mar 07, 2024 @ 11:44 AM | SOC176 alert triggered |
| Mar 07, 2024 @ 11:44 AM | Analyst investigation initiated |

---

## 🔍 Threat Intelligence Findings

| Tool | Finding |
|---|---|
| VirusTotal | Flagged as malicious |
| AbuseIPDB | 8 abuse reports — SSH/RDP Brute Force |
| Country | China 🇨🇳 |

---

## 🚨 Key Findings

| Field | Value |
|---|---|
| Event ID | 234 |
| Rule | SOC176 - RDP Brute Force Detected |
| Source IP | 218.92.0.56 |
| Destination Port | 3389 (RDP) |
| Attempts | 30 rapid fire connections |
| Firewall Action | Allowed ⚠️ |

---

## ⚠️ Security Misconfiguration Identified

RDP (port 3389) was exposed directly to the internet.
This is a critical misconfiguration — RDP should never 
be publicly accessible without VPN or IP whitelisting.

---

## 🎯 MITRE ATT&CK Mapping

| Field | Value |
|---|---|
| Tactic | Credential Access |
| Technique | T1110 — Brute Force |
| Sub-technique | T1110.001 — Password Guessing |

---

## ✅ Classification

**True Positive (TP)** — Confirmed malicious external brute force 
attack against exposed RDP service.

---

## 🛡️ Response Actions

1. **Contain** — Block source IP 218.92.0.56 at firewall
2. **Remediate** — Disable public RDP access immediately
3. **Recommend** — Implement VPN for remote access
4. **Escalate** — Notify Tier 2 analyst for deeper investigation
5. **Document** — Incident report filed

---

## 📸 Screenshots

> See `/screenshots` folder for evidence

---

*Investigation completed on LetsDefend SOC Simulator*  
*Alert ID: SOC176 | Event ID: 234*
