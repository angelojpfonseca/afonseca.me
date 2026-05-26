# afonseca.me — v0.1 redesign

**Date:** 2026-05-26
**Author:** Angelo Fonseca (with Claude)
**Status:** Approved design, ready for implementation planning.

## Goal

Take `afonseca.me` from a bare Hugo + PaperMod scaffold to a published personal hub at `https://afonseca.me`. The site should serve a mixed audience — potential consulting clients, recruiters, technical peers, future-self as a public record — without optimising for any one of them.

## Audience and tone

**Two-mode design.** The header surface (About, Projects, Writing, Contact) is the professional front door: clean, English-only, business-legible. The footer surface (Now, Uses, Reading, Colophon) is the personal-depth layer for anyone who clicks in: more personal voice, Portuguese phrases allowed where natural, room for reflection. Both modes share the same visual language; the split is editorial, not structural.

## Content & privacy constraints

These rules apply to every page on the public site and to any draft generated from LinkedIn or other sources.

### Topics held back

1. **No mention of fatherhood, pregnancy, or related family-planning topics.** Off-limits for v0.1 and until explicitly lifted by Angelo.
2. **No partner, household member, or family details.** Generalises the rule above — no names, no references to people sharing the home.
3. **No exact address.** City-level is fine ("Mannheim, Germany"); street, neighbourhood, or postcode is not.
4. **No birthdate or age.**
5. **No phone number, even hand-wavy** ("DM me on …" is fine; a number is not).
6. **Now page = themes only.** Current focus, what's being learned, what's being thought about — never schedule, travel plans, or real-time location ("learning X" yes; "in Lisbon this week" no).
7. **No internal infrastructure names, fleet project codenames, or homelab machine codenames** on any public surface. Use neutral role descriptions ("homelab VPS", "personal knowledge vault", "fleet handbook") when context demands them.

### Style and language

8. **Header pages are English-only.** Portuguese is permitted on About body paragraphs, Now, Uses, Reading, Colophon — never forced, only where natural.

### Decided content choices

9. **Headshot:** professional headshot on About page. Angelo provides the image.
10. **Past employers:** named, consistent with LinkedIn. Abstraction reserved for case studies under NDA.

## Security and privacy posture

### Already in scope (v0.1)

- **HTTPS** via GitHub Pages — certificate issued and renewed automatically.
- **No third-party scripts, trackers, or analytics.** Browser fingerprinting protection is implicit — there are no scripts to do it from.
- **No comments, no newsletter, no user-data collection.**
- **No embeds** (no YouTube, Twitter, Vimeo embeds — those phone home). Link out instead.
- **Contact via domain alias only.** A `contact@afonseca.me` IONOS email-forwarding alias points to Angelo's Gmail. The site exposes only the alias as a plain `<a href="mailto:contact@afonseca.me">` — no obfuscation theatre, no contact form, no backend. If harvested, the alias can be rotated.
- **No phone number anywhere on the site.**
- **WHOIS privacy:** before DNS cutover, verify `whois afonseca.me` shows redacted registrant. If exposed, enable IONOS privacy.
- **Git commit author email:** global `git config user.email` is set to `ajpfonseca@proton.me` — a ProtonMail address Angelo is content to have publicly visible in commit metadata. Historical commits on this repo still record `angelojpfonseca@gmail.com`; that leak is accepted and not retroactively rewritten. GitHub → Settings → Emails → "Keep my email addresses private" is enabled to block accidental Gmail-tagged commits going forward.

### Deferred (tracked as repo issues)

- Content Security Policy via `<meta http-equiv>` tag (modest hardening; GH Pages cannot set HTTP headers).
- DNSSEC at IONOS (low ROI for a static site, but worth evaluating).
- Broken-link / link-rot check (`lychee` in CI, quarterly cadence).

### Explicitly rejected

- HSTS preload submission (one-way trip; no upside for a personal site).
- Contact form with a backend (dependency for nothing).
- Email obfuscation tricks (HTML entities, JS rewriting) — defeated by modern scrapers; the real defence is a rotatable alias.

### Spec visibility

**Accepted risk:** this spec lives in a public repo and discloses what is withheld from the site (fatherhood, fleet codenames, etc.). The disclosure itself reads as reasonable privacy hygiene. Moving design docs into a private repo creates a fleet-context split that costs more than it saves.

## Information architecture

### URL map

| URL | Page | Type | v0.1 |
|---|---|---|---|
| `/` | Home | single | ✓ |
| `/about/` | About | single | ✓ |
| `/projects/` | Projects index | list | ✓ |
| `/projects/<slug>/` | Project case study | leaf | ✓ (3 cards) |
| `/writing/` | Writing index | list | **404 until ≥1 post; not in header** |
| `/writing/<slug>/` | Post | leaf | — |
| `/contact/` | Contact | single | ✓ |
| `/now/` | Now | single | ✓ |
| `/uses/` | Uses | single | ✓ |
| `/reading/` | Reading | single | ✓ |
| `/colophon/` | Colophon | single | ✓ |
| `/cv.pdf` | CV | static asset | ✓ |

