# CLAUDE.md — afonseca.me

Agent guidance for this repository. Read before proposing deploy/CI changes.

## What this is

Source for the personal website <https://afonseca.me>. Hugo with hand-written
layouts and a single hand-written stylesheet — **no theme** since v0.3
(2026-07-04; PaperMod submodule removed). Facts about the site's owner for
copy/CV work: `docs/profile.md` (public-safe; check the v0.1 spec's privacy
constraints before adding personal facts anywhere).

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

## Local development

**Hugo is not installed on the local machine.** Build verification only happens on deploy via the Ansible playbook. When writing implementation plans or verify steps, do not rely on `hugo --minify` or `hugo server` locally — verify templates and CSS by inspection instead. Install from the quilombo pin if local builds become needed (quilombo #279).

## Theme-toggle gotcha

The light/dark toggle needs both halves: the FOUC guard in
`layouts/partials/head.html` and the `addEventListener` script at the end of
`layouts/partials/footer.html`. Keep both when touching those partials, or the
header button silently does nothing.

## References

- Design spec (v0.3, current): `docs/superpowers/specs/2026-07-04-afonseca-me-v0.3-redesign.md`
- Design spec (v0.2, superseded): `docs/superpowers/specs/2026-06-02-afonseca-me-v0.2-redesign.md`
- Design spec (v0.1, privacy constraints still in force): `docs/superpowers/specs/2026-05-26-afonseca-me-redesign-design.md`
- Owner profile reference: `docs/profile.md` · CV source: `docs/cv/cv.md`
- Deploy role: quilombo `ansible/roles/hugo-site/`
