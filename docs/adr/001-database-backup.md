# 001. Do not build a backup system for SQLite in MVP

## Status
Accepted | 2026.08.14

## Context  
A non-scalable system. Average active users 5-10 (friends and relatives). Recovery comes down to re-issuing links. 1 VPS for the whole infrastructure. There is no payment system and no logins or passwords.

## Decision
We do not build a backup system. Re-issuing subscriptions is faster than a backup cycle.


**Rejected options:**
- Backup in S3 Bucket: One more action for the user who deploys this architecture. It also requires an S3 account and bucket management.

- Cron task on the same VPS: The backup dies together with the server, so it does not cover the main scenario.

- Storage in GitHub: History on GitHub is permanent, collaborators have access to it, and a private repository can be made public by accident.

**Deferred options:**
- Dump in Telegram: requires a decision on encryption — Telegram has no E2E for bots.

## Consequences

1. What became easier: 
- Faster deployment of the whole infrastructure, fewer actions for the user.

2. What became worse:
If the server is gone, we lose all data on that server. Namely:
- Private key for Reality
- shortIds
- inbound parameters and other settings for vpn-manager

3. What we must do now:
- Ansible must generate a new private key for Reality on a clean deploy
- Telegram bot must re-create users and issue subscription links in one command