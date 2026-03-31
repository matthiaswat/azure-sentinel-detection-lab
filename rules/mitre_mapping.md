# MITRE ATT&CK Mapping

## Impossible Travel
<img width="238" height="53" alt="image" src="https://github.com/user-attachments/assets/08ee6f51-5d67-459e-b529-660cd67f37d4" />
<br>
<img width="469" height="419" alt="image" src="https://github.com/user-attachments/assets/0b807055-d9c3-47bf-b8c1-eb15d28b99f1" />

| Tactic | Technique | Technique ID | How it applies |
|--------|-----------|--------------|----------------|
| Initial Access | Valid Accounts | T1078 | Compromised credentials used to log in from multiple locations |
| Credential Access | Unsecured Credentials | T1552 | Potential credential theft enabling multiple logins |

## Failed Logins Attempts
<img width="863" height="222" alt="image" src="https://github.com/user-attachments/assets/7ae85efc-06e1-41d0-8274-8a2c5a21ffa7" />
<img width="492" height="392" alt="image" src="https://github.com/user-attachments/assets/fee832ea-93df-4e12-9e5a-decba088854a" />

| Tactic | Technique | Technique ID | How it applies |
|--------|-----------|--------------|----------------|
| Credential Access | Brute Force | T1110 | Multiple failed login attempts in short time window |
| Credential Access | Password Guessing | T1110.001 | Automated attempts to guess passwords |

## Detection Coverage
| Attack Phase | Detected by |
|--------------|-------------|
| Reconnaissance | (not covered) |
| Initial Access | ✅ Impossible Travel |
| Credential Access | ✅ Failed Logins Attempts |
| Persistence | (not covered) |
| Lateral Movement | (not covered) |
| Exfiltration | (not covered) |

## MITRE ATT&CK Navigator - Coverage view
<img width="200" height="694" alt="image" src="https://github.com/user-attachments/assets/0c9e1374-bf95-486d-876d-bccfc4c3f368" />
<img width="292" height="694" alt="image" src="https://github.com/user-attachments/assets/39960cd1-cf2c-4d19-829d-a12b022415c6" />
<img width="288" height="694" alt="image" src="https://github.com/user-attachments/assets/be0f8b73-57a7-43b7-8565-c40349713bcd" />