### Navigation

- **Header:** About · Projects · Writing · Contact
- **Footer:** Now · Uses · Reading · Colophon · GitHub · LinkedIn

Writing remains hidden from the header until the first real post is published, to keep the front door honest.

### Content types (Hugo archetypes)

- **`project`** — `title`, `date`, `summary`, `role`, `period`, `stack[]`, `links[]`, `featured: bool`. Body = case study.
- **`post`** (writing) — `title`, `date`, `summary`, `tags[]`, `draft`. Body = markdown.
- **Singletons** (About, Now, Uses, Reading, Contact, Colophon) — plain `page` archetype, `title` + `lastmod` only.

## Visual direction

- **Theme:** PaperMod, customised via `assets/css/extended/custom.css` and `layouts/partials/` overrides. No fork; submodule stays on upstream.
- **Typography:** system serif stack for body and headings (Iowan / Charter / IBM Plex Serif). Monospace for code (PaperMod default). No web fonts in v0.1.
- **Colour:** dark default, neutral palette, single muted accent (earth-tone or muted green — finalised during implementation). Light mode auto-available via PaperMod.
- **Layout:** centred single column, max content width ~720px. Generous line-height. Only the home page uses a multi-card layout; everything else is reading-width.
- **No JS beyond PaperMod defaults** (theme switcher). No animations. Fast, static, archival-feeling.
- **Cultural / personal signals** live in body copy and the Colophon page, never in the header or chrome.
- **Favicon:** simple `AF` monogram, shipped as `favicon.ico` + `favicon.svg`. No `apple-touch-icon`, no web manifest — kept minimal in v0.1. OG image generation deferred.

## Implementation conventions

### `hugo.toml` baseline

The existing `hugo.toml` is partially aligned with this spec but contains drift to fix during infra step 1.

- `baseURL = "https://afonseca.me"` — keep.
- `languageCode = "en-us"` — change to `"en"`; site is English without US-specific locale.
- `title = "Angelo Fonseca"` — keep.
- `theme = "PaperMod"` — keep.
- `defaultContentLanguage = "en"` — add.
- `enableRobotsTXT = true` — add. Hugo will generate `robots.txt` allow-all.
- `[params].defaultTheme = "dark"` — keep.
- `[params].ShowReadingTime`, `ShowShareButtons`, `ShowPostNavLinks`, etc. — explicit `false` for v0.1; revisit when Writing turns on.
- `[params].homeInfoParams` — **remove.** The home page uses a custom template (see "Home page data source" below), not PaperMod's built-in home info partial.
- `[menu.main]` — rewrite to header `About · Projects · Writing · Contact`; **Writing entry is commented out in v0.1** and uncommented when the first post lands.

### PaperMod version pinning

The PaperMod submodule must be pinned to a specific release tag (latest stable at implementation time) — not tracking `master`. Document the pinned tag in the repo `README.md`. Manual upgrade after testing, never an automatic update. This matches Angelo's general "pin versions after testing" rule.

### Home page data source

- **Hero block:** static copy, edited in the custom `layouts/index.html` partial. Brief, name + one-line identity.
- **Featured projects card(s):** Hugo iterates `where .Site.RegularPages "Section" "projects"` filtered by `Params.featured: true`. v0.1: 3 projects marked `featured: true` in their front matter.
- **Now card:** pulls the first paragraph of `/now/` via `Site.GetPage` + truncation. Single source of truth for current focus.
- **About link card:** static.

### Accessibility commitments

These are minimums for v0.1, not aspirations.

- Semantic HTML — preserve PaperMod's structure; no `<div>` soup in overrides.
- `<html lang="en">` set unequivocally.
- Visible keyboard focus styles — never remove the default outline in `extended/custom.css` without an equivalent replacement.
- Colour contrast on body text, links, and the muted accent meets WCAG 2.1 AA (4.5:1 for body, 3:1 for large text). Verify the chosen accent against both dark and light backgrounds before merging the CSS.
- Required `alt` text on the headshot and any project image. Decorative images use `alt=""`.
- Page titles unique per page; first `<h1>` matches the page title.

### Locale and dates

- Site language: English (`lang="en"`).
- Date format on all rendered pages: **ISO 8601 (`2026-05-26`)**. Set via `dateFormat` in `hugo.toml` (e.g., `dateFormat = "2006-01-02"` in PaperMod params).
- Author timezone is Europe/Berlin but timestamps on the site are date-only; timezone-irrelevant for v0.1.

### Sitemap, robots, feeds

