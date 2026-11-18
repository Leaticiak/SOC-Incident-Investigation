# SOC Incident Investigation Report

**Project:** `SOC-Incident-Investigation`  
**Incident:** Suspected compromise and misuse of the `SupportSvc` account  
**Primary host:** `FINANCE-PC-04`  
**Potentially affected host:** `HR-PC-02`  
**Scenario date:** 16 November 2026  
**Assessment:** High (provisional)  
**Evidence type:** Simulated training data

> **Important scope note:** This is a simulated portfolio exercise, not a report of a real-world breach. The report distinguishes records present in the supplied CSV datasets from additional illustrative events discussed during the exercise. Illustrative events must not be represented as collected evidence.

---

## 1. Executive Summary

A review of simulated Windows security and endpoint records identified suspicious authentication activity involving the `SupportSvc` account on `FINANCE-PC-04`.

The supplied authentication dataset records four consecutive failed network logons from `198.51.100.24`, followed by a successful network logon using `SupportSvc`. The successful logon is associated with a subsequent Event ID 4672 record indicating that special privileges were assigned to the session.

The scenario also includes account discovery and local administrator-group discovery commands, followed by a successful network authentication involving `SupportSvc` on `HR-PC-02`. These latter activities were introduced as illustrative investigation evidence and require corresponding source records before they can be treated as independently verified findings.

The combination of repeated authentication failures, a subsequent successful logon, a privileged session, discovery activity and a possible second-host authentication warrants urgent investigation and a **provisional High severity** assessment.

The available evidence does not establish data theft, successful exploitation, or full compromise of `HR-PC-02`.

## 2. Incident Scope

### 2.1 Primary system

**Host:** `FINANCE-PC-04`

This is the primary host under investigation because the supplied simulated records show repeated failed authentication attempts, a subsequent successful `SupportSvc` logon and a special-privileges event.

### 2.2 Potentially affected system

**Host:** `HR-PC-02`

The scenario includes a successful network authentication involving `SupportSvc` from `FINANCE-PC-04`. A successful logon alone does not prove malicious execution, file access or compromise of the destination host. Further host-side investigation is required.

### 2.3 Accounts of interest

- `Administrator`
- `SupportSvc`

### 2.4 Network indicators

- `192.168.1.45` — source address associated with the earlier Administrator authentication sequence in Dataset 1.
- `198.51.100.24` — source address in the simulated `SupportSvc` authentication sequence in Dataset 2.

**Address caveat:** `198.51.100.24` is within the TEST-NET-2 range reserved for documentation and examples. It is not evidence of a real external attacker address.

## 3. Evidence Reviewed

### 3.1 Supplied simulated datasets

| Evidence file | Content | Use in investigation |
|---|---|---|
| `soc_project4_simulated_evidence.csv` | Simulated authentication, account-management and endpoint-alert records | Initial alert triage and account-creation investigation |
| `supportsvc_authentication_events.csv` | Simulated `SupportSvc` logons, failures and special-privileges event | Authentication pattern and session correlation |

### 3.2 Relevant Windows event identifiers

| Event ID | Meaning | Investigative relevance |
|---|---|---|
| 4624 | Successful logon | Establishes that a logon succeeded; review logon type, source and account |
| 4625 | Failed logon | Helps identify repeated or unusual authentication failures |
| 4672 | Special privileges assigned to a new logon | Identifies a session assigned sensitive privileges; does not by itself prove abuse |
| 4798 | User's local group membership was enumerated | May support account or permission-group discovery, but can be normal Windows activity |
| 4720 | User account created | Relevant to account creation and possible persistence |
| 4732 | Member added to a security-enabled local group | Relevant to local group changes and possible privilege manipulation |
| 4688 | Process creation | Can help establish what executable or command was launched, if auditing is enabled |

**Evidence availability:** The two CSV files are the supplied simulated records. Process execution, SMB share access, and `HR-PC-02` host-side events discussed later in the exercise are proposed or illustrative evidence unless separately collected and added to the repository.

