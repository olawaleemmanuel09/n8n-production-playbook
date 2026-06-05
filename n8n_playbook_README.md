# n8n Production Playbook

Practical patterns for running n8n in production — written from real operational experience hardening a multi-branch payment system.

Most n8n tutorials cover happy-path workflow building. This playbook covers what comes after: rotating secrets, handling silent failures, recovering from database corruption, encrypting backups, and verifying every change byte-by-byte before declaring victory.

If you're operating an n8n installation that handles real money, real customer data, or real operational consequences — this is for you.

---

## What's inside

| Pattern | What it solves |
|---|---|
| [Snapshot-before-changes](patterns/01_snapshot_before_changes.md) | Recoverable workflow patches via DB snapshot + container restart |
| [Nano-placeholder secrets pattern](patterns/02_nano_placeholder_secrets.md) | Setting environment variables without secrets leaking into shell history |
| [Encrypted offsite backups](patterns/03_encrypted_backups.md) | GPG AES-256 backups with verified decryption |
| [Error trigger workflow](patterns/04_error_trigger_workflow.md) | 10-second Telegram alerts on any workflow failure |
| [Webhook signature verification](patterns/05_webhook_signature_verification.md) | HMAC-SHA512 verification, idempotency, and amount validation |
| [Payment audit log](patterns/06_payment_audit_log.md) | Write-once forensic trail per payment event |
| [Daily reconciliation](patterns/07_daily_reconciliation.md) | Nightly cross-check against payment provider, with alerting |
| [Read upstream before patching](patterns/08_read_upstream_first.md) | The hard-earned rule that prevents 5-iteration debugging cycles |
| [Credential rotation across caching layers](patterns/09_credential_rotation.md) | Why "rotate the key" is never a single change |
| [Silent failure detection](patterns/10_silent_failure_detection.md) | Why `onError: continueRegularOutput` on money paths is a trap |

---

## Why these patterns

These aren't theoretical. Every pattern in this repo was learned by paying for it in production:

- **Silent failure** taught by a real customer who paid for an order the kitchen never received
- **Credential rotation** taught by a service key that broke three caching layers after deletion
- **Trailing whitespace** taught by a two-hour debugging session that found one invisible character
- **Encrypted backups** taught by realising the cloud-storage credential and the backup credential were both in the same wallet
- **Database corruption recovery** taught by a SQLite database that corrupted mid-write during a routine update

The "playbook" is the set of habits I'd want any engineer inheriting one of my systems to follow. It's also the set of habits I wish I'd had before I needed them.

---

## How to use this repo

1. **Read the pattern that matches your current problem.** Each one is 2-5 pages with example code, the failure mode it addresses, and the verification steps.
2. **Adapt, don't copy-paste.** Every example is sanitized and generic. You'll need to substitute your own workflow IDs, sheet IDs, and service-specific details.
3. **Verify after applying.** Each pattern includes a verification step. Don't skip them. The whole point of the playbook is that "I think I applied it" isn't the same as "I verified it worked."

---

## What this repo is NOT

- **Not a tutorial.** If you're learning n8n from scratch, start with the official docs. This assumes you already build workflows and now need to run them in production.
- **Not the code for any specific client system.** Everything here is sanitized patterns. The original system that taught me these lessons stays private out of respect for the business it serves.
- **Not a finished product.** Production engineering is iterative. New incidents teach new patterns. I'll add them as I learn them.

---

## Contributing

If you're running n8n in production and have a pattern that's saved you from a real incident, open a PR or an issue with the story. I'm interested in the kind of patterns that come from real operational pain, not from theory.

---

## About

I'm Olawale Emmanuel Olaonipekun, an AI Automation Engineer based in Lagos, Nigeria. I build and operate production workflow systems — most of them powered by n8n.

[Portfolio](https://olawaleemmanuel09.github.io/) · [LinkedIn](https://www.linkedin.com/in/olawale-olaonipekun-0505303b4) · [Email](mailto:olawaleemmanuel100@gmail.com)

---

*MIT licensed. Use freely. If a pattern saves you from a bad day, that's the only thanks I need.*
