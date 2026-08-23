# Deployment

This document says how FDFN reaches a rented VPS, and who does what. It is a design
document, not a manual. Exact commands, secret names and file paths come later, when the
workflow exists.

Related: [ADR-002](adr/002-ssh-authentication.md), [ADR-003](adr/003-bootstrap-model.md).

## Prerequisites

- A VPS with Ubuntu 24.04 and a public IPv4 address.
- An SSH key pair, made for FDFN only.
- A Telegram bot, created in BotFather.
- A private repository, created from the FDFN template.

## What the administrator does once

### 1. Create the key pair

One key pair for FDFN. Do not reuse a personal key: the private key goes into GitHub
Secrets, and a key that opens other servers must not go there.

### 2. Put the public key on the VPS

There are two recipes. The provider decides which one is possible.

**The order form.** Some providers have an "SSH keys" field when you create a server. The
administrator pastes the public key there. The server already has the key when it boots.

**`ssh-copy-id`.** Cheaper providers only send a root login and a password by email. The
administrator runs `ssh-copy-id` once from their own machine. The command asks for the
root password one time and copies the public key to the server.

Both recipes end in the same state: the public key is in `/root/.ssh/authorized_keys`.
The playbook does not know which recipe was used, and does not need to know.

### 3. Add the secrets

The workflow needs four things it cannot keep in the repository:

| Secret | Why |
|---|---|
| SSH private key | Ansible logs in with it |
| Server address | Where to connect |
| Telegram bot token | The bot talks to Telegram with it |
| Administrator Telegram ID | The bot answers this account only |

Secrets can be written but not read back. If the administrator loses the private key,
they cannot get a copy from GitHub.

### 4. Add the configuration

Values that are not secret live in a file in the repository. This file is the input of
the playbook. The administrator edits it and commits the change.

### 5. Start the workflow

The workflow runs by hand, from the Actions tab.

## What the workflow does

1. It checks the configuration and the secrets. If something is missing, it stops here.
   This step is cheap, so it runs first.
2. It opens an SSH connection to the server and checks the host key.
3. Ansible runs the bootstrap play as `root`. The play creates `deploy-user`, installs the
   public key, gives `sudo`, and configures `sshd`. See ADR-003.
4. Ansible runs the other plays as `deploy-user`. They install and configure Xray, the
   firewall, `fdfn-manager` and the Telegram bot.
5. It checks the result: the systemd services are running, and Xray accepts its
   configuration.
6. It writes a short report to the GitHub Actions summary.

Steps 3 and 4 are two plays in one playbook. Step 3 runs on a new server and on every
later run. It does the same work every time, so a second run changes nothing.

## Behaviour on failure

The workflow stops at the first critical failure. It does not report success for a part of
the work.

The server can stay half-configured. This is expected. The repair is another run: Ansible
brings the server to the described state again, and finishes the steps that failed. The
administrator does not clean up by hand.

Tasks that handle secrets hide their output. A failed task must not print a token or a
private key into the job log, because job logs are kept and can be read.

## Open questions

- **The host key on the first run.** The runner has never seen the server before, so it has
  nothing to compare the host key with. Two ways out: trust it on first use, or ask the
  administrator for the fingerprint. The first is one step less and one guarantee less.
- **The configuration file.** Where it lives, what its fields are, and what happens when a
  field changes after the first deployment.
- **The names of the secrets.**
