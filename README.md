# n8n Production Playbook

Operational notes from maintaining business workflows. Each published pattern describes a failure mode, a recovery procedure and a way to check the result.

## Published

[Snapshot before changes](patterns/01_snapshot_before_changes.md) explains how to preserve a recoverable starting point before patching a workflow. Review the assumptions against your own storage layout and rehearse the procedure with a disposable copy.

## Planned notes

Future topics include credential rotation, encrypted backups, webhook verification, payment audit records, reconciliation and silent failures. The snapshot guide is the published chapter available in this repository today.