### 3.3 Evidence integrity verification

Two original simulated CSV files were hashed using SHA-256 on 18 November 2026. Working copies were then created in `investigation/working-copies/windows-events/` and their hashes were compared with the corresponding original-file hashes.

| Evidence ID | File | Original SHA-256 | Working-copy comparison |
|---|---|---|---|
| E001 | `soc_project4_simulated_evidence.csv` | `ED63E20919B5841CA19C125B0E7F00B2D406F511072690698882F22F1B23C2C0` | Match |
| E002 | `supportsvc_authentication_events.csv` | `DDCBB1AA8A232302022C0BCDCC018AB5964EF98365F59A3EADE682931CA2B979` | Match |

**Result:** Both working-copy hashes matched their corresponding original-file hashes at the time of comparison. This supports the conclusion that the copied file contents matched the originals at that point.

**Limitation:** Hash equality does not establish that the source records were authentic before hashing, that the evidence is complete, or that no relevant records were omitted. The source data in this project is simulated. The hashes are documented as baseline values for future comparison.

## 4. Key Findings

### 4.1 Earlier Administrator authentication sequence

Dataset 1 records three failed network logons for `Administrator` from `192.168.1.45`, followed by a successful network logon from the same source. This pattern warrants review but does not independently establish brute force. An authorised administrator, a misconfigured service or repeated incorrect credentials may produce similar records.

### 4.2 SupportSvc authentication sequence

Dataset 2 records four failed network logons for `SupportSvc` from `198.51.100.24`, followed by a successful network logon from the same source. The failures and success occur within approximately one minute.

This is suspicious and consistent with possible credential guessing or other unauthorised authentication. The exact method is not established by these events alone.

### 4.3 Special privileges

A simulated Event ID 4672 is associated with the same logon session identifier as the successful `SupportSvc` Event ID 4624.

Event 4672 indicates that special privileges were assigned to the logon session. It does not prove that the privileges were abused or that privilege escalation occurred. In a real investigation, the full event XML, privilege list, account group membership, host role and expected administrative activity should be reviewed.

### 4.4 Account creation and local administrator group change

Dataset 1 includes simulated Event IDs 4720 and 4732 involving `SupportSvc`: account creation and addition to the local Administrators group.

These events are important because an unexpected account creation and privilege assignment can support persistence or privilege manipulation. However, they must be checked against approved change records, the initiating account, source host, timestamps and business justification. An event is not malicious solely because it creates an account or changes group membership.

### 4.5 Discovery activity and possible lateral movement

The training scenario also discusses execution of `net user` and `net localgroup administrators`, which would be consistent with local account discovery and local permission-group discovery. The scenario further discusses a successful `SupportSvc` network logon to `HR-PC-02`.

These are illustrative investigation details unless their underlying process and destination-host records are collected. If verified, they would increase concern about reconnaissance and possible lateral movement. A successful network logon to `HR-PC-02` alone would not establish what the account did after authenticating.

## 5. Preliminary Timeline

The timeline below combines exact records in the supplied simulated datasets. Later events discussed in the scenario are marked as illustrative and must not be treated as independently verified evidence.

