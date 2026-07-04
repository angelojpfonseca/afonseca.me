# Angelo Fonseca — profile reference

Consolidated, public-safe facts about the site's owner. Purpose: any future work on
this site (copy, CV, design) starts here instead of re-researching. Everything in
this file is already public (site, CV PDF, LinkedIn) or cleared for public use.
Anything *not* in this file falls under the privacy constraints in
`docs/superpowers/specs/2026-05-26-afonseca-me-redesign-design.md` — check there
before adding new personal facts to any page.

Last reviewed: 2026-07-04.

## Identity

- **Full name:** Angelo Junio Pereira Fonseca. Public byline: Angelo Fonseca.
- **Location:** Mannheim, Germany (city-level only — never more precise).
- **Languages:** Portuguese (native), German (fluent), English (fluent).
- **Contact:** contact@afonseca.me (rotatable alias). No phone number anywhere, ever.
- **Profiles:** [github.com/angelojpfonseca](https://github.com/angelojpfonseca),
  [linkedin.com/in/angelojpfonseca](https://linkedin.com/in/angelojpfonseca).

## The arc (one paragraph)

Brazilian mechanical engineer who moved deliberately into data and AI work.
Trained in Brasília (Diplom-Ingenieur, Centro Universitário do Distrito Federal,
2010–2017), spent a two-year exchange at TU Berlin under *Ciência sem Fronteiras*
(2015–2017) while working as a student research assistant at Fraunhofer IPK.
Since June 2018, Pre-Sales Consultant at Daikin Airconditioning Germany GmbH,
where the role has shifted from classical HVAC engineering toward data and
automation: Python and Power BI tooling, reports and dashboards, a digital
process redesign that cut the Pre-Sales team's annual workload by roughly ten
percent, and — currently — retrieval-augmented LLM tooling for
customer-quotation workflows, as part of Daikin's internal RPA & AI team.
Earlier: internship at Johnson Controls, Brasília (2013–2014), Application
Engineering / Building Efficiency.

## Skills (as publicly stated on CV)

- **Data:** Python, SQL, Pandas, NumPy, Power BI, R, Matlab, MySQL, MongoDB.
- **AI/LLM:** RAG, OpenAI / Anthropic / Gemini / Ollama, LangChain,
  Hugging Face Transformers, multi-agent frameworks (Autogen, CrewAI).
- **Infra:** Linux, Docker, AWS, GCP, Git.
- **Domain:** HVAC systems (Daikin, Johnson Controls), building automation,
  P&ID, SolidWorks, industrial measurement technology (tactile/optical/electronic).

## Personal technical life (public-safe descriptions only)

Described generically per privacy rule 7 — no project codenames, no machine names:

- Runs a **multi-site personal infrastructure** (home lab + VPS) as
  infrastructure-as-code: Terraform, Ansible, WireGuard, Proxmox, Caddy,
  default-deny network segmentation. The VPS serves this site.
- Builds **local-first AI/agent systems**: local LLM inference, agent memory
  tooling, automation assistants. Hard rule: personal data is processed locally
  or not at all.
- **Smart home** on Home Assistant with VLAN-segmented IoT (ESPHome, Zigbee).
- Long-running hobby threads: 3D printing (including self-designed printer
  electronics), microcontroller/sensor projects, small robotics experiments,
  an LLM-driven role-play system for tabletop D&D.
- Keeps his life in plain text: git-versioned notes, documentation-first habits.

## Voice and design taste (how to write and build for him)

- First person, concrete, hedged claims ("roughly ten percent"), en-dash asides.
  No exclamation marks, no taglines, no "passionate about", no LinkedIn-speak.
- Header pages English-only; Portuguese allowed where natural on personal pages.
- Rejects AI-generated aesthetics: no amber palettes, no hero+card grids,
  no CTA blocks, no emoji in content, no promotional framing.
- Facts over adjectives. Empty sections are removed, never marked "coming soon".
- Approved palette: forest green (see v0.2/v0.3 specs). Serif body
  (Iowan Old Style stack), JetBrains Mono for code and labels.

## Fixed site facts

- CV: `/cv.pdf` (German). Markdown source of truth: `docs/cv/cv.md`.
- Deploy: VPS via quilombo Ansible role — see `CLAUDE.md`. Never add a second
  deploy path.
- No analytics, no trackers, no third-party requests, no contact forms.
