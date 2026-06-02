# afonseca.me — v0.2 redesign

**Date:** 2026-06-02
**Status:** Approved, ready for implementation planning.

## What this is

A redesign of the home page layout, information architecture, and visual direction.
The v0.1 design had three problems: content too narrow on wide screens, a promotional tagline,
and a colour palette that looked AI-generated. This spec replaces those decisions.

## Information architecture

### Navigation (header)

| Item | URL | Notes |
|---|---|---|
| About | `/about/` | Bio, reading list, CV, social links |
| Projects | `/projects/` | Existing case studies |
| Home Lab | `/homelab/` | Tools (replaces `/uses/`), Colophon |

3D Printing (`/3d-printing/`) and DIY (`/diy/`) are planned sections, added to the nav
**only when they have real content**. Do not create placeholder pages.

### Page inventory

| URL | Page | Status |
|---|---|---|
| `/` | Home | Redesigned |
| `/about/` | About | Expanded — absorbs Reading, social links, CV link |
| `/projects/` | Projects index | Unchanged |
| `/projects/<slug>/` | Project case study | Unchanged |
| `/homelab/` | Home Lab | New — merges `/uses/` and `/colophon/` |
| `/now/` | Now | Kept, not linked from nav or home; accessible via direct URL |
| `/contact/` | Contact | Kept at URL, not in nav |
| `/cv.pdf` | CV | Unchanged |

`/writing/` remains at its URL but not in nav until first post exists.

### Dropped from navigation

- **Contact** — email address is in the hero on the home page and in the About page. A dedicated nav item is redundant.
- **Now** — kept as a page at `/now/` but not advertised. People who know the `/now` convention can find it.
- **Footer nav** (Now · Uses · Reading · Colophon) — removed. Footer shows only copyright.

## Home page

### Structure

Everything fits above the fold on a standard laptop screen. No scrolling required to see all sections.

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
  Full about page — reading list, CV →

[Projects card]     [Home Lab card]
  Projects            Home Lab
  Brief description   Brief description
  See all →           Tools, colophon →
─────────────────────────────────────────────────────
© 2026 Angelo Fonseca
```

### Hero

- **Name:** `Angelo Fonseca` — large, no tagline
- **Label:** `Mechanical engineer. Mannheim, Germany.` — factual, no promotional framing
- **CV link:** inline after the label, `CV (PDF) ↗`
- **Social links:** horizontal row of pill-style links with SVG icons — GitHub, LinkedIn, email
- No separate Contact section on home

### Cards

Card titles are `h2` elements — visible, prominent, not the v0.1 faint uppercase label style.
Three cards:
1. **About** — full width (2 columns). Bio excerpt, link to full about page.
2. **Projects** — half width. One-line description, "See all projects →" link.
3. **Home Lab** — half width. One-line description, "Tools, colophon →" link.

## Visual direction

### Colour palette — Forest green

| Token | Value | Use |
|---|---|---|
| Background | `#f5f6f2` | Page background |
| Surface | `#ffffff` | Cards |
| Border | `#d5d9cd` | Card borders, dividers |
| Text primary | `#181c17` | Body text, headings |
| Text secondary | `#525a50` | Nav links, meta text |
| Text muted | `#8a9088` | Footer, timestamps |
| Accent | `#2d5a3d` | Links, card headings, social pill hover |

Dark mode: PaperMod's built-in toggle remains available. Define a matching dark variant in `custom.css`.

### Typography

Unchanged from v0.1 — system serif stack (Iowan Old Style / Charter / IBM Plex Serif / Georgia).
No web fonts.

### Layout

- **Content max-width:** `960px` (up from `720px` in v0.1)
- **Home page:** single-column max-width container, 2-column card grid below the hero
- **Inner pages** (About, Projects, Home Lab): reading-width `680px` centred within the `960px` container

### No dark default

v0.2 switches to **light as default** (`defaultTheme = "light"` in `hugo.toml`).
The theme toggle remains so users can switch to dark manually.

## About page

Absorbs content currently spread across separate pages:
- Bio (existing `about.md`)
- Reading list (existing `reading.md` — merged as a section within About)
- Social links and CV link (previously footer / home only)

`/reading/` stays as a standalone page. The About page gets a visible link to it
("Reading list →"). Do not merge files — Hugo handles them separately.

## Home Lab page

New page at `/homelab/`. Absorbs:
- **Tools** (content from existing `uses.md`)
- **Colophon** (content from existing `colophon.md`)

Structured as two sections on one page: "Tools" then "How this site is built."
`/uses/` and `/colophon/` stay as thin pages with a single line pointing to `/homelab/`.
No content removed — just a "This page has moved to Home Lab" note with a link.

## Implementation notes

### `hugo.toml` changes

- `defaultTheme = "light"` (was `"dark"`)
- `.main { max-width: 960px }` in `custom.css` (was `720px`)
- Menu rewrite: About · Projects · Home Lab

### CSS changes (`assets/css/extended/custom.css`)

- Replace `--afm-accent: #b87a4b` with `--afm-accent: #2d5a3d`
- Add full palette variable set (background, surface, border, text tokens)
- Update light-mode overrides to use new palette
- Update dark-mode overrides to match

### New layout files

- `layouts/index.html` — rewrite hero + cards structure
- `layouts/partials/footer.html` — strip nav links, keep only copyright
- `content/homelab.md` — new leaf page (not a section; no sub-pages needed)

### Social icons

Use PaperMod's existing `social_icons.html` partial for consistency where possible.
For the hero pill-style row, inline SVG is acceptable — GitHub, LinkedIn, email icons only.

## Content privacy constraints

All constraints from the v0.1 spec remain in force — no fatherhood, no partner details,
no exact address, no birthdate, no phone. See `docs/superpowers/specs/2026-05-26-afonseca-me-redesign-design.md`.

## Success criteria

1. Home page fits above the fold on a 1080p laptop screen (1366×768 or larger).
2. Card headings (About, Projects, Home Lab) are visually prominent — same weight as body `h2`.
3. Accent colour is `#2d5a3d` throughout — no remaining amber (`#b87a4b`).
4. Nav shows About · Projects · Home Lab only.
5. Footer shows only copyright — no nav links.
6. Social pills in hero render with correct SVG icons on all three entries.
7. Hugo builds clean, no broken internal links.
8. Light mode is the default; dark toggle still works.
