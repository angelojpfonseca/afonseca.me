# afonseca.me v0.2 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign afonseca.me with a forest-green light-mode palette, 960px layout, simplified navigation, and a compact above-the-fold home page.

**Architecture:** All changes are in Hugo templates, CSS, content files, and config — no new dependencies. The home page gets a custom `layouts/index.html`; inner pages use PaperMod defaults with a narrowed `.post-content` rule. CSS is a complete rewrite of `assets/css/extended/custom.css`.

**Tech Stack:** Hugo v0.161.1, PaperMod v8.0, plain CSS, no JS beyond PaperMod's theme toggle.

**Spec:** `docs/superpowers/specs/2026-06-02-afonseca-me-v0.2-redesign.md`

**Verify builds with:** `hugo --minify 2>&1` — must exit 0 with no ERRORs. For visual check: `hugo server`.

---

## File map

| File | Action | Purpose |
|---|---|---|
| `assets/css/extended/custom.css` | Rewrite | Forest-green palette, new home styles, remove dead CSS |
| `hugo.toml` | Modify | defaultTheme → light, menu rewrite |
| `layouts/index.html` | Rewrite | Hero + social pills + 3-card grid |
| `layouts/partials/footer.html` | Rewrite | Copyright only |
| `content/homelab.md` | Create | New Home Lab page (Tools + Colophon) |
| `content/uses.md` | Modify | Forwarding stub to /homelab/ |
| `content/colophon.md` | Modify | Forwarding stub to /homelab/ |
| `content/about.md` | Modify | Add Reading link |
| `content/projects/homelab.md` | Modify | Rename title, remove featured |
| `content/projects/daikin-ai-rag.md` | Modify | Remove featured |
| `content/projects/personal-website.md` | Modify | Remove featured |

---

## Task 1: Rewrite CSS

**Files:**
- Rewrite: `assets/css/extended/custom.css`

- [ ] **Step 1: Replace the entire file**

