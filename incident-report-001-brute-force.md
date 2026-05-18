# INCIDENT TRIAGE REPORT
======================
Analyst:       Odiaka Adebola
Date:           2026-05-18
Severity:       HIGH

SUMMARY: A successful bruteforce attack on jsmiths account. it happened on the worskstation 047 and could lead to severe damage.  

**TRIAGE FINDINGS:
WHO:    jsmith
WHAT:   47 failed logins, 1 successfull login 
WHEN:   (2:58)-(3:17) UTC
WHERE:  Netherland, external ip with the Tor anonymization

**WHY SUSPICIOUS:  tor anonymization was used 

CIA IMPACT:
Confidentiality:  the attacker now has access to sensitive information 
Integrity:   they could alter it      
Availability:     and make us lose acces to it

RECOMMENDED ACTION:
Escalate to Tier 2 for full investigation 
Block the IP and disssable the account