- **Sitemap:** Hugo default sitemap at `/sitemap.xml`, includes all v0.1 pages. No `sitemap = false` overrides on any page.
- **`robots.txt`:** auto-generated by Hugo (`enableRobotsTXT = true`), allow-all.
- **RSS feed at `/writing/index.xml`:** Hugo generates this by default at every section. **In v0.1, while no posts exist, the feed will be empty but the URL will resolve.** Before merging step 2 of implementation, verify with a `draft: true` test post that drafts do NOT appear in the feed (Hugo's default behaviour, but confirm). This closes the RSS-leak risk in v0.1 rather than deferring it.

### Build behaviour

- `hugo --minify` for production builds.
- `buildDrafts = false`, `buildFuture = false` in `hugo.toml`. Archetypes default front matter to `draft: true`.

## Deployment

- **Host:** GitHub Pages, served from `angelojpfonseca/afonseca.me` via GitHub Actions.
- **Build:** `.github/workflows/deploy.yml` checks out (with submodules), installs a **pinned** Hugo version, runs `hugo --minify`, deploys via `actions/deploy-pages`.
- **Domain:** `afonseca.me`. DNS managed at **IONOS**. Apex `A` records to `185.199.108.153/.109/.110/.111`; `www` `CNAME` to `angelojpfonseca.github.io`. `CNAME` file at repo root holds `afonseca.me`. GitHub Pages issues and renews TLS automatically.
- **Self-hosting on private infrastructure was considered and rejected** for v0.1: no privacy gain (public content) and unnecessary ops surface.
- **No analytics, no comments, no third-party scripts** in v0.1.

## v0.1 scope and content sources

| Page | Source | Drafting approach |
|---|---|---|
| Home | — | Minimal hero + 3 cards; templated. |
| About | LinkedIn (paste / PDF export — site does not scrape) + Angelo's edits | Claude drafts a short minimal version; Angelo refines later. |
| Projects (3 cards) | LinkedIn for pre-fleet engineering / data work; current work described in **neutral, codename-free language** | Claude drafts; Angelo edits. |
| Contact | `contact@afonseca.me` (IONOS forwarding alias), LinkedIn, GitHub | Trivial. Alias must be set up at IONOS before DNS cutover. |
| Now | One short paragraph, current focus. **Must comply with content constraints — no fatherhood mention.** | Claude drafts minimal; Angelo updates later. |
| Uses | Daily-driver stack | Claude drafts from observable tooling; Angelo corrects. |
| Reading | Single-page list | Angelo supplies 3-5 items; Claude formats. |
| Colophon | How the site is built | Claude writes. |
| `/cv.pdf` | LinkedIn PDF export | Angelo exports from LinkedIn; committed to `static/cv.pdf`. Swap to markdown-generated PDF is a deferred upgrade. |

**Content philosophy:** ship minimal honest drafts now; Angelo refines later. Getting the site live takes precedence over perfect copy. No placeholders, no "Coming soon" — every shipped page has real, if brief, content.

### Success criteria for v0.1

1. Builds clean on push to `main`; deploys via GitHub Actions to GitHub Pages.
2. `https://afonseca.me` resolves with valid TLS, no mixed-content warnings.
3. Every page in the v0.1 column above renders with real content.
4. Mobile-readable (verified on a real device or DevTools emulation).
5. Lighthouse ≥ 90 on Performance and Accessibility.
6. RSS feed exists at `/writing/index.xml` (empty in v0.1 until first post; enables cleanly when post 1 ships). Draft-leak suppression verified before merge (see Implementation conventions → Sitemap, robots, feeds).

## Implementation order

0. **Prerequisites (Angelo, before/during infra step):**
   - ✅ Global `git config user.email` set to `ajpfonseca@proton.me`.
   - Enable "Keep my email addresses private" in GitHub settings (if not already).
   - Set up `contact@afonseca.me` IONOS forwarding alias → primary inbox.
   - Verify `whois afonseca.me` privacy redaction at IONOS.
1. **Infra:** GitHub Actions workflow, `CNAME` file, custom CSS scaffold, archetype files. Site builds and deploys to a placeholder, with DNS not yet cut over.
2. **Content scaffolding:** all singleton pages and archetypes exist with minimal real content (no placeholders).
3. **Home page template:** custom `index.html` partial in `layouts/`, with the hero + cards.
4. **Content pass:** Angelo provides LinkedIn material, headshot, and reading items; Claude drafts; Angelo edits where needed. Iterate until ship-ready.
5. **Polish + Lighthouse check + DNS cutover** at IONOS. TLS verification post-cutover.

## Deferred work (tracked as GitHub issues on the repo)

- Enable Writing header link after first post.
- Evaluate privacy-respecting analytics (self-hosted vs. SaaS vs. none).
- OG image generator for posts and project pages.
- Enable PaperMod search once content justifies it.
- Custom 404 page.
- Internationalisation (Portuguese / German) if needed later.
- Per-entry Reading log (migrate from single-page if it starts feeling like a stream).
- Featured-image support for project cards.
- Content Security Policy via `<meta http-equiv>` tag.
- DNSSEC at IONOS — evaluate cost/benefit.
- Broken-link / link-rot check (`lychee` in CI, quarterly cadence).

## Out of scope

- A blog post backlog. Writing section infrastructure ships, but no posts are required for v0.1.
- A custom theme. PaperMod with overrides is the v0.1 ceiling.
- Comments, newsletter signup, mailing list, or any user-data collection.
- Multi-language content. Site is English; isolated Portuguese phrases in body copy are allowed under the tone rules.