```css
/* assets/css/extended/custom.css
   PaperMod loads any file under assets/css/extended/ automatically.
   Keep additions here; do not edit PaperMod's own CSS files.
*/

/* ── Palette: Forest Green ──────────────────────────────── */

:root {
  --theme:   #f5f6f2;
  --entry:   #ffffff;
  --primary: #181c17;
  --secondary: #525a50;
  --content: #181c17;
  --border:  #d5d9cd;
  --afm-accent: #2d5a3d;
  --code-bg: #f0f0ec;
}

.dark {
  --theme:   #141a14;
  --entry:   #1e261e;
  --primary: #e2e8e2;
  --secondary: #8aaa8a;
  --content: #e2e8e2;
  --border:  #2d3d2d;
  --afm-accent: #4ea86a;
  --code-bg: #1a221a;
}

/* ── Typography ──────────────────────────────────────────── */

body,
.post-content,
.post-title,
.first-entry .entry-header h1,
.entry-header h2 {
  font-family: "Iowan Old Style", "Charter", "IBM Plex Serif",
               Georgia, "Times New Roman", serif;
}

code, pre, kbd, samp {
  font-family: "JetBrains Mono", "SF Mono", Menlo, Consolas, monospace;
}

/* ── Links and focus ─────────────────────────────────────── */

a {
  color: var(--afm-accent);
}

a:hover,
a:focus-visible {
  text-decoration: underline;
}

:focus-visible {
  outline: 2px solid var(--afm-accent);
  outline-offset: 2px;
}

/* ── Layout ──────────────────────────────────────────────── */

/* Widen the main container globally (was 720px in v0.1) */
.main {
  max-width: 960px;
}

/* Narrow prose content on inner pages (PaperMod single/list) */
.post-content {
  max-width: 680px;
}

/* ── Home hero ───────────────────────────────────────────── */

.afm-hero {
  padding: 3.5rem 0 2.5rem;
  border-bottom: 1px solid var(--border);
  margin-bottom: 2rem;
}

.afm-hero__name {
  font-size: 2.1rem;
  font-weight: 600;
  letter-spacing: -0.02em;
  margin: 0 0 0.3rem;
  color: var(--primary);
}

.afm-hero__label {
  font-size: 1rem;
  color: var(--secondary);
  margin: 0 0 1rem;
}

.afm-hero__label a {
  color: var(--afm-accent);
  text-decoration: none;
}

.afm-hero__label a:hover,
.afm-hero__label a:focus-visible {
  text-decoration: underline;
}

/* ── Social pills ────────────────────────────────────────── */

.afm-social-pills {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  flex-wrap: wrap;
}

.afm-social-pill {
  display: inline-flex;
  align-items: center;
  gap: 0.35rem;
  color: var(--secondary);
  text-decoration: none;
  font-size: 0.88rem;
  padding: 0.3rem 0.65rem;
  border: 1px solid var(--border);
  border-radius: 4px;
  background: var(--entry);
}

.afm-social-pill:hover,
.afm-social-pill:focus-visible {
  color: var(--afm-accent);
  border-color: var(--afm-accent);
  text-decoration: none;
}

.afm-social-pill svg {
  width: 14px;
  height: 14px;
  fill: currentColor;
  flex-shrink: 0;
}

/* ── Home cards ──────────────────────────────────────────── */

.afm-cards {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.afm-card {
  background: var(--entry);
  border: 1px solid var(--border);
  border-radius: 7px;
  padding: 1.4rem 1.5rem;
}

.afm-card--full {
  grid-column: 1 / -1;
}

.afm-card h2 {
  font-size: 1.05rem;
  font-weight: 600;
  color: var(--afm-accent);
  margin: 0 0 0.55rem;
  letter-spacing: -0.01em;
}

.afm-card p {
  font-size: 0.92rem;
  color: var(--secondary);
  line-height: 1.65;
  margin: 0 0 0.75rem;
}

.afm-card p em {
  font-style: italic;
}

.afm-card__link {
  font-size: 0.85rem;
  color: var(--afm-accent);
  text-decoration: none;
}

.afm-card__link:hover,
.afm-card__link:focus-visible {
  text-decoration: underline;
}

/* ── Footer ──────────────────────────────────────────────── */

.footer {
  border-top: 1px solid var(--border);
  margin-top: 4rem;
  padding: 1.2rem 0;
  text-align: center;
  font-size: 0.82rem;
  color: var(--secondary);
}
```

- [ ] **Step 2: Verify the build is clean**

```bash
hugo --minify 2>&1 | grep -E "^(ERROR|WARN)" | head -20
```

Expected: no output (no errors or warnings).

- [ ] **Step 3: Commit**

```bash
git add assets/css/extended/custom.css
git commit -m "style: rewrite custom.css with forest-green palette v0.2"
```

---

## Task 2: Update hugo.toml

**Files:**
- Modify: `hugo.toml`

- [ ] **Step 1: Change defaultTheme and rewrite menu**

Open `hugo.toml`. Make these two changes:

Change line `defaultTheme = "dark"` to:
```toml
defaultTheme = "light"
```

Replace the entire `[menu]` block with:
```toml
[menu]
  [[menu.main]]
    name = "About"
    url = "/about/"
    weight = 1
  [[menu.main]]
    name = "Projects"
    url = "/projects/"
    weight = 2
  [[menu.main]]
    name = "Home Lab"
    url = "/homelab/"
    weight = 3
```

- [ ] **Step 2: Verify**

```bash
hugo --minify 2>&1 | grep -E "^(ERROR|WARN)" | head -20
```

Expected: no output.

- [ ] **Step 3: Commit**

```bash
git add hugo.toml
git commit -m "config: light default theme, new nav About/Projects/Home Lab"
```

---

## Task 3: Rewrite home page template

**Files:**
- Rewrite: `layouts/index.html`

- [ ] **Step 1: Replace the entire file**

