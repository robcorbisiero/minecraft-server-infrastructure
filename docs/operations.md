# Maintenance and things left to do

All three servers were working after the Paper migration. This page is my checklist for the maintenance side.

## Backups

For Bedrock, I created a recovery archive, checked that tar could read it, and restarted the service successfully. The commands and output are in the [Bedrock notes](bedrock-migration.md#5-validate-a-recovery-archive).

On November 4, I checked the backup directory again: it contained one 375 MB archive from October 1. The filtered systemd timer list showed only Ubuntu's package-database backup. I haven't found a Minecraft timer; the follow-up cron results are below. [Screenshots](../servers/bedrock/README.md#service-check--november-4-2026).

The follow-up cron checks found no crontab for `rob` or `root` and no Minecraft entries in `/etc/crontab` or `/etc/cron.d`. Automatic Bedrock backups are still a task to set up or locate elsewhere; the October archive is the only file shown in the checked backup directory.

For ATM10, I worked on `atm10-backup.service` and checked its errors and directory permissions. I still need to document the final schedule and a successful recurring run. Automatic Paper backups were also on the follow-up list.

The next steps are to confirm retention, keep copies away from the server's own disk, and restore a backup into a separate test location. Reading an archive is useful, but I want to know I can actually recover a playable world.

## Updates

Before updating a server or modpack, I want a backup and a record of the version I'm replacing. Afterward, I check startup logs and join the server to make sure the worlds and plugins still work. I'll add a more detailed update procedure as I settle on one.

## Checklist

- [ ] Confirm backup schedules and retention for all three servers.
- [ ] Check off-host backup copies.
- [ ] Complete and document a restore test.
- [x] Add the installed Bedrock systemd unit.
- [ ] Add the installed ATM10 and Paper systemd units with secrets removed.
- [ ] Check clean shutdown behavior.
- [ ] Add CPU, RAM, disk, and Minecraft performance monitoring.
- [ ] Confirm cleanup of Paper's migration staging folder.

Worlds, player lists, raw logs, and credentials stay outside this repo.
