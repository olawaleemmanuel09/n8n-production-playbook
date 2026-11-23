# Production Automation Engineering — Lessons from Real Operations

**Engineering field notes on making business automation safer to change, recover and operate.**

Most workflow tutorials show the happy path. These notes focus on the difficult parts: unknown upstream behavior, repeated payments, partial failures, credentials, backups, rollback and recovery.

## Published content vs planned series

**Published in this repository:** [`patterns/01_snapshot_before_changes.md`](patterns/01_snapshot_before_changes.md) — a practical snapshot-first habit before modifying running automation.

**Planned topics, not yet published here:** credential rotation, encrypted recovery snapshots, webhook verification, idempotency, payment reconciliation, error alerting and provider-outage handling.

## Why it matters

Production systems are different from tutorials. A workflow can appear to succeed while downstream delivery fails. Updating an API key can break multiple consumers. A test environment can hide constraints that become visible under real operations.

I approach those failures with five habits:

1. **Write down the business invariant.** Example: a retry must not create a second paid order.
2. **Take an independently restorable snapshot before changing anything.**
3. **Make the smallest reversible change.**
4. **Test both success and failure paths, using synthetic data where possible.**
5. **Observe, document and rehearse recovery, not just deployment.**

The repository is intentionally sanitized. It does not include private customer data, production credentials or the code for a client installation.

## Who it's for

AI automation engineers, integration engineers and operations teams who maintain n8n workflows involving money, CRM records, customer communications or other irreversible side effects.

**Portfolio:** https://olawaleolaonipekun.vercel.app
