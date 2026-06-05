[playbook_pattern_01_snapshot.md](https://github.com/user-attachments/files/28632698/playbook_pattern_01_snapshot.md)
# Snapshot Before Changes

Every DB-level patch to a running n8n installation needs a recoverable rollback. This is the pattern.

## When to use

- Patching workflows via direct SQLite edits
- Updating credentials via DB modification
- Any change you're not 100% sure about

## The pattern

```bash
PRE=/root/pre_TASK_$(date +%Y%m%d_%H%M%S)
mkdir -p $PRE
docker stop n8n_n8n_1
cp /var/lib/docker/volumes/n8n_n8n_data/_data/database.sqlite $PRE/
cp /var/lib/docker/volumes/n8n_n8n_data/_data/config $PRE/
docker start n8n_n8n_1
sleep 5
ls -la $PRE/
docker ps | grep n8n
```

Replace `TASK` with what you're doing (e.g. `pre_audit_patch`, `pre_credential_rotation`).

## Why every step matters

- `docker stop` before copying: SQLite mid-write is corrupted SQLite
- Copy `database.sqlite` AND `config`: the config file holds the encryption key for stored credentials. Without it, your DB is gibberish.
- `docker start` immediately after: minimize downtime
- `ls -la` and `docker ps` at the end: verify the snapshot exists and the container came back up

## Rolling back

```bash
docker stop n8n_n8n_1
cp $PRE/database.sqlite /var/lib/docker/volumes/n8n_n8n_data/_data/
cp $PRE/config /var/lib/docker/volumes/n8n_n8n_data/_data/
docker start n8n_n8n_1
```

## What this saved me from

A SQLite write that corrupted the workflow database mid-update. Without the snapshot, recovery would have meant rebuilding workflows by memory. With it, recovery was 30 seconds.

## Verification

After any patch, before declaring victory:

```bash
docker logs n8n_n8n_1 --tail 30 | grep "Activated workflow" | wc -l
```

Should match your expected workflow count. If it doesn't, the patch broke something — roll back.
