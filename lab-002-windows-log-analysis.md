# LAB REPORT — Windows Log Analysis

Analyst:     Odiaka Adebola Favour

Date:        2026-05-20

Environment: Windows 11 — VirtualBox

## Objective
Configure Windows audit logging and generate
real security events to understand how attacks
appear in raw Windows Security logs.

## Environment Setup
- Enabled 4 audit policies via secpol.msc
- Verified logging was active via eventvwr.msc
- Policies enabled: Account Logon Events,
  Logon Events, Account Management,
  Process Tracking — all set to
  Success and Failure

### Policy Configuration Proof:
![Enabling Audit Account Management](windows%20audit%20succcess%20and%20failiur.png)

![Local Security Policy Audit Settings](success%20and%20failiure.png)

---

## Scenarios Executed

### Scenario 1 — Brute Force Simulation
**Event IDs captured:** 4625, 4624

**What I did:**
Locked the Windows screen and deliberately
entered the wrong password 10 times, then
logged in successfully on the 11th attempt.

**What I observed in logs:**
Found 10 x Event ID 4625 (failed login)
followed by 1 x Event ID 4624 (successful
login) under account name Debola.
Source address was 127.0.0.1 confirming
local interactive login. Logon Type 2.

![Failed Logon Event Log](event%20veiw.jpg)

**Analyst note:**
Same event sequence as a real brute force
attack. The difference is the source IP —
127.0.0.1 (local) vs an external attacker
IP. Context determines verdict.

---

### Scenario 2 — Backdoor Account Creation
**Event IDs captured:** 4720, 4732, 4726

**What I did:**
Created a new user account called hackersim
using net user command, added it to the
Administrators group, then deleted it.

**What I observed in logs:**
- Event ID 4720: hackersim account created
  by Debola. Password not required. Never
  expires. Immediate red flag.

![User Account Created Log](acccount%20created.jpg)

- Event ID 4732: hackersim added to Users
  group then Administrators group.
  Full privilege escalation confirmed.

- Event ID 4726: hackersim account deleted.
  Attacker covering tracks — but Windows
  logged the deletion anyway.

![User Account Deleted Log](accout%20deleted.jpg)

**Analyst note:**
This mirrors real attacker persistence
technique MITRE ATT&CK T1136.001 —
Create Local Account. Deletion of account
does not erase the audit trail.

---

### Scenario 3 — Off Hours Access
**Event IDs captured:** 4624

**What I did:**
Changed system clock to 3:00 AM, locked
the screen, and logged back in successfully.

**What I observed in logs:**
Event ID 4624 showing successful login by
Debola at 3AM. Logon Type 2. This mirrors
the suspicious off-hours login pattern from
the jsmith brute force alert.

**Analyst note:**
Time alone does not confirm malicious
activity. Combined with external source IP
and Tor exit node — off hours login becomes
a critical red flag.

---

## Key Takeaways
1. Windows does not log security events by
   default. Audit policies must be manually
   enabled via secpol.msc — without this,
   attacks are completely invisible to SOC.

2. Event IDs tell a story when read in
   sequence. A single 4625 means nothing.
   47 x 4625 followed by 4624 from an
   external IP means active breach.

3. Attackers delete accounts to cover tracks
   but Windows logs the deletion as Event ID
   4726. You cannot hide from enabled logging.

---

## Analyst Conclusion
All three scenarios were successfully
generated and captured in Windows Security
logs. This lab confirms that proper audit
policy configuration is the foundation of
any effective SOC detection capability.

### Network Traffic Telemetry (Bonus Verification)
![Inbound RDP Network Connection Log](windows%205156.png)
