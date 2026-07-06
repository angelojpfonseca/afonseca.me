# afonseca.me

Personal website source. Built with Hugo; layouts and CSS are hand-written in
this repo — no theme since v0.3 (2026-07-04).

- Hugo: pinned to `v0.161.1`. Canonical pin lives in the quilombo `hugo-site`
  role (`ansible/roles/hugo-site/defaults/main.yml`); bump there and here together.
- Fonts: self-hosted IBM Plex Serif + JetBrains Mono in `static/fonts/`,
  pinned — sources, licenses, and hashes in `docs/fonts.md`.
- Design spec (current): `docs/superpowers/specs/2026-07-04-afonseca-me-v0.3-redesign.md`.
- Owner profile reference: `docs/profile.md`. CV source of truth: `docs/cv/cv.md`.

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
