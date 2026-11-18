\# 🔍 SOC Incident Investigation – Windows Security Event Analysis



\## Project Overview



This project demonstrates a practical Security Operations Centre (SOC) investigation involving suspicious authentication activity, potential account compromise, privilege assignment, account manipulation and possible lateral movement.



The investigation uses simulated Windows security events and endpoint alerts to demonstrate how a SOC analyst identifies suspicious behaviour, correlates evidence, assesses risk and recommends incident response actions.



\*\*Project type:\*\* Practical SOC investigation  

\*\*Environment:\*\* Simulated Windows enterprise environment  

\*\*Primary host:\*\* FINANCE-PC-04  

\*\*Potentially affected host:\*\* HR-PC-02  

\*\*Scenario date:\*\* 16 November 2026  

\*\*Incident severity:\*\* High (provisional)



> All evidence in this project is simulated for educational and portfolio purposes. No real organisation or production systems were investigated.



\## Investigation Objectives



\- Identify suspicious authentication patterns.

\- Investigate failed and successful Windows logons.

\- Correlate Windows Security Event IDs.

\- Examine account creation and local administrator-group changes.

\- Interpret privileged logon events.

\- Identify possible account discovery and lateral movement.

\- Map observed and suspected behaviours to MITRE ATT\&CK.

\- Apply evidence preservation and SHA-256 integrity verification.

\- Produce an incident investigation report with response recommendations.



\## Evidence and Investigation



Two simulated CSV datasets were examined:



| Evidence file | Investigation focus |

|---|---|

| `soc\_project4\_simulated\_evidence.csv` | Administrator authentication, account creation, group membership changes and endpoint alert |

| `supportsvc\_authentication\_events.csv` | SupportSvc authentication failures, successful logon and special privileges |



\### Key Findings



1\. Three failed Administrator network logons were followed by a successful logon from the same source address.

2\. Four failed SupportSvc network logons were followed by a successful logon within approximately one minute.

3\. Event ID 4672 indicated that special privileges were assigned to the successful SupportSvc logon session.

4\. Event IDs 4720 and 4732 recorded the creation of SupportSvc and its addition to the local Administrators group.

5\. Additional illustrative scenario activity raised concerns about local account discovery and possible lateral movement to HR-PC-02.



These findings warranted further investigation. They do not independently prove malicious activity, privilege abuse or data exfiltration.



\## Windows Event IDs Examined



| Event ID | Description |

|---|---|

| 4624 | Successful logon |

| 4625 | Failed logon |

| 4672 | Special privileges assigned to a new logon |

| 4798 | User's local group membership enumerated |

| 4720 | User account created |

| 4732 | Member added to a security-enabled local group |

| 4688 | Process creation |



\## MITRE ATT\&CK Mapping



The investigation considered the following techniques:



| Technique ID | Technique |

|---|---|

| T1110 | Brute Force |

| T1078 | Valid Accounts |

| T1087.001 | Account Discovery: Local Account |

| T1069.001 | Permission Groups Discovery: Local Groups |

| T1136.001 | Create Account: Local Account |

| T1098.007 | Account Manipulation: Additional Local or Domain Groups |

| T1021.002 | Remote Services: SMB/Windows Admin Shares |



Mappings are provisional and conditional. A technique is not confirmed merely because an event resembles its behaviour.



\## Evidence Integrity and Preservation



Evidence preservation was included as part of the practical investigation.



Activities performed:



\- Calculated SHA-256 hashes for both original evidence files.

\- Created an Evidence Integrity Register.

\- Documented evidence handling procedures.

\- Created separate working copies for analysis.

\- Recalculated and compared original and working-copy hashes.

\- Confirmed that both pairs matched at the time of verification.



The original evidence files were retained separately from the working copies.



\## Incident Response Recommendations



\- Investigate and validate the legitimacy of the SupportSvc account.

\- Preserve Windows Security and endpoint telemetry.

\- Isolate affected systems where justified by the threat assessment.

\- Review privileged account membership.

\- Examine process execution and command-line activity.

\- Investigate possible access to HR-PC-02.

\- Review persistence mechanisms and other potentially affected systems.

\- Reset credentials where compromise is established or reasonably suspected.

\- Monitor affected systems after recovery.



\## Repository Structure



```text

SOC-Incident-Investigation/

│

├── evidence/

│   ├── windows-events/

│   ├── network-traffic/

│   ├── endpoint-alerts/

│   ├── SOC\_Incident\_Investigation\_Evidence\_Integrity\_Register.md

│   └── evidence-handling-log.md

│

├── investigation/

│   └── working-copies/

│       └── windows-events/

│

├── screenshots/

│

├── reports/

│   └── SOC\_Incident\_Investigation\_Report\_Updated.md

│

└── README.md

```



\## Skills Demonstrated



\- SOC alert triage and incident investigation

\- Windows Event Log analysis

\- Authentication event correlation

\- Account and privilege investigation

\- MITRE ATT\&CK mapping

\- Digital evidence preservation

\- SHA-256 integrity verification

\- Incident severity assessment

\- Incident response planning

\- Technical documentation



\## Project Limitations



This is a simulated training investigation. Some activities discussed during the exercise, including command execution and the possible HR-PC-02 authentication, are illustrative and require original supporting telemetry before they can be treated as verified findings.



The source IP `198.51.100.24` belongs to a reserved documentation range and is not a real attacker indicator.



\## Author



Cybersecurity learner building practical skills in SOC operations, digital forensics, networking and ethical hacking.

