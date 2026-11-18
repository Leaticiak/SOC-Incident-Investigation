# Evidence Integrity Register

**Project:** SOC Incident Investigation  
**Hash algorithm:** SHA-256  
**Baseline recorded:** 18 November 2026  
**Purpose:** Record reference hashes for the original simulated evidence files so their contents can be checked for changes during later handling.

| Evidence ID | File name | Relative path | Algorithm | Baseline SHA-256 |
|---|---|---|---|---|
| E-001 | `soc_project4_simulated_evidence.csv` | `evidence/windows-events/soc_project4_simulated_evidence.csv` | SHA-256 | `ED63E20919B5841CA19C125B0E7F00B2D406F511072690698882F22F1B23C2C0` |
| E-002 | `supportsvc_authentication_events.csv` | `evidence/windows-events/supportsvc_authentication_events.csv` | SHA-256 | `DDCBB1AA8A232302022C0BCDCC018AB5964EF98365F59A3EADE682931CA2B979` |

## How to verify later

Run these commands from the `evidence\windows-events` folder in PowerShell:

```powershell
(Get-FileHash .\soc_project4_simulated_evidence.csv -Algorithm SHA256).Hash
(Get-FileHash .\supportsvc_authentication_events.csv -Algorithm SHA256).Hash
```

Compare each new 64-character hexadecimal result with its baseline value above. An exact match indicates that the file's contents match the recorded baseline at the time of comparison. A mismatch means the file contents differ and should be investigated; it does not, by itself, prove tampering.

## Evidence-integrity limitations

- These are **baseline hashes**. They do not prove that the files were authentic before hashing, who created them, or where they originated.
- A matching SHA-256 hash supports that the file contents have not changed since the baseline was recorded, subject to the security properties of SHA-256.
- Keep the original evidence files unchanged. Perform analysis on working copies where practical.
- Record any later hash checks, transfers, copies, or handling actions in the investigation notes, including the date/time, person, action, and result.
- The CSV files in this exercise contain simulated evidence.
