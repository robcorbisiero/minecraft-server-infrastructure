# Operations and remaining work

## Recorded validation

The owner confirmed all three servers working after the Paper migration. This is historical evidence; no live server connection or health check was performed for this documentation.

## Next maintenance work

| Task | Recorded status | Evidence needed to mark complete |
| --- | --- | --- |
| Inventory hardware, OS, versions, ports, and runtime paths | Hardware mapping recovered; see inventory | Fresh host inspection to confirm current state |
| Capture service management configuration | Not recovered | Actual sanitized unit files and service status |
| Verify graceful shutdown | Follow-up | Logs showing a clean save and stop |
| Automate backups to separate storage | ATM10 backup service appears in troubleshooting; Bedrock recovery archive verified | Final schedule, successful recurring runs, and off-host destination verification |
| Test restoration | Not recorded | Isolated restore and gameplay validation |
| Monitor CPU, RAM, disk, and application performance | Planned | Actual monitoring configuration and observed metrics |
| Clean Paper migration staging files | Recommended; completion unknown | Disk inspection and confirmed independent backup |

## Recovered backup evidence

Earlier Bedrock terminal output shows a recovery archive passing a tar listing check, followed by the service active and listening on UDP 19132. This verifies archive readability and service restart, not a complete restore test. Older ATM10 messages include `atm10-backup.service` status and error diagnostics, so backup work had progressed beyond planning. The final recurring-job outcome is not yet established. Paper automatic backups remained follow-up work in the reviewed messages.

## Update procedure to establish

Before an update, record the current versions and take a recoverable backup. Stop the workload gracefully, apply the intended server or modpack update, then validate logs and gameplay. Record the result and rollback steps. This is a proposed operating procedure, not a claim of installed automation.

## Publication boundaries

Publish reviewed documentation and sanitized examples. Keep worlds, player identifiers, allowlists, operator lists, logs, secrets, RCON passwords, and raw host configuration outside the repository. Any future scripts should be tested before being described as operational.