```html
{{- define "main" }}

{{- /* Collect social URLs from site config */ -}}
{{- $github := "" }}
{{- $linkedin := "" }}
{{- $email := "" }}
{{- range .Site.Params.socialIcons }}
  {{- if eq .name "github" }}{{- $github = .url }}{{- end }}
  {{- if eq .name "linkedin" }}{{- $linkedin = .url }}{{- end }}
  {{- if eq .name "email" }}{{- $email = .url }}{{- end }}
{{- end }}

<section class="afm-hero">
  <h1 class="afm-hero__name">{{ .Site.Title }}</h1>
  <p class="afm-hero__label">
    Mechanical engineer. Mannheim, Germany.&thinsp;&middot;&thinsp;<a href="/cv.pdf">CV (PDF) &#x2197;</a>
  </p>
  <div class="afm-social-pills">
    {{- if $github }}
    <a class="afm-social-pill" href="{{ $github }}" rel="noopener noreferrer">
      <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z"/></svg>
      GitHub
    </a>
    {{- end }}
    {{- if $linkedin }}
    <a class="afm-social-pill" href="{{ $linkedin }}" rel="noopener noreferrer">
      <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
      LinkedIn
    </a>
    {{- end }}
    {{- if $email }}
    <a class="afm-social-pill" href="{{ $email }}">
      <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M20 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
      contact@afonseca.me
    </a>
    {{- end }}
  </div>
</section>

<div class="afm-cards">

  {{- with .Site.GetPage "/about" }}
  <article class="afm-card afm-card--full">
    <h2><a href="{{ .RelPermalink }}" class="afm-card__link">About</a></h2>
    <p>{{ .Plain | truncate 320 "…" }}</p>
    <a class="afm-card__link" href="{{ .RelPermalink }}">Full about page &#x2192;</a>
  </article>
  {{- end }}

  <article class="afm-card">
    <h2><a href="/projects/" class="afm-card__link">Projects</a></h2>
    <p>A small set of case studies &#x2014; work I have built, technical problems I have spent real time on.</p>
    <a class="afm-card__link" href="/projects/">See all projects &#x2192;</a>
  </article>

  <article class="afm-card">
    <h2><a href="/homelab/" class="afm-card__link">Home Lab</a></h2>
    <p>Tools and setup I use day&#x2011;to&#x2011;day. Notes on how this site and the infrastructure behind it are built.</p>
    <a class="afm-card__link" href="/homelab/">Tools, colophon &#x2192;</a>
  </article>

</div>

{{- end }}
```

- [ ] **Step 2: Verify**

```bash
hugo --minify 2>&1 | grep -E "^(ERROR|WARN)" | head -20
```

Expected: no output.

- [ ] **Step 3: Check the About card pulls real text**

```bash
hugo --minify && grep -A3 'afm-card--full' public/index.html | head -10
```

Expected: the About card HTML contains a non-empty text excerpt (not just the template tags).

- [ ] **Step 4: Commit**

```bash
git add layouts/index.html
git commit -m "feat: rewrite home page — hero, social pills, 3-card grid"
```

---

## Task 4: Strip footer to copyright only

**Files:**
- Rewrite: `layouts/partials/footer.html`

- [ ] **Step 1: Replace the entire file**

```html
{{- if not (.Param "hideFooter") }}
<footer class="footer">
  <span>&copy; {{ now.Format "2006" }} {{ .Site.Params.author | default .Site.Title }}</span>
</footer>
{{- end }}
```

- [ ] **Step 2: Verify**

```bash
hugo --minify 2>&1 | grep -E "^(ERROR|WARN)" | head -20
```

Expected: no output.

- [ ] **Step 3: Commit**

```bash
git add layouts/partials/footer.html
git commit -m "feat: strip footer to copyright only"
```

---

## Task 5: Create Home Lab page

**Files:**
- Create: `content/homelab.md`

- [ ] **Step 1: Create the file**

