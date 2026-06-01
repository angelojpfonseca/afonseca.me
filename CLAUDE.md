# CLAUDE.md — afonseca.me

Agent guidance for this repository. Read before proposing deploy/CI changes.

## What this is

Source for the personal website <https://afonseca.me>. Hugo + PaperMod theme.

## Deployment — single source of truth

**The live site is deployed by the quilombo `hugo-site` Ansible role onto the
`pantanal` VPS.** pantanal builds from this repo's `main` and serves the result
via Caddy at `/var/www/afonseca.me`. That is the only path that touches the live
domain.

- Deploy mechanism: quilombo `ansible/roles/hugo-site/` + playbook
  `playbooks/pantanal-hugo.yml`.
- Trigger: push to `main` here, then run the playbook
  (`ansible-playbook -i inventory/hosts.yml playbooks/pantanal-hugo.yml --ask-become-pass`)
  from the quilombo control node.

### Do NOT

- **Do not add a GitHub Pages, Netlify, Vercel, or local-rsync deploy.** A second
  publish path produces a divergent live site. The original GitHub Pages workflow
  (`.github/workflows/deploy.yml`) was **removed 2026-06-01** precisely to avoid
  this redundancy — do not reintroduce it.
- **Do not change the live serving host or webroot here.** That lives in quilombo
  (`inventory/host_vars/pantanal/`), not in this repo.

## Version pins

- **Hugo `v0.161.1`** — canonical pin is in quilombo
  `ansible/roles/hugo-site/defaults/main.yml`. If you bump Hugo, change it in
  **both** that file and this repo's `README.md`, and test before pinning.
- **PaperMod `v8.0`** — git submodule (`themes/PaperMod`). Manual upgrade after
  testing only; never automatic.

## References

- Design spec: `docs/superpowers/specs/2026-05-26-afonseca-me-redesign-design.md`
- Implementation plan: `docs/superpowers/plans/2026-05-26-afonseca-me-v0.1.md`
- Deploy role: quilombo `ansible/roles/hugo-site/`
