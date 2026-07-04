---
title: "Home Lab"
lastmod: 2026-07-04
draft: false
---

## Tools

What I work with day to day. Not affiliate links, not endorsements — just what I happen to use.

### Machine and OS

- A Linux laptop as the primary working machine.
- A Windows machine for software that does not yet have a Linux equivalent in my workflow.

### Tools

- **Editor:** Visual Studio Code with vim keybindings.
- **Terminal:** the default that ships with the OS, plus `tmux` for sessions.
- **Languages I am actively using:** Python for data work, a steady amount of Bash, growing Go familiarity.
- **AI assistance:** Claude Code in the terminal, mostly for thinking out loud and for repetitive scaffolding work.

### Services

- **Git hosting:** GitHub and Codeberg.
- **Email:** ProtonMail for personal mail; a domain-aliased forwarder for the contact address on this site.
- **Password management:** an open-source password manager, self-hostable.

This list is intentionally shallow. I update it when something earns a place I expect to keep.

---

## How this site is built

This site is built with [Hugo](https://gohugo.io) — no theme; the layouts and the single stylesheet are written by hand in this repo. Source is on [GitHub](https://github.com/angelojpfonseca/afonseca.me).

- **Typography:** system serif stack (Iowan Old Style / Charter / IBM Plex Serif), JetBrains Mono for code and labels.
- **No JavaScript** beyond a small hand-written light/dark toggle.
- **No analytics, no trackers, no embeds**, no third-party requests at runtime.
- **Hosted on a personal VPS** behind Caddy, served over HTTPS with a Let's Encrypt certificate. Builds are pushed to the server via Ansible.

Design and implementation specs live under `docs/superpowers/` in the repo.