| Time (16 Nov 2026) | Host | Event | Status |
|---|---|---|---|
| 09:14:02 | `FINANCE-PC-04` | Failed network logon for `Administrator` from `192.168.1.45` | Present in Dataset 1 |
| 09:14:08 | `FINANCE-PC-04` | Failed network logon for `Administrator` from `192.168.1.45` | Present in Dataset 1 |
| 09:14:15 | `FINANCE-PC-04` | Failed network logon for `Administrator` from `192.168.1.45` | Present in Dataset 1 |
| 09:14:35 | `FINANCE-PC-04` | Successful network logon for `Administrator` from `192.168.1.45` | Present in Dataset 1 |
| 09:15:10 | `FINANCE-PC-04` | Event 4798: local group membership enumeration | Present in Dataset 1 |
| 09:16:42 | `FINANCE-PC-04` | Simulated EDR alert for `C:\Users\Public\update.exe` | Present in Dataset 1 |
| 09:17:05 | `FINANCE-PC-04` | Event 4720: `SupportSvc` account created | Present in Dataset 1 |
| 09:17:31 | `FINANCE-PC-04` | Event 4732: `SupportSvc` added to local Administrators group | Present in Dataset 1 |
| 09:25:14 | `FINANCE-PC-04` | Successful `SupportSvc` network logon from `192.168.1.20`, workstation `IT-ADMIN-02` | Present in Dataset 2; approval still requires verification |
| 10:02:11 | `FINANCE-PC-04` | `SupportSvc` failed network logon from `198.51.100.24` | Present in Dataset 2 |
| 10:02:18 | `FINANCE-PC-04` | `SupportSvc` failed network logon from `198.51.100.24` | Present in Dataset 2 |
| 10:02:25 | `FINANCE-PC-04` | `SupportSvc` failed network logon from `198.51.100.24` | Present in Dataset 2 |
| 10:02:32 | `FINANCE-PC-04` | `SupportSvc` failed network logon from `198.51.100.24` | Present in Dataset 2 |
| 10:03:04 | `FINANCE-PC-04` | Successful `SupportSvc` network logon from `198.51.100.24` | Present in Dataset 2 |
| 10:03:05 | `FINANCE-PC-04` | Special privileges assigned to the session | Present in Dataset 2 |
| Later, time not established | `FINANCE-PC-04` | `cmd.exe`, `net user`, and `net localgroup administrators` discussed | Illustrative; obtain process records |
| Later, time not established | `HR-PC-02` | Successful network logon involving `SupportSvc` discussed | Illustrative; obtain destination-host record |

**Timeline limitation:** Do not assign exact timestamps to the illustrative events until the corresponding original event records or telemetry are obtained.

## 6. MITRE ATT&CK Mapping (Provisional)

| Observed or suspected behaviour | Technique | Interpretation |
|---|---|---|
| Repeated failed authentication attempts | T1110 – Brute Force | Possible credential guessing; exact method requires investigation |
| Successful use of an account by an unauthorised party | T1078 – Valid Accounts | Apply only if unauthorised adversary use is established |
| Local account enumeration using `net user` | T1087.001 – Account Discovery: Local Account | Illustrative command evidence in this exercise |
| Local administrator-group enumeration | T1069.001 – Permission Groups Discovery: Local Groups | Illustrative command evidence in this exercise |
| Creation of an account for adversarial persistence | T1136.001 – Create Account: Local Account | Conditional on malicious, unauthorised account creation |
| Addition to a local group for adversarial account manipulation | T1098.007 – Account Manipulation: Additional Local or Domain Groups | Conditional on malicious, unauthorised group modification |
| Use of SMB for movement between hosts | T1021.002 – Remote Services: SMB/Windows Admin Shares | Apply only if adversarial lateral movement using SMB is established |

ATT&CK mappings describe possible adversary behaviour; they do not independently prove that the behaviour was malicious.

## 7. Severity Assessment

**Provisional severity: High**

Rationale:

- Repeated failed logons followed by a successful logon.
- A successful logon associated with a special-privileges event.
- Potentially unauthorised account and local administrator-group changes.
- Discovery activity discussed in the scenario.
- Possible authentication to a second workstation.

The rating should be reassessed as the investigation establishes confidence, scope, business impact, account authorisation and whether any critical assets or sensitive information were affected. Critical severity is not established by the current evidence.

## 8. Recommended Response Actions

### 8.1 Immediate containment

- Coordinate with incident response before disabling or restricting `SupportSvc`, while avoiding delay if active harm is occurring.
- Isolate `FINANCE-PC-04` where justified by the active threat assessment.
- Investigate and, if warranted, isolate `HR-PC-02` separately.
- Restrict suspicious network communications and review use of the account on other systems.

### 8.2 Evidence preservation

