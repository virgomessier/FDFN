# Outpost

One click between a rented VPS and a working personal proxy.

**Status: in design.** There is no code yet. The documents come first.

## What it is

Outpost turns a rented VPS into a personal proxy server. The administrator needs a GitHub
account and a VPS. They install nothing on their own machine and open no terminal after
the first step.

A GitHub Actions workflow runs Ansible over SSH. Ansible sets up the server: Xray with
VLESS + REALITY + XTLS Vision, a firewall, and a Telegram bot. After that the Telegram bot
is the only control panel. The administrator issues and revokes connection links there.

## Who it is for

A person who shares a proxy with five to ten friends and relatives. Not a hosting company,
and not an experienced Linux administrator.

## Constraints

One VPS (1 vCPU, 1 GB RAM). One administrator. Host-native, no Docker.

## Documentation

| Document | What is inside |
|---|---|
| [concept.md](docs/concept.md) | The problem, the solution, and what Outpost does not promise |
| [scope.md](docs/scope.md) | What the MVP includes, and what it leaves out |
| [architecture.md](docs/architecture.md) | Components, data flows, trust boundaries |
| [threat-model.md](docs/threat-model.md) | Assets, threats, mitigations, accepted risks |
| [deployment.md](docs/deployment.md) | How the workflow reaches the server, and who does what |
| [glossary.md](docs/glossary.md) | The words this project uses, and the words it avoids |
| [adr/](docs/adr/README.md) | Decisions, with the reason and the price of each |

## License

MIT. See [LICENSE](LICENSE).
