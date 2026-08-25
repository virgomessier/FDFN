# Outpost

An easier way to deploy a proxy service for users who are not Linux administrators, controlled through Telegram.

## Problem
Running Xray manually requires knowledge of Linux, bash, systemd, firewalls and more.
Ready-made solutions such as 3x-ui or SSPanel solve different problems (scaling, payments) and have many more settings and heavier deployment requirements.

## Solution
Outpost automates the deployment of the whole system and gives you a management interface in Telegram only. The user does not install any tools on their own machine (Ansible, Docker, an SSH client). They just fork the template repository, add a couple of secrets and press the GitHub Actions button — after that, the server is ready.

## Who it is for
Outpost targets users who know how to rent a VPS and work with SSH keys. They also need basic GitHub skills:

- repositories
- templates
- secrets
- Actions

Outpost is for people who need their own proxy for friends and relatives (about 5–10 people). It is not intended for resale or as a public service.

## Constraints
One VPS (1 vCPU, 1 GB RAM). One administrator. Host-native, no Docker.

## What Outpost does not promise
Outpost does not guarantee that it evades DPI or TSPU, and it cannot guarantee that it bypasses censorship in your country.

1. Your server IP can get banned — the fix is a new server, not a new configuration.
2. The protocol profile may stop working — Outpost ships one profile (VLESS + REALITY). If DPI learns to detect it, Outpost has nothing to switch to. Support for other protocols is not part of the MVP.