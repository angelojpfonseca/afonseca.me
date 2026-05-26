---
title: "Homelab and side experiments"
date: 2026-05-26
draft: false
summary: "A small home lab for virtualisation and AI experiments, plus a handful of hobby projects in 3D printing, electronics, and robotics."
role: "Builder"
period: "ongoing"
stack: ["Linux", "Hypervisors", "Microcontrollers", "3D printing", "LLMs", "Multi-agent frameworks (Autogen, CrewAI)"]
links: []
featured: true
---

A small home lab I run on the side, built around a single rule: anything dealing with personal or sensitive data runs locally on hardware I control, on a network with default-deny policies. Cloud services are used only where no local alternative exists.

The lab does double duty — it virtualises a few network and infrastructure scenarios I want to be able to break safely, and it hosts the compute behind whatever experiment I am currently curious about. Recent ones include a small LLM-driven role-play system for tabletop Dungeons & Dragons sessions (dynamic scenarios, NPC interactions, all running locally on the lab), and ad-hoc multi-agent setups in Autogen and CrewAI to test what these frameworks actually do well and where they fall over.

Around the lab there are smaller hobby threads I keep returning to: 3D printing for prototyping and small-run parts (including modifying the printers themselves with self-designed electronics), microcontroller projects for sensors and home automation, and the occasional robotics experiment that uses the lab as its brain.

The interesting tension is operating cost: every service has a maintenance budget, and the lab is only useful if I can keep it healthy without it eating my evenings. The architecture has gone through several rounds of simplification — fewer moving parts, more boring choices, tighter monitoring.
