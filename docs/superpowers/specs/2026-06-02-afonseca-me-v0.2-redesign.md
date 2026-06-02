# afonseca.me — v0.2 redesign

**Date:** 2026-06-02
**Status:** Approved, ready for implementation planning.

## What this is

A redesign of the home page layout, information architecture, and visual direction.
The v0.1 design had three problems: content too narrow on wide screens, a promotional tagline,
and a colour palette that looked AI-generated. This spec replaces those decisions.

## Information architecture

### Navigation (header)

| Item | Display name | URL | Notes |
|---|---|---|---|
| About | About | `/about/` | Bio, reading list link, CV, social links |
| Projects | Projects | `/projects/` | Existing case studies |
| Home Lab | Home Lab | `/homelab/` | Tools (replaces `/uses/`), Colophon |

3D Printing (`/3d-printing/`) and DIY (`/diy/`) are planned sections, added to the nav
**only when they have real content**. Do not create placeholder pages.

**Naming note:** The nav item "Home Lab" (`/homelab/`) is a tools/setup page. The existing
project case study `projects/homelab.md` is titled "Homelab and side experiments." These are
two different things. To reduce confusion, rename the project case study title to
"Personal lab and side experiments" in its front matter — the URL `/projects/homelab/` stays
unchanged. This separates the nav concept (tools page) from the project concept (case study).

### Page inventory

| URL | Page | Status |
|---|---|---|
| `/` | Home | Redesigned |
| `/about/` | About | Expanded — adds Reading link, social links, CV link |
| `/projects/` | Projects index | Unchanged |
| `/projects/<slug>/` | Project case study | Unchanged |
| `/homelab/` | Home Lab | New — merges `/uses/` and `/colophon/` content |
| `/reading/` | Reading | Unchanged — linked from About page |
| `/now/` | Now | Kept, not in nav or home; accessible via direct URL |
| `/contact/` | Contact | Kept at URL, not in nav |
| `/cv.pdf` | CV | Unchanged |

`/writing/` remains at its URL but not in nav until first post exists.

### Dropped from navigation

- **Contact** — email is in the hero and on the About page. Nav item redundant.
- **Now** — kept at `/now/` but not advertised. People who know the convention can find it.
- **Footer nav** (Now · Uses · Reading · Colophon) — removed. Footer shows only copyright.

## Home page

### Structure

Everything fits above the fold on a 1366×768 viewport at 100% zoom with default system font size.

```
[Nav: Angelo Fonseca | About · Projects · Home Lab]
─────────────────────────────────────────────────────
Angelo Fonseca
Mechanical engineer. Mannheim, Germany. · CV (PDF) ↗
[GitHub] [LinkedIn] [contact@afonseca.me]
─────────────────────────────────────────────────────
[About card — full width]
  About
  Bio paragraph
  Full about page →

[Projects card]     [Home Lab card]
  Projects            Home Lab
  Brief description   Brief description
  See all →           Tools, colophon →
─────────────────────────────────────────────────────
© 2026 Angelo Fonseca
```

### Hero

- **Name:** `Angelo Fonseca` — large heading, no tagline
- **Label:** `Mechanical engineer. Mannheim, Germany.` — factual, no promotional framing
- **CV link:** inline after the label, `CV (PDF) ↗`
- **Social links:** horizontal row of pill-style links with SVG icons — GitHub, LinkedIn, email
- No separate Contact section on home

### Cards

Card titles are `h2` elements — visible, prominent, not the v0.1 faint uppercase label style.
Three cards:
1. **About** — full width (spans both columns). Bio excerpt, link to full about page.
2. **Projects** — half width. One-line description, "See all projects →" link.
3. **Home Lab** — half width. One-line description, "Tools, colophon →" link.

The home page no longer iterates `featured: true` project front matter. The three cards are
static. The `featured: true` flag on project entries becomes inert — remove it from all project
front matter to avoid confusion.

## Visual direction

### Colour palette — Forest green

Light mode (default):

| Design token | CSS variable | Value | Use |
|---|---|---|---|
| Background | `--theme` | `#f5f6f2` | Page background |
| Surface | `--entry` | `#ffffff` | Cards |
| Border | `--border` | `#d5d9cd` | Card borders, dividers |
| Text primary | `--primary` / `--content` | `#181c17` | Body text, headings |
| Text secondary | `--secondary` | `#525a50` | Nav links, meta text |
| Text muted | — (custom) | `#8a9088` | Footer, timestamps |
| Accent | `--afm-accent` | `#2d5a3d` | Links, card headings, social pill hover |

Dark mode overrides (`.dark` block in `custom.css`):

| Design token | CSS variable | Value |
|---|---|---|
| Background | `--theme` | `#141a14` |
| Surface | `--entry` | `#1e261e` |
| Border | `--border` | `#2d3d2d` |
| Text primary | `--primary` / `--content` | `#e2e8e2` |
| Text secondary | `--secondary` | `#8aaa8a` |
| Accent (dark) | `--afm-accent` | `#4ea86a` |