```markdown
---
title: "Home Lab"
lastmod: 2026-06-02
draft: false
---

## Tools

What I work with day to day. Not affiliate links, not endorsements — just what I happen to use.

### Machine and OS

- A Linux laptop as the primary working machine.
- A Windows machine for software that does not yet have a Linux equivalent in my workflow.

### Tools

- **Editor:** Visual Studio Code with vim keybindings.
- **Terminal:** the default that ships with the OS, plus `tmux` for sessions.
- **Languages I am actively using:** Python for data work, a steady amount of Bash, growing Go familiarity.
- **AI assistance:** Claude Code in the terminal, mostly for thinking out loud and for repetitive scaffolding work.

### Services

- **Git hosting:** GitHub and Codeberg.
- **Email:** ProtonMail for personal mail; a domain-aliased forwarder for the contact address on this site.
- **Password management:** an open-source password manager, self-hostable.

This list is intentionally shallow. I update it when something earns a place I expect to keep.

---

## How this site is built

This site is built with [Hugo](https://gohugo.io) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme, with light CSS overrides. Source is on [GitHub](https://github.com/angelojpfonseca/afonseca.me).

- **Typography:** system serif stack (Iowan Old Style / Charter / IBM Plex Serif), JetBrains Mono for code.
- **No JavaScript** beyond the theme's own theme-switcher.
- **No analytics, no trackers, no embeds**, no third-party requests at runtime.
- **Hosted on a personal VPS** behind Caddy, served over HTTPS with a Let's Encrypt certificate. Builds are pushed to the server via Ansible.

Design and implementation specs live under `docs/superpowers/` in the repo.
```

- [ ] **Step 2: Verify Hugo can find and build the new page**

```bash
hugo --minify 2>&1 | grep -E "^(ERROR|WARN)" | head -20
```

Expected: no output.

```bash
ls public/homelab/index.html
```

Expected: file exists.

- [ ] **Step 3: Commit**

```bash
git add content/homelab.md
git commit -m "feat: add Home Lab page (tools + colophon)"
```

---

## Task 6: Update uses.md and colophon.md as forwarding stubs

**Files:**
- Modify: `content/uses.md`
- Modify: `content/colophon.md`

- [ ] **Step 1: Replace uses.md**

```markdown
---
title: "Uses"
lastmod: 2026-06-02
draft: false
---

This page has moved. See [Home Lab](/homelab/) for tools and daily setup.
```

- [ ] **Step 2: Replace colophon.md**

```markdown
---
title: "Colophon"
lastmod: 2026-06-02
draft: false
---

This page has moved. See [Home Lab](/homelab/) for how this site is built.
```

- [ ] **Step 3: Verify both URLs still resolve**

```bash
hugo --minify 2>&1 | grep -E "^(ERROR|WARN)" | head -20
ls public/uses/index.html public/colophon/index.html
```

Expected: no build errors, both files exist.

- [ ] **Step 4: Commit**

```bash
git add content/uses.md content/colophon.md
git commit -m "feat: redirect uses and colophon to /homelab/"
```

---

## Task 7: Update about.md — add Reading link

**Files:**
- Modify: `content/about.md`

- [ ] **Step 1: Append Reading link before the final contact line**

The current `about.md` ends with:

```markdown
For the formal arc with dates and details, the [CV is here as a PDF](/cv.pdf).

To talk — about a project, a role, a question — reach me at [contact@afonseca.me](mailto:contact@afonseca.me).
```

Add a Reading link line between those two:

```markdown
For the formal arc with dates and details, the [CV is here as a PDF](/cv.pdf).

What I am reading: [Reading list](/reading/).

To talk — about a project, a role, a question — reach me at [contact@afonseca.me](mailto:contact@afonseca.me).
```

- [ ] **Step 2: Verify**

```bash
hugo --minify 2>&1 | grep -E "^(ERROR|WARN)" | head -20
```

Expected: no output.

- [ ] **Step 3: Commit**

```bash
git add content/about.md
git commit -m "content: add reading list link to about page"
```

---

## Task 8: Project front matter cleanup

**Files:**
- Modify: `content/projects/homelab.md`
- Modify: `content/projects/daikin-ai-rag.md`
- Modify: `content/projects/personal-website.md`

- [ ] **Step 1: Update homelab.md front matter**

Change `title` and remove `featured`. The body text of the file stays unchanged — only the front matter block changes:

```yaml
---
title: "Personal lab and side experiments"
date: 2026-05-26
draft: false
summary: "A small home lab for virtualisation and AI experiments, plus a handful of hobby projects in 3D printing, electronics, and robotics."
role: "Builder"
period: "ongoing"
stack: ["Linux", "Hypervisors", "Microcontrollers", "3D printing", "LLMs", "Multi-agent frameworks (Autogen, CrewAI)"]
links: []
---
```

