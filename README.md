# afonseca.me

Personal website source. Built with Hugo and the PaperMod theme.

- Hugo: pinned to `v0.161.1`. Canonical pin lives in the quilombo `hugo-site`
  role (`ansible/roles/hugo-site/defaults/main.yml`); bump there and here together.
- PaperMod: pinned to `v8.0` (git submodule). Manual upgrade after testing — never automatic.
- Design spec: `docs/superpowers/specs/2026-05-26-afonseca-me-redesign-design.md`.
- Implementation plan: `docs/superpowers/plans/2026-05-26-afonseca-me-v0.1.md`.

Live site: <https://afonseca.me>.

## Deployment

The live site is built and deployed by the **quilombo `hugo-site` Ansible role**
(playbook `pantanal-hugo.yml`) onto the **pantanal** VPS, which serves it via Caddy
from `/var/www/afonseca.me`. pantanal is the single source of truth for serving.

To publish changes: push to `main` here, then run the quilombo playbook
(`ansible-playbook -i inventory/hosts.yml playbooks/pantanal-hugo.yml --ask-become-pass`).

Do **not** add a GitHub Pages / Netlify / local-rsync deploy path — that creates a
second, divergent live target. The redundant GitHub Pages workflow was removed
2026-06-01 for this reason. See `CLAUDE.md`.
