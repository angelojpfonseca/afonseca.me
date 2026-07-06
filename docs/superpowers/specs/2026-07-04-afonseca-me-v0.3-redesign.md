# afonseca.me — v0.3 redesign: own layouts, document aesthetic

**Date:** 2026-07-04
**Status:** Drafted autonomously (background session); pending Angelo's review.

## Goal

Angelo asked for a site that is *unique to him* and that would not be identified
as AI-generated. v0.2 fixed the palette and the tagline, but two generic tells
remain: the site is recognisably a PaperMod theme (its markup and class names are
fingerprintable, its chrome is shared with thousands of sites), and the home page
is the hero-plus-card-grid layout that generated sites default to.

v0.3 removes the theme entirely and replaces it with a small set of hand-written
Hugo layouts with a design language drawn from Angelo's actual identity: a
mechanical engineer's document. Everything else — palette, typography choice,
information architecture, privacy posture, deploy path — is kept from v0.2.

## Approaches considered

- **A. Rewrite the home page inside PaperMod.** Cheapest; but the theme
  fingerprint and chrome remain, so the "not AI-generated" goal is only half met.
- **B. Drop PaperMod; hand-written layouts (~8 small templates), document
  aesthetic.** Full control of markup and design; removes the theme submodule
  and its maintenance pin; the site becomes genuinely one of a kind. Cost:
  templates must be maintained here, and the build must be re-verified.
  **Chosen.**
- **C. Concept site** (e.g. skeuomorphic technical-drawing frames throughout).
  Maximum uniqueness, but gimmicky — contradicts Angelo's documented taste
  (facts over decoration).

## Design language: "an engineer's papers"

A personal site that reads like a well-set printed document from someone who
spent years around technical drawings — restrained, precise, typographic. No
boxes, no cards, no pills, no icons in content.

Signature elements:

1. **Title-block footer.** Every page ends with a bordered block modelled on a
   technical drawing's title block (Schriftfeld): a thin-ruled grid of small
   mono-labelled fields with real values —

   | NAME | LOCATION | BUILT |
   |---|---|---|
   | Angelo Fonseca | Mannheim, Germany | *build date* |
   | **CONTACT** contact@afonseca.me | **SOURCE** github.com/… | **PRIVACY** no scripts · no trackers · no analytics |

   The BUILT date is `{{ now.Format "2006-01-02" }}` — it changes on each deploy,
   an honest "last poured" stamp. This is the site's signature; no generated
   site has one, and for a Diplom-Ingenieur it is autobiographical rather than
   decorative.

2. **Double-rule under the header** — the thick/thin line pair of a drawing
   frame, rendered as two CSS borders. Quiet print cue, not skeuomorphism.

3. **Prose-first home page.** No hero, no cards. The name, one factual line
   (`Mechanical engineer working in data and AI. Mannheim, Germany.`), then
   three short first-person paragraphs (from `content/_index.md`, so copy is
   content-managed), a terse **Currently** list, the projects as a plain list
   with dates, and a closing contact sentence with inline text links. Nothing
   truncated, nothing "→ See all".

4. **Typography as identity.** IBM Plex Serif for prose and JetBrains Mono for
   labels (section kickers, meta lines, title-block field names, small
   uppercase with letter-spacing — the engineering-document register), both
   **self-hosted** as pinned WOFF2 files from `static/fonts/` so the site
   renders identically on every OS; the old system stack remains as fallback.
   No font CDN — same-origin requests only. Provenance, versions, and hashes:
   `docs/fonts.md`. Old-style numerals in prose
   (`font-variant-numeric: oldstyle-nums`).
   *(Amended 2026-07-04, same PR: the first draft kept the system stack, which
   rendered differently per OS — Iowan Old Style is macOS-only and not
   redistributable.)*

5. **Project pages get a data table.** Role / period / stack rendered as a
   small ruled meta table under the title — the drawing's parts list.

## What is kept from v0.2 (explicitly unchanged)

- Forest-green palette, light default, dark toggle (same hex values).
- Information architecture and nav: About · Projects · Home Lab.
- All content and URLs; `/uses/` and `/colophon/` forwarders; `/now/`,
  `/contact/`, `/reading/` reachable but unadvertised.
- Privacy rules (v0.1 spec) and posture: no JS beyond the theme toggle,
  no analytics, no embeds, no third-party requests.
- Hugo pin v0.161.1 and the quilombo/pantanal deploy path.

## Content changes

- `content/_index.md` (new): home-page prose. First person, concrete,
  no promotional framing.
- `content/about.md`: add one sentence that the data/AI work now sits in
  Daikin's internal RPA & AI team (public-safe; flag for Angelo's confirmation).
- New custom 404 page (closes repo issue #5): short, dry, one link home.
- Everything else untouched.

## Implementation

- **Remove** the PaperMod submodule (`themes/PaperMod`, `.gitmodules`) and the
  `theme` line plus PaperMod-specific params from `hugo.toml`.
- **New layouts** (all small):
  `layouts/_default/baseof.html` (skip-link, header/nav, main, title-block
  footer, theme-toggle script — keep the FOUC-guard inline script in `<head>`),
  `layouts/_default/single.html`, `layouts/_default/list.html` (projects),
  `layouts/index.html`, `layouts/404.html`,
  partials: `head.html` (meta, canonical, RSS, favicons, existing
  opengraph/schema/twitter partials), `header.html`, `footer.html`.
- **CSS:** one hand-written file `assets/css/main.css` via Hugo pipes
  (minify + fingerprint). Palette variables as in v0.2.
- RSS/sitemap/robots: Hugo built-ins.
- `README.md` + `CLAUDE.md`: drop PaperMod references (pin, gotcha section);
  note that layouts are now local.
- Quilombo role note: the role's build step must not assume a submodule exists
  (`git submodule update --init` on a repo with none is a no-op — verify on
  next deploy).

## Success criteria

1. `hugo --minify` (v0.161.1) builds clean with no theme present.
2. No PaperMod class names or assets in the output; view-source reads as
   hand-written.
3. Home page: no cards, no pills; prose renders from `content/_index.md`;
   projects listed with dates.
4. Title-block footer on every page with correct BUILT date; double-rule header.
5. Dark toggle works with no flash of wrong theme; palettes unchanged from v0.2.
6. All v0.2 URLs still resolve (uses/colophon forwarders included); RSS and
   sitemap still emitted; custom 404 present.
7. WCAG AA contrast maintained (same palette as v0.2, re-checked where mono
   labels use the muted colour).
8. No JavaScript beyond the theme toggle; no third-party requests.
