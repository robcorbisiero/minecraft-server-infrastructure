# Maintenance and things left to do

All three servers were working after the Paper migration. This page is my checklist for the maintenance side.

## Backups

For Bedrock, I created a recovery archive, checked that tar could read it, and restarted the service successfully. The commands and output are in the [Bedrock notes](bedrock-migration.md#5-validate-a-recovery-archive).

For ATM10, I worked on `atm10-backup.service` and checked its errors and directory permissions. I still need to document the final schedule and a successful recurring run. Automatic Paper backups were also on the follow-up list.

The next steps are to confirm retention, keep copies away from the server's own disk, and restore a backup into a separate test location. Reading an archive is useful, but I want to know I can actually recover a playable world.

## Updates

Before updating a server or modpack, I want a backup and a record of the version I'm replacing. Afterward, I check startup logs and join the server to make sure the worlds and plugins still work. I'll add a more detailed update procedure as I settle on one.

## Checklist

- [ ] Confirm backup schedules and retention for all three servers.
- [ ] Check off-host backup copies.
- [ ] Complete and document a restore test.
- [ ] Add the installed systemd units with secrets removed.
- [ ] Check clean shutdown behavior.
- [ ] Add CPU, RAM, disk, and Minecraft performance monitoring.
- [ ] Confirm cleanup of Paper's migration staging folder.

Worlds, player lists, raw logs, and credentials stay outside this repo.