- Preserve relevant Windows Security, endpoint detection and network records.
- Collect original event log files and relevant endpoint artefacts.
- Preserve suspicious binaries, including any `update.exe` sample, and calculate SHA-256 hashes.
- Consider volatile-memory acquisition where appropriate and feasible.
- Maintain a chain-of-custody record and work from verified copies.
- Retain the original simulated CSV files unchanged and document subsequent evidence access or transfer.

### 8.3 Investigation and eradication

- Review the full XML for Events 4624, 4625 and 4672, including logon identifiers, source details and privilege list.
- Examine process creation records (Event 4688 or Sysmon Event 1), command lines and parent-child process relationships.
- Review Events 5140 and 5145 and relevant SMB telemetry for share access.
- Examine `HR-PC-02` for corresponding logons, process execution, file access and persistence.
- Verify account creation and group changes against approved change-management records.
- Review services, scheduled tasks, startup entries, registry changes and other persistence locations.
- Determine whether other accounts or systems were accessed and reset credentials where compromise is established or reasonably suspected.

### 8.4 Recovery and prevention

- Remove confirmed malicious components only after evidence preservation.
- Restore affected systems from trusted sources where required.
- Review privileged account membership and apply least privilege.
- Strengthen authentication controls and alerting for repeated failures followed by success.
- Monitor for renewed suspicious activity after recovery.

## 9. Outstanding Investigative Questions

1. Was the `SupportSvc` account creation and Administrators-group membership change authorised?
2. Who or what initiated the account and group changes?
3. What exact privileges appear in the full Event 4672 record?
4. What process launched `cmd.exe`, and what commands were executed?
5. Did `SupportSvc` access shares or files on `HR-PC-02`?
6. Were commands or other processes executed on `HR-PC-02` after the network logon?
7. Were additional systems accessed using the same account or credentials?
8. Was any sensitive information accessed, staged or exfiltrated?
9. Is the endpoint alert involving `update.exe` related to the authentication activity?

## 10. Conclusion

The supplied simulated records show a suspicious `SupportSvc` authentication pattern and a subsequent special-privileges event. Additional scenario evidence raises concerns about account discovery and possible lateral movement.

The activity warrants prompt investigation and a provisional High severity assessment. However, the available evidence does not establish the exact credential attack method, malicious use of the assigned privileges, compromise of `HR-PC-02`, or data exfiltration.

Further validation using original event records, endpoint telemetry, network evidence and change-management records is required before those conclusions can be confirmed.

---

## Appendix A: Evidence Handling Checklist

For each collected item, record:

- Evidence ID
- Description and source host
- Original file name and path
- Collection date and time, including time zone
- Collector name
- Collection method and tool/version
- SHA-256 hash at collection
- Storage location and access controls
- Every transfer, access or examination (chain of custody)
- Hash verification results

## Appendix B: Evidence Status Legend

- **Supplied simulated record:** Present in one of the two CSV datasets created for this project.
- **Illustrative scenario evidence:** Discussed during the exercise but not yet supported by a separately collected source record.
- **Suspected:** An interpretation that requires corroboration.
- **Not established:** No sufficient evidence currently supports the conclusion.

## Appendix C: Project Evidence Integrity Record

The project contains a separate Evidence Integrity Register and Evidence Handling Log under the `evidence/` directory. The register records the SHA-256 baseline values and the comparison results for the working copies.

The evidence handling exercise recorded the following process:

1. Calculate SHA-256 baseline hashes for both original CSV files.
2. Preserve the original files in `evidence/windows-events/`.
3. Create working copies under `investigation/working-copies/windows-events/`.
4. Recalculate SHA-256 hashes for the originals and working copies.
5. Confirm that each working-copy hash matches its corresponding original-file hash.

**Verification result on 18 November 2026:** Both file pairs matched. The working copies were therefore suitable for analysis against the recorded baseline at the time of verification.

**Forensic limitation:** A matching hash demonstrates matching file contents at the time of comparison. It does not prove source authenticity, completeness, or that the original data had not been altered before the baseline hash was calculated.