`#4ea86a` on `#141a14` achieves approximately 7:1 contrast — passes WCAG AA for normal text.
`#2d5a3d` on `#141a14` fails (≈2.5:1) and must not be used as foreground on dark backgrounds.
Verify both values with a contrast checker before merging.

### Typography

Unchanged from v0.1 — system serif stack (Iowan Old Style / Charter / IBM Plex Serif / Georgia).
No web fonts.

### Layout

- **Global container:** `.main { max-width: 960px }` — applies to all pages
- **Home page:** 2-column card grid within the 960px container
- **Inner pages** (About, Projects, Home Lab, reading, now): prose content wrapped in an
  `.afm-prose` container limited to `680px`. The selector is:
  ```css
  body:not(.home) .main .afm-prose { max-width: 680px; }
  ```
  Every inner-page template must wrap its body content in `<div class="afm-prose">`.
  The home page (`layouts/index.html`) does not use `.afm-prose`.

### No dark default

v0.2 switches to **light as default** (`defaultTheme = "light"` in `hugo.toml`).
The theme toggle remains available for users who prefer dark.

## About page

Content:
- Bio (existing `about.md` body — unchanged)
- Visible link to `/reading/` ("Reading list →") added near the end of the bio
- Social links and CV link displayed prominently (can use PaperMod's `social_icons.html` partial
  or inline links — consistent with the hero pill style)

`/reading/` stays as a standalone page. About links to it; its content is not merged into
`about.md`. Hugo handles them as separate files.

## Home Lab page

New leaf page at `content/homelab.md`. Structured as two sections:
1. **Tools** — content migrated from `uses.md`
2. **How this site is built** — content migrated from `colophon.md`

`/uses/` and `/colophon/` stay as thin pages with a single forwarding note:
"This page has moved to [Home Lab](/homelab/)." No content deleted.

## Deployment

Unchanged — VPS via Ansible as documented in `CLAUDE.md`. The v0.1 spec describes GitHub Pages;
that is stale. Do not introduce any new CI or deploy mechanism.

## Implementation notes

### `hugo.toml` changes

- `defaultTheme = "light"` (was `"dark"`)
- Menu rewrite: About · Projects · Home Lab (remove Contact entry)

### CSS changes (`assets/css/extended/custom.css`)

- Set `--afm-accent: #2d5a3d` (was `#b87a4b`) at `:root`
- Add full light-mode palette overrides (PaperMod defaults to light; these override its defaults
  with the forest-green palette values — this is new, v0.1 had no light-mode overrides)
- Rewrite `.dark { … }` block with the dark-mode palette above
- Add `.afm-prose { max-width: 680px; }` rule
- Remove dead CSS: `.afm-footer-nav`, `.footer .social-icons`, `.afm-footer-meta` rules are
  unused after the footer strip — delete them
- Remove `.afm-cards`, `.afm-card`, `.afm-card--project`, `.afm-card--now`, `.afm-card--about`
  rules — replaced by new card styles for the v0.2 home layout

### Layout files

- `layouts/index.html` — rewrite: hero + social pills + 3-card grid
- `layouts/partials/footer.html` — strip to copyright only
- `content/homelab.md` — new leaf page

### Social icons (hero pills)

Inline SVG for the three hero pills (GitHub, LinkedIn, email). Do not use PaperMod's
`social_icons.html` for the hero — that partial renders icon-only links; the pills need
icon + label text. The partial remains in use on the About page if desired.

### Front matter cleanup

- `content/projects/homelab.md`: change `title` to `"Personal lab and side experiments"`,
  remove `featured: true`
- `content/projects/daikin-ai-rag.md`: remove `featured: true`
- `content/projects/personal-website.md`: remove `featured: true`

## Content privacy constraints

All constraints from the v0.1 spec remain in force — no fatherhood, no partner details,
no exact address, no birthdate, no phone number. See
`docs/superpowers/specs/2026-05-26-afonseca-me-redesign-design.md`.

## Success criteria

1. Home page fits above the fold at 1366×768 viewport, 100% zoom, default system font size.
2. Card headings (About, Projects, Home Lab) render as prominent `h2` elements.
3. Light-mode accent `#2d5a3d` and dark-mode accent `#4ea86a` both verified WCAG AA against
   their respective backgrounds before merge. No amber (`#b87a4b`) remains in `custom.css`.
4. Nav shows About · Projects · Home Lab only — no Contact, no Writing.
5. Footer shows `© 2026 Angelo Fonseca` only — no nav links.
6. Social pills in hero: three entries (GitHub, LinkedIn, email), each with correct SVG icon.
7. Hugo builds clean (`hugo --minify`), no broken internal links.
8. Light mode is the default; dark toggle switches correctly to the dark forest-green palette.
9. `/uses/` and `/colophon/` return HTTP 200 with forwarding note — no 404s.
10. `featured: true` removed from all project front matter.