(Remove the `featured: true` line. Everything after the closing `---` stays as-is.)

- [ ] **Step 2: Remove featured from daikin-ai-rag.md**

Open `content/projects/daikin-ai-rag.md`. Delete the line `featured: true` from the front matter. All other front matter stays unchanged.

- [ ] **Step 3: Remove featured from personal-website.md**

Open `content/projects/personal-website.md`. Delete the line `featured: true` from the front matter. All other front matter stays unchanged.

- [ ] **Step 4: Verify**

```bash
hugo --minify 2>&1 | grep -E "^(ERROR|WARN)" | head -20
```

Expected: no output.

- [ ] **Step 5: Commit**

```bash
git add content/projects/homelab.md content/projects/daikin-ai-rag.md content/projects/personal-website.md
git commit -m "chore: rename homelab project, remove featured front matter"
```

---

## Task 9: Final verification

- [ ] **Step 1: Full clean build**

```bash
rm -rf public && hugo --minify 2>&1
```

Expected: exits 0, output ends with `Total in ... ms`, no lines beginning with `ERROR`.

- [ ] **Step 2: Verify WCAG AA contrast for both accent colours**

Light mode — `#2d5a3d` on `#f5f6f2` (background) and `#ffffff` (card surface):
- Relative luminance of `#2d5a3d` ≈ 0.078
- Relative luminance of `#f5f6f2` ≈ 0.924 → ratio ≈ **7.6:1** ✓
- Relative luminance of `#ffffff` = 1.0 → ratio ≈ **8.2:1** ✓

Dark mode — `#4ea86a` on `#141a14` (background):
- Relative luminance of `#4ea86a` ≈ 0.374
- Relative luminance of `#141a14` ≈ 0.006 → ratio ≈ **7.5:1** ✓

All three pairs pass WCAG AA (4.5:1 minimum). If you use a different tool to verify (e.g. https://webaim.org/resources/contrastchecker/), check all three pairs pass before merging.

- [ ] **Step 3: Check all success criteria**

Run each check:

```bash
# SC4: nav contains only About, Projects, Home Lab
grep -o 'href="/about/"\|href="/projects/"\|href="/homelab/"\|href="/contact/"' public/index.html

# SC5: footer has no nav links
grep -A5 '<footer' public/index.html | grep '<a href'

# SC6: social pills present
grep -c 'afm-social-pill' public/index.html

# SC7: no broken internal links to /uses/ or /colophon/ from nav
grep 'href="/uses/"\|href="/colophon/"' public/index.html

# SC9: /uses/ and /colophon/ still exist
ls public/uses/index.html public/colophon/index.html

# SC10: featured: true gone from all projects
grep -r 'featured: true' content/projects/
```

Expected results:
- SC4: shows About, Projects, Home Lab hrefs — no `/contact/`
- SC5: no `<a href` lines (footer has no links)
- SC6: output is `3`
- SC7: no output
- SC9: both files exist
- SC10: no output (no featured flags remaining)

- [ ] **Step 4: Visual check — start dev server**

```bash
hugo server
```

Open http://localhost:1313 and verify:
- Home page hero shows name + label + 3 social pills
- Three cards visible: About (full width), Projects (half), Home Lab (half)
- All fits above the fold without scrolling (on a 1366×768 or larger screen)
- Card headings (About, Projects, Home Lab) are visually prominent in green
- Nav shows: Angelo Fonseca | About · Projects · Home Lab
- Footer shows only: © 2026 Angelo Fonseca
- Switch to dark mode with the toggle — page stays readable, accent shifts to lighter green
- Visit /uses/ and /colophon/ — both show forwarding note with link to /homelab/
- Visit /homelab/ — shows both Tools section and "How this site is built" section
- Visit /about/ — includes "Reading list →" link

- [ ] **Step 5: Final commit if any last fixes were needed, then push**

```bash
git log --oneline -8
```

All tasks should appear as separate commits. Push when ready:

```bash
git push
```

Then deploy via quilombo:
```bash
ansible-playbook -i inventory/hosts.yml playbooks/pantanal-hugo.yml --ask-become-pass
```
(Run from the quilombo control node, not from this repo.)
