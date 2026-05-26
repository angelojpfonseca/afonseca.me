---
title: "Homelab"
date: 2026-05-26
draft: false
summary: "A self-hosted homelab built around local-first principles — services running on my own hardware where possible."
role: "Architect, operator"
period: "2024 – present"
stack: ["Linux", "Containers", "Wireguard", "Caddy"]
links: []
featured: true
---

A small homelab I run on the side, built around a single rule: anything dealing with personal or sensitive data runs locally on hardware I control, on a network with default-deny policies. Cloud services are used only where no local alternative exists.

The interesting tension is operating cost: every service has a maintenance budget, and the lab is only useful if I can keep it healthy without it eating my evenings. The architecture has gone through several rounds of simplification — fewer moving parts, more boring choices, tighter monitoring.

A write-up of specific components and the lessons I have learned will land here as I have time to write them properly.
