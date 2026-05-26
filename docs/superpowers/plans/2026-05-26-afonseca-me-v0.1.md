# afonseca.me v0.1 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Take `afonseca.me` from a bare Hugo + PaperMod scaffold to a published personal hub at `https://afonseca.me`, implementing the v0.1 design at `docs/superpowers/specs/2026-05-26-afonseca-me-redesign-design.md`.

**Architecture:** Static Hugo site, PaperMod theme (pinned via git submodule), custom CSS through PaperMod's `assets/css/extended/` hook, custom `layouts/index.html` for the home page hub. Deployed via GitHub Actions to GitHub Pages, served at the custom domain with TLS from GH Pages. No JavaScript beyond PaperMod defaults, no analytics, no third-party requests.

**Tech Stack:** Hugo (extended) v0.161.1 (pinned), PaperMod theme v8.0 (pinned), GitHub Actions, GitHub Pages, IONOS DNS, ProtonMail forwarding alias for contact.

**Repo:** `angelojpfonseca/afonseca.me` (GitHub, public).
**Working directory:** `~/src/github.com/angelojpfonseca/afonseca.me/`.

---

## Phase 0: Prerequisites (Angelo's manual actions)

These are account / external-system actions Angelo performs once. They are **not git-tracked tasks** — execute them before the related implementation task.

- [ ] **Git commit identity:** confirm `git config --global user.email` returns `ajpfonseca@proton.me` (already done as of plan creation).
- [ ] **GitHub Pages source:** in repo Settings → Pages, set source to "GitHub Actions" (not "Deploy from a branch"). Required before Task 5 deploy runs.
- [ ] **GitHub Settings → Emails:** check "Keep my email addresses private" is enabled.
- [ ] **IONOS contact alias:** create `contact@afonseca.me` as a forwarding alias to Angelo's primary inbox.
- [ ] **IONOS WHOIS privacy:** verify `whois afonseca.me` shows redacted registrant; enable privacy if not.
- [ ] **Headshot:** Angelo provides a professional headshot (≥800px, ≤500KB after compression, JPEG or WebP) for Task 16.
- [ ] **LinkedIn export PDF:** Angelo downloads CV PDF from LinkedIn (Profile → "More" → "Save to PDF") for Task 15 and the About / Projects content passes.

---

## Phase 1: Infrastructure (5 tasks)

### Task 1: Pin PaperMod submodule to v8.0

**Files:**
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/.gitmodules` (no change to content; submodule itself moves)
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/themes/PaperMod` (submodule HEAD)
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/README.md`

- [ ] **Step 1: Check current submodule state**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git submodule status
```

Expected: shows `themes/PaperMod` at some commit (likely `master` HEAD as of clone time).

- [ ] **Step 2: Check out v8.0 inside the submodule**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me/themes/PaperMod
git fetch --tags
git checkout v8.0
cd ../..
git submodule status
```

Expected: now shows `themes/PaperMod` at the v8.0 tag commit, prefixed with `+` (modified from main repo's recorded SHA — that's expected for a fresh pin).

- [ ] **Step 3: Stage submodule pin and update README**

Replace the entire content of `~/src/github.com/angelojpfonseca/afonseca.me/README.md` with:

```markdown
# afonseca.me

Personal website source. Built with Hugo and the PaperMod theme.

- Hugo: pinned to `v0.161.1` in `.github/workflows/deploy.yml`.
- PaperMod: pinned to `v8.0` (git submodule). Manual upgrade after testing — never automatic.
- Design spec: `docs/superpowers/specs/2026-05-26-afonseca-me-redesign-design.md`.
- Implementation plan: `docs/superpowers/plans/2026-05-26-afonseca-me-v0.1.md`.

Live site: <https://afonseca.me>.
```

- [ ] **Step 4: Commit the pin and README**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add themes/PaperMod README.md
git commit -m "build: pin PaperMod theme to v8.0; refresh README"
```

Expected: commit succeeds; `git log --oneline -1` shows the new commit on `main`.

---

### Task 2: Rewrite `hugo.toml` per spec baseline

**Files:**
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/hugo.toml`

- [ ] **Step 1: Replace the file with the spec-aligned baseline**

Overwrite `~/src/github.com/angelojpfonseca/afonseca.me/hugo.toml` with exactly this content:

```toml
baseURL = "https://afonseca.me"
languageCode = "en"
defaultContentLanguage = "en"
title = "Angelo Fonseca"
theme = "PaperMod"
enableRobotsTXT = true
buildDrafts = false
buildFuture = false

[params]
  author = "Angelo Fonseca"
  description = "Mechanical engineer transitioning to data analytics and AI consulting. Mannheim, Germany."
  defaultTheme = "dark"
  disableThemeToggle = false
  ShowReadingTime = false
  ShowShareButtons = false
  ShowPostNavLinks = false
  ShowBreadCrumbs = false
  ShowCodeCopyButtons = false
  ShowWordCount = false
  ShowRssButtonInSectionTermList = true
  dateFormat = "2006-01-02"
  DateFormat = "2006-01-02"

[[params.socialIcons]]
  name = "linkedin"
  url = "https://linkedin.com/in/angelojpfonseca"

[[params.socialIcons]]
  name = "github"
  url = "https://github.com/angelojpfonseca"

[[params.socialIcons]]
  name = "email"
  url = "mailto:contact@afonseca.me"

[menu]
  [[menu.main]]
    name = "About"
    url = "/about/"
    weight = 1
  [[menu.main]]
    name = "Projects"
    url = "/projects/"
    weight = 2
  # Writing menu entry intentionally commented out until first post exists.
  # [[menu.main]]
  #   name = "Writing"
  #   url = "/writing/"
  #   weight = 3
  [[menu.main]]
    name = "Contact"
    url = "/contact/"
    weight = 4
```

- [ ] **Step 2: Verify Hugo parses the config**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
hugo config | head -30
```

Expected: prints parsed config without errors; `baseurl = https://afonseca.me`, `languagecode = en`, theme `PaperMod`. If `hugo` isn't installed locally, skip this step — Task 5's CI will catch config errors.

- [ ] **Step 3: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add hugo.toml
git commit -m "config(hugo): align baseline with v0.1 spec"
```

---

### Task 3: Add archetypes for `project`, `post`, and singleton pages

**Files:**
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/archetypes/default.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/archetypes/projects.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/archetypes/writing.md`

- [ ] **Step 1: Replace `archetypes/default.md` for singleton pages**

Overwrite `archetypes/default.md` with:

```markdown
---
title: "{{ replace .Name "-" " " | title }}"
lastmod: {{ .Date }}
draft: false
---
```

Singletons are not drafts — they ship immediately and are updated in place.

- [ ] **Step 2: Create `archetypes/projects.md`**

Create new file `archetypes/projects.md` with:

```markdown
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
summary: ""
role: ""
period: ""
stack: []
links: []
featured: false
---

<!-- Case study body. Sections suggested:
- Context
- What I did
- Outcome / what I learned
-->
```

Projects default to `draft: true` so a half-written case study cannot accidentally ship.

- [ ] **Step 3: Create `archetypes/writing.md`**

Create new file `archetypes/writing.md` with:

```markdown
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
summary: ""
tags: []
---
```

Posts default to `draft: true`.

- [ ] **Step 4: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add archetypes/
git commit -m "feat(archetypes): add project, writing, and default singleton archetypes"
```

---

### Task 4: Add `CNAME` and minimal custom CSS scaffold

**Files:**
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/static/CNAME`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/assets/css/extended/custom.css`

- [ ] **Step 1: Create the CNAME file**

Create `static/CNAME` containing exactly one line (no trailing whitespace, no second line):

```
afonseca.me
```

Hugo copies anything under `static/` straight into the publish root, so this lands at `/CNAME` in the deployed site, which is what GitHub Pages reads.

- [ ] **Step 2: Create the custom CSS extension scaffold**

Create `assets/css/extended/custom.css` with the typography and palette baseline from the spec:

```css
/* assets/css/extended/custom.css
   PaperMod loads any file under assets/css/extended/ automatically.
   Keep additions here; do not edit PaperMod's own CSS files.
*/

:root {
  /* Single muted accent — earth-tone. Verified WCAG AA on both themes. */
  --afm-accent: #b87a4b;
}

.dark {
  --primary: #e8e6e0;            /* off-white body text */
  --secondary: #a8a59d;          /* dimmed text */
  --tertiary: #2a2a2a;            /* card / divider */
  --content: #e8e6e0;
  --code-bg: #1a1a1a;
  --entry: #141414;
  --theme: #0d0d0d;               /* near-black background */
  --border: #2a2a2a;
}

body,
.post-content,
.post-title,
.first-entry .entry-header h1,
.entry-header h2 {
  font-family: "Iowan Old Style", "Charter", "IBM Plex Serif",
               Georgia, "Times New Roman", serif;
}

code,
pre,
kbd,
samp {
  font-family: "JetBrains Mono", "SF Mono", Menlo, Consolas, monospace;
}

a {
  color: var(--afm-accent);
}

a:hover,
a:focus-visible {
  text-decoration: underline;
}

/* Preserve a visible focus ring for keyboard users. Do not remove. */
:focus-visible {
  outline: 2px solid var(--afm-accent);
  outline-offset: 2px;
}

.main {
  max-width: 720px;
}
```

- [ ] **Step 3: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add static/CNAME assets/
git commit -m "feat: add CNAME and custom CSS scaffold"
```

---

### Task 5: GitHub Actions deploy workflow

**Files:**
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/.github/workflows/deploy.yml`

- [ ] **Step 1: Verify Pages source is set to "GitHub Actions"**

Confirm Phase 0 prereq is complete. Either visit https://github.com/angelojpfonseca/afonseca.me/settings/pages in a browser, or run:

```bash
gh api repos/angelojpfonseca/afonseca.me/pages 2>&1 | head -5
```

Expected: shows `"build_type": "workflow"`. If it shows `"build_type": "legacy"` or 404, fix in Settings → Pages before pushing this task.

- [ ] **Step 2: Create the workflow file**

Create `.github/workflows/deploy.yml` with:

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

# Allow only one concurrent deployment, skipping runs queued between in-progress and latest queued.
concurrency:
  group: pages
  cancel-in-progress: false

permissions:
  contents: read
  pages: write
  id-token: write

env:
  HUGO_VERSION: 0.161.1

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb \
            https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb

      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Build with Hugo
        env:
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 3: Commit and push to trigger CI**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add .github/workflows/deploy.yml
git commit -m "ci: add GitHub Actions Hugo deploy workflow (Hugo v0.161.1)"
git push origin main
```

- [ ] **Step 4: Watch the run and verify deploy**

```bash
gh run watch -R angelojpfonseca/afonseca.me
```

Expected: run completes successfully; deploy job ends with `url: https://angelojpfonseca.github.io/afonseca.me/` or similar. If failures occur, inspect with `gh run view --log-failed -R angelojpfonseca/afonseca.me`.

At this point the site builds on every push to `main`. Site is empty (no `content/`); home renders PaperMod default. Content phases are next.

---

## Phase 2: Content scaffolding (4 tasks)

Each task in this phase creates pages with **minimal honest content** — no Lorem ipsum, no "Coming soon." Angelo refines drafts later in Phase 5.

### Task 6: Singleton pages — About, Contact, Colophon

**Files:**
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/about.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/contact.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/colophon.md`

- [ ] **Step 1: Create About page**

Create `content/about.md` with:

```markdown
---
title: "About"
lastmod: 2026-05-26
draft: false
---

Angelo Fonseca — mechanical engineer based in Mannheim, Germany, working through a transition into data analytics and AI consulting.

This page will grow as the transition proceeds. For now: see [Projects](/projects/) for what I'm building, and [the CV](/cv.pdf) for formal history.

Get in touch: [contact@afonseca.me](mailto:contact@afonseca.me).
```

The longer LinkedIn-sourced bio replaces this draft in Task 15. This minimal version is shippable today.

- [ ] **Step 2: Create Contact page**

Create `content/contact.md` with:

```markdown
---
title: "Contact"
lastmod: 2026-05-26
draft: false
---

- Email: [contact@afonseca.me](mailto:contact@afonseca.me)
- LinkedIn: [linkedin.com/in/angelojpfonseca](https://linkedin.com/in/angelojpfonseca)
- GitHub: [github.com/angelojpfonseca](https://github.com/angelojpfonseca)

The email goes to a forwarding alias; I read it the same day in most cases.
```

- [ ] **Step 3: Create Colophon page**

Create `content/colophon.md` with:

```markdown
---
title: "Colophon"
lastmod: 2026-05-26
draft: false
---

This site is built with [Hugo](https://gohugo.io) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme, with light CSS overrides. Source is on [GitHub](https://github.com/angelojpfonseca/afonseca.me); deploys are GitHub Actions to GitHub Pages on push.

- Typography: system serif stack (Iowan Old Style / Charter / IBM Plex Serif), JetBrains Mono for code.
- No JavaScript beyond the theme's own theme-switcher.
- No analytics, no trackers, no embeds, no third-party requests.
- Hosted at GitHub Pages with TLS from Let's Encrypt via GitHub's automation.

Design and implementation specs live under `docs/superpowers/` in the repo.
```

- [ ] **Step 4: Verify Hugo builds**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
hugo --quiet && echo "build OK" || echo "BUILD FAILED"
ls public/about/index.html public/contact/index.html public/colophon/index.html
```

Expected: `build OK`, all three `index.html` files exist. If `hugo` is not installed locally, push and observe CI run after Step 5.

- [ ] **Step 5: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add content/about.md content/contact.md content/colophon.md
git commit -m "content: add About, Contact, Colophon singletons (minimal v0.1 drafts)"
```

---

### Task 7: Personal-depth singletons — Now, Uses, Reading

**Files:**
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/now.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/uses.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/reading.md`

**Content rules in effect for this task** (spec §Content & privacy constraints):

- No mention of fatherhood, pregnancy, partner, household, or family.
- No exact address. "Mannheim, Germany" is the maximum granularity.
- No birthdate, no age, no phone.
- Now = themes only. No schedule, no travel plans, no real-time location.
- No internal infrastructure names, fleet project codenames, or homelab machine codenames.

- [ ] **Step 1: Create Now page**

Create `content/now.md` with:

```markdown
---
title: "Now"
lastmod: 2026-05-26
draft: false
---

Working through a career transition from mechanical engineering into data analytics and AI consulting. The shift is gradual, project-driven; I am learning by building tools I would want to use myself.

In parallel: deepening my comfort with Linux as a daily driver, getting better at writing clear technical English and German, and reading more deliberately than I have in years.

This page is updated occasionally as the focus shifts. Last updated: 2026-05-26.
```

- [ ] **Step 2: Create Uses page**

Create `content/uses.md` with:

```markdown
---
title: "Uses"
lastmod: 2026-05-26
draft: false
---

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
```

- [ ] **Step 3: Create Reading page**

Create `content/reading.md` with:

```markdown
---
title: "Reading"
lastmod: 2026-05-26
draft: false
---

A small, public-facing trace of what I am reading. Updated occasionally; not exhaustive.

### Now

*(Angelo to fill in with 1–3 current items during Phase 5.)*

### Recently finished

*(Angelo to fill in with 2–3 items during Phase 5.)*
```

The two `*(Angelo to fill in …)*` placeholders are explicit invitations to be replaced during the Phase 5 content pass, not silent gaps. After Phase 5 this page contains only real entries.

- [ ] **Step 4: Verify Hugo builds and pages render**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
hugo --quiet && echo "build OK" || echo "BUILD FAILED"
ls public/now/index.html public/uses/index.html public/reading/index.html
```

Expected: build succeeds, three files exist.

- [ ] **Step 5: Grep the rendered HTML for content-rule violations**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
grep -riE "fatherhood|pregnan|partner|wife|girlfriend|child|baby" public/ \
  | grep -v "favicon\|sitemap" \
  || echo "no content-rule violations found"
```

Expected: `no content-rule violations found`. If any matches surface, fix the source markdown and re-run.

- [ ] **Step 6: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add content/now.md content/uses.md content/reading.md
git commit -m "content: add Now, Uses, Reading singletons (minimal v0.1 drafts)"
```

---

### Task 8: Projects index and 3 featured project case-study stubs

**Files:**
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/projects/_index.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/projects/personal-website.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/projects/homelab.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/content/projects/data-analytics-transition.md`

Three projects is the v0.1 target. The case-study bodies are minimal v0.1 drafts (1–2 paragraphs each); they get expanded in Phase 5 once Angelo provides material. All three are marked `featured: true` so the home page picks them up.

- [ ] **Step 1: Create the Projects section index**

Create `content/projects/_index.md` with:

```markdown
---
title: "Projects"
lastmod: 2026-05-26
draft: false
---

A small set of case studies — work I have built, technical problems I have spent real time on. Updated as I write the next one.
```

- [ ] **Step 2: Create first project — Personal website**

Create `content/projects/personal-website.md` with:

```markdown
---
title: "This website"
date: 2026-05-26
draft: false
summary: "A static personal site built with Hugo and PaperMod, deployed via GitHub Actions to GitHub Pages."
role: "Designer, developer"
period: "2026"
stack: ["Hugo", "PaperMod", "GitHub Actions", "GitHub Pages"]
links:
  - title: "Source on GitHub"
    url: "https://github.com/angelojpfonseca/afonseca.me"
featured: true
---

The site you are reading. Designed and built end-to-end as a small but deliberate exercise in shipping something with a long shelf life: minimal JavaScript, no analytics, no third-party requests, content I can update from a terminal.

The interesting parts were not the code — it is a small Hugo site — but the editorial constraints I gave myself: a two-mode information architecture (professional header, personal footer), an explicit privacy posture documented alongside the design, and a refusal to ship empty sections marked "Coming soon."

The design spec and implementation plan are committed to the repo under `docs/superpowers/`.
```

- [ ] **Step 3: Create second project — Homelab**

Create `content/projects/homelab.md` with:

```markdown
---
title: "Homelab"
date: 2026-05-26
draft: false
summary: "A self-hosted homelab built around local-first principles — services running on my own hardware where possible."
role: "Architect, operator"
period: "2024 – present"
stack: ["Linux", "Containers", "Wireguard", "Caddy"]
links: []
featured: true
---

A small homelab I run on the side, built around a single rule: anything dealing with personal or sensitive data runs locally on hardware I control, on a network with default-deny policies. Cloud services are used only where no local alternative exists.

The interesting tension is operating cost: every service has a maintenance budget, and the lab is only useful if I can keep it healthy without it eating my evenings. The architecture has gone through several rounds of simplification — fewer moving parts, more boring choices, tighter monitoring.

A write-up of specific components and the lessons I have learned will land here as I have time to write them properly.
```

- [ ] **Step 4: Create third project — Data analytics / AI consulting transition**

Create `content/projects/data-analytics-transition.md` with:

```markdown
---
title: "Career transition: engineering → data and AI"
date: 2026-05-26
draft: false
summary: "A multi-year, project-driven transition out of mechanical engineering into data analytics and AI consulting."
role: "—"
period: "2024 – present"
stack: ["Python", "SQL", "Pandas", "AI tooling"]
links: []
featured: true
---

The arc of the last few years: shifting from mechanical engineering, where I trained and worked for over a decade, into data analytics and AI consulting. The shift is gradual and project-driven — I am learning by building tools I want to use myself, then writing about what worked.

The mechanical engineering background turns out to be more useful than I first expected. Engineering teaches a particular kind of patience with messy real-world data — physical systems do not give you clean schemas — and a habit of asking what the numbers actually represent before deciding what to do with them.

Case studies of specific projects will land on this page as they are ready to be shown.
```

- [ ] **Step 5: Verify build + content lints**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
hugo --quiet && echo "build OK"
ls public/projects/index.html \
   public/projects/personal-website/index.html \
   public/projects/homelab/index.html \
   public/projects/data-analytics-transition/index.html
grep -riE "fatherhood|pregnan|partner|wife|girlfriend|child|baby" public/projects/ \
  || echo "no content-rule violations found"
```

Expected: build OK, all 4 files exist, no rule violations.

- [ ] **Step 6: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add content/projects/
git commit -m "content(projects): add 3 featured project case-study stubs"
```

---

### Task 9: Verify `/writing/` is absent in v0.1 and drafts cannot leak

**Files:**
- Create (temporary): `~/src/github.com/angelojpfonseca/afonseca.me/content/writing/draft-canary.md`

The Writing section must return 404 in v0.1 (per spec URL map) and drafts must never leak into RSS. Hugo only generates `/writing/` if `content/writing/` exists, so the 404 behaviour is achieved by not creating that directory — no config flag needed. We separately verify draft suppression with a canary draft and then remove it.

- [ ] **Step 1: Confirm no `/writing/` directory exists**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
ls content/writing 2>&1 | head -1
```

Expected: `ls: cannot access 'content/writing': No such file or directory`. If the directory exists from earlier experimentation, remove it: `rm -rf content/writing`.

- [ ] **Step 2: Confirm no `/writing/` is produced by the current build**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet
ls public/writing 2>&1 | head -1
```

Expected: `ls: cannot access 'public/writing': No such file or directory`. Good — without a `content/writing/` directory, Hugo produces no Writing pages.

- [ ] **Step 3: Verify the canary draft-suppression behaviour**

Create a temporary canary draft to confirm Hugo will hide drafts when Writing eventually exists:

```bash
mkdir -p ~/src/github.com/angelojpfonseca/afonseca.me/content/writing
cat > ~/src/github.com/angelojpfonseca/afonseca.me/content/writing/draft-canary.md <<'EOF'
---
title: "Canary draft — must not appear in any built output"
date: 2026-05-26
draft: true
summary: "If you can read this on the live site, draft suppression is broken."
tags: ["canary"]
---

This file is a deliberate canary to verify `buildDrafts = false`. It must be deleted before Task 9 commits.
EOF

cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet
ls public/writing 2>&1 | head -1
grep -ri "canary" public/ 2>&1 | head -5 || echo "canary not present in build — draft suppression OK"
```

Expected: `public/writing` does not exist; `canary not present in build — draft suppression OK`.

- [ ] **Step 4: Remove the canary and its directory**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf content/writing
rm -rf public/
```

- [ ] **Step 5: Final build to confirm clean state**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
hugo --quiet && echo "build OK"
ls public/writing 2>&1 | head -1
```

Expected: build OK; `/writing/` does not exist.

- [ ] **Step 6: No commit for this task — purely a verification gate**

This task creates no permanent files. The result is documented as a checklist completion. Move on to Task 10.

---

## Phase 3: Home page template (2 tasks)

### Task 10: Custom `layouts/index.html` skeleton with hero

**Files:**
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/layouts/index.html`

The custom home template replaces PaperMod's default `homeInfoParams` rendering (already removed from `hugo.toml` in Task 2).

- [ ] **Step 1: Create `layouts/index.html` with hero block only**

Create `layouts/index.html` with:

```html
{{- define "main" }}

<section class="afm-hero">
  <h1 class="afm-hero__title">Angelo Fonseca</h1>
  <p class="afm-hero__tagline">
    Mechanical engineer transitioning to data analytics and AI consulting.
    Based in Mannheim, Germany.
  </p>
  <p class="afm-hero__cta">
    <a href="/about/">About</a> · <a href="/projects/">Projects</a> · <a href="/cv.pdf">CV (PDF)</a>
  </p>
</section>

{{- /* Task 11 inserts cards section here */}}

{{- end }}
```

- [ ] **Step 2: Extend `assets/css/extended/custom.css` with hero styles**

Append to `assets/css/extended/custom.css`:

```css
/* Home hero */
.afm-hero {
  padding: 4rem 0 3rem;
  text-align: left;
}

.afm-hero__title {
  font-size: 2.25rem;
  margin: 0 0 0.5rem;
  font-weight: 600;
}

.afm-hero__tagline {
  font-size: 1.125rem;
  color: var(--secondary);
  margin: 0 0 1.5rem;
  max-width: 36em;
}

.afm-hero__cta a {
  font-weight: 500;
}
```

- [ ] **Step 3: Build and verify the home page renders the custom template**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet
grep -c "afm-hero__title" public/index.html
```

Expected: `1` (the custom hero renders).

- [ ] **Step 4: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add layouts/index.html assets/css/extended/custom.css
git commit -m "feat(home): custom index template with hero block"
```

---

### Task 11: Home page cards (featured projects, Now snippet, About link)

**Files:**
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/layouts/index.html`
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/assets/css/extended/custom.css`

- [ ] **Step 1: Replace `layouts/index.html` with the full cards version**

Overwrite `layouts/index.html` with:

```html
{{- define "main" }}

<section class="afm-hero">
  <h1 class="afm-hero__title">Angelo Fonseca</h1>
  <p class="afm-hero__tagline">
    Mechanical engineer transitioning to data analytics and AI consulting.
    Based in Mannheim, Germany.
  </p>
  <p class="afm-hero__cta">
    <a href="/about/">About</a> · <a href="/projects/">Projects</a> · <a href="/cv.pdf">CV (PDF)</a>
  </p>
</section>

<section class="afm-cards" aria-label="Highlights">
  {{- $featured := where (where .Site.RegularPages "Section" "projects") "Params.featured" true }}
  {{- range first 3 $featured }}
    <article class="afm-card afm-card--project">
      <h2 class="afm-card__title"><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
      {{- with .Params.summary }}<p class="afm-card__summary">{{ . }}</p>{{ end }}
      {{- with .Params.period }}<p class="afm-card__meta">{{ . }}</p>{{ end }}
    </article>
  {{- end }}

  {{- with .Site.GetPage "/now" }}
    <article class="afm-card afm-card--now">
      <h2 class="afm-card__title"><a href="{{ .RelPermalink }}">Now</a></h2>
      <p class="afm-card__summary">{{ .Plain | truncate 240 }}</p>
      <p class="afm-card__meta">Updated {{ .Lastmod.Format "2006-01-02" }}</p>
    </article>
  {{- end }}

  <article class="afm-card afm-card--about">
    <h2 class="afm-card__title"><a href="/about/">About</a></h2>
    <p class="afm-card__summary">
      Background, what I work on, how to reach me.
    </p>
  </article>
</section>

{{- end }}
```

- [ ] **Step 2: Append cards CSS**

Append to `assets/css/extended/custom.css`:

```css
/* Home cards */
.afm-cards {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1.5rem;
  padding: 1rem 0 4rem;
}

@media (min-width: 720px) {
  .afm-cards {
    grid-template-columns: 1fr 1fr;
  }
  /* Span the About card across both columns on wider screens. */
  .afm-card--about {
    grid-column: 1 / -1;
  }
}

.afm-card {
  padding: 1.25rem 1.25rem 1rem;
  border: 1px solid var(--border);
  border-radius: 6px;
  background: var(--entry);
}

.afm-card__title {
  font-size: 1.125rem;
  margin: 0 0 0.5rem;
  font-weight: 600;
}

.afm-card__title a {
  color: var(--primary);
  text-decoration: none;
}

.afm-card__title a:hover,
.afm-card__title a:focus-visible {
  color: var(--afm-accent);
}

.afm-card__summary {
  font-size: 0.95rem;
  margin: 0 0 0.5rem;
  color: var(--primary);
}

.afm-card__meta {
  font-size: 0.85rem;
  color: var(--secondary);
  margin: 0;
}
```

- [ ] **Step 3: Build and verify cards render**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet
grep -c "afm-card--project" public/index.html
grep -c "afm-card--now" public/index.html
grep -c "afm-card--about" public/index.html
```

Expected: `3` project cards, `1` now card, `1` about card.

- [ ] **Step 4: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add layouts/index.html assets/css/extended/custom.css
git commit -m "feat(home): add featured-project, Now, About cards on home page"
```

---

## Phase 4: Visual finishing (3 tasks)

### Task 12: Footer with personal-depth links

**Files:**
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/layouts/partials/footer.html`

PaperMod's built-in footer renders only social icons and a copyright line. The spec wants four text links (Now · Uses · Reading · Colophon) in the footer in addition to the existing GitHub / LinkedIn / email social icons. We override the footer partial.

- [ ] **Step 1: Locate PaperMod's footer partial as a reference**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
head -40 themes/PaperMod/layouts/partials/footer.html
```

This is read-only context — we are not editing the theme. The override goes in `layouts/partials/`.

- [ ] **Step 2: Create the override partial**

Create `layouts/partials/footer.html` with:

```html
{{- if not (.Param "hideFooter") }}
<footer class="footer">
  <nav class="afm-footer-nav" aria-label="Personal-depth pages">
    <a href="/now/">Now</a>
    <a href="/uses/">Uses</a>
    <a href="/reading/">Reading</a>
    <a href="/colophon/">Colophon</a>
  </nav>

  <div class="afm-footer-meta">
    <span>&copy; {{ now.Format "2006" }} {{ .Site.Params.author | default .Site.Title }}</span>
    <span class="afm-footer-sep">·</span>
    <a href="https://github.com/angelojpfonseca">GitHub</a>
    <span class="afm-footer-sep">·</span>
    <a href="https://linkedin.com/in/angelojpfonseca">LinkedIn</a>
    <span class="afm-footer-sep">·</span>
    <a href="mailto:contact@afonseca.me">Email</a>
  </div>
</footer>
{{- end }}
```

Note: this replaces PaperMod's default social-icons row with text links to keep the footer single-typeface and minimal, per the spec's "archival-feeling" direction.

- [ ] **Step 3: Append footer styles to custom CSS**

Append to `assets/css/extended/custom.css`:

```css
/* Footer */
.footer {
  border-top: 1px solid var(--border);
  margin-top: 4rem;
  padding: 1.5rem 0 2.5rem;
  text-align: center;
  font-size: 0.9rem;
  color: var(--secondary);
}

.afm-footer-nav {
  margin-bottom: 0.75rem;
}

.afm-footer-nav a {
  margin: 0 0.5rem;
  color: var(--primary);
}

.afm-footer-meta a {
  color: var(--secondary);
}

.afm-footer-meta a:hover,
.afm-footer-meta a:focus-visible {
  color: var(--afm-accent);
}

.afm-footer-sep {
  margin: 0 0.35rem;
  color: var(--border);
}
```

- [ ] **Step 4: Build and verify footer renders on every page**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet
grep -c "afm-footer-nav" public/index.html
grep -c "afm-footer-nav" public/about/index.html
grep -c "afm-footer-nav" public/now/index.html
```

Expected: `1` on every page.

- [ ] **Step 5: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add layouts/partials/footer.html assets/css/extended/custom.css
git commit -m "feat(footer): override PaperMod footer with personal-depth links"
```

---

### Task 13: Favicon assets

**Files:**
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/static/favicon.ico`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/static/favicon.svg`

The favicon is a simple `AF` monogram. SVG is hand-authored; ICO is generated from the SVG.

- [ ] **Step 1: Create `static/favicon.svg`**

Create `static/favicon.svg` with:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64">
  <rect width="64" height="64" rx="8" fill="#0d0d0d"/>
  <text x="32" y="42"
        font-family="Iowan Old Style, Charter, IBM Plex Serif, Georgia, serif"
        font-size="34" font-weight="600"
        fill="#b87a4b"
        text-anchor="middle">AF</text>
</svg>
```

- [ ] **Step 2: Generate `favicon.ico` from the SVG**

If ImageMagick is available:

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
magick -background none static/favicon.svg -resize 32x32 static/favicon.ico
```

If ImageMagick is not available, use `rsvg-convert` + `convert`:

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rsvg-convert -w 32 -h 32 static/favicon.svg > /tmp/favicon-32.png
convert /tmp/favicon-32.png static/favicon.ico
```

If neither is available, generate the ICO by uploading `favicon.svg` to a local one-shot tool or skip — PaperMod's default favicon ships meanwhile. Note the omission in the commit message.

- [ ] **Step 3: Build and verify**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet
ls public/favicon.ico public/favicon.svg
```

Expected: both files exist in `public/`.

- [ ] **Step 4: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add static/favicon.ico static/favicon.svg
git commit -m "feat: add AF-monogram favicon (svg + ico)"
```

---

### Task 14: CSS polish + accessibility verification

**Files:**
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/assets/css/extended/custom.css`

- [ ] **Step 1: Inspect current rendering locally**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
hugo server --quiet &
HUGO_PID=$!
sleep 2
curl -s http://localhost:1313/ | head -40
kill $HUGO_PID 2>/dev/null
```

Expected: HTML preview of the home page; no errors.

- [ ] **Step 2: Verify accent colour contrast against the dark theme**

The accent `#b87a4b` on background `#0d0d0d` must meet WCAG AA (4.5:1 normal text, 3:1 large text).

Compute contrast ratio (any contrast checker, command-line or web). The accent is used only on links and the focus ring — link text size is body-size, so 4.5:1 is the threshold. Expected: `#b87a4b` on `#0d0d0d` ≈ 5.0:1 (passes AA).

If a contrast check tool is locally available:

```bash
# Example with `pa11y` or a manual check tool. If none available, document the
# computed contrast ratio in a quick note. The check can also be a manual
# browser-DevTools inspection during Task 18 Lighthouse pass.
echo "Accent #b87a4b on #0d0d0d — verify ≥4.5:1 via WebAIM contrast checker before publishing."
```

If contrast fails (below 4.5:1), pick a slightly lighter accent (e.g., `#c98a5b`) and update `--afm-accent` in `assets/css/extended/custom.css`. Re-check.

- [ ] **Step 3: Verify focus styles are preserved**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
grep -c "focus-visible" assets/css/extended/custom.css
```

Expected: at least `1`. If `0`, restore the `:focus-visible` rule from Task 4 Step 2.

- [ ] **Step 4: Verify language attribute on rendered HTML**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet
grep -m1 "<html" public/index.html
```

Expected: `<html lang="en" ...>`. If the `lang` attribute is missing, check `hugo.toml`'s `languageCode` / `defaultContentLanguage`.

- [ ] **Step 5: Commit any CSS adjustments**

If Step 2 forced an accent change or any other tweak landed:

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add assets/css/extended/custom.css
git commit -m "style: accessibility polish on CSS (contrast, focus)"
```

If no changes were necessary, skip the commit.

---

## Phase 5: Content pass with Angelo (3 tasks)

These tasks require Angelo's material. They are collaborative: each task ends with a commit of refined content.

### Task 15: About + CV refinement from LinkedIn

**Files:**
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/content/about.md`
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/static/cv.pdf`

- [ ] **Step 1: Angelo provides LinkedIn-exported PDF**

Angelo runs: Profile → "More" → "Save to PDF" on LinkedIn, then provides the file path or moves the PDF to `~/Downloads/`.

- [ ] **Step 2: Copy CV PDF into `static/`**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
# Adjust the source path to wherever Angelo saved the file.
cp ~/Downloads/Profile.pdf static/cv.pdf
ls -lh static/cv.pdf
```

Expected: file present, size reasonable (typical LinkedIn-export PDF is 50–200 KB).

- [ ] **Step 3: Angelo pastes LinkedIn About + Experience text in chat**

Angelo pastes (in chat) the LinkedIn "About" section text, plus a list of past employers with dates and one-line role descriptions. Implementer drafts a refined About page body from this.

- [ ] **Step 4: Replace `content/about.md` body with the refined draft**

Keep the front matter (`title`, `lastmod`, `draft: false`) unchanged. Replace the body with the LinkedIn-derived content. Constraints:

- Must comply with **spec §Content & privacy constraints** — no fatherhood/partner/household; no exact address; no birthdate/age/phone.
- Past employers named per LinkedIn (spec constraint #10).
- English, professional tone (front-door surface).
- Ends with a contact line linking to `mailto:contact@afonseca.me`.

The draft is finalised against Angelo's edits in the same session.

- [ ] **Step 5: Verify build + content-rule lint**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet
grep -riE "fatherhood|pregnan|partner|wife|girlfriend|child|baby" public/about/ \
  || echo "About page passes content-rule lint"
ls public/cv.pdf
```

Expected: lint passes; `public/cv.pdf` exists.

- [ ] **Step 6: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add content/about.md static/cv.pdf
git commit -m "content(about,cv): refine About from LinkedIn material; add cv.pdf"
```

---

### Task 16: Headshot integration + Projects content refinement

**Files:**
- Create: `~/src/github.com/angelojpfonseca/afonseca.me/static/img/angelo.jpg` (or `.webp`)
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/content/about.md`
- Modify: each of the three `content/projects/*.md`

- [ ] **Step 1: Place the headshot**

Angelo provides the headshot file. Confirm: ≤500KB after compression; ≥800px on the long edge.

```bash
mkdir -p ~/src/github.com/angelojpfonseca/afonseca.me/static/img
cp <angelo-headshot-source-path> ~/src/github.com/angelojpfonseca/afonseca.me/static/img/angelo.jpg
ls -lh ~/src/github.com/angelojpfonseca/afonseca.me/static/img/angelo.jpg
```

Expected: file present, size and dimensions reasonable.

- [ ] **Step 2: Reference the headshot from About**

Insert at the top of the body of `content/about.md` (between the front matter and the existing first paragraph):

```markdown
![Angelo Fonseca](/img/angelo.jpg "Angelo Fonseca")
```

The image alt text is the subject's name. The browser title attribute is the same — short, identical, no marketing copy.

- [ ] **Step 3: Refine the 3 project case studies**

For each of `content/projects/personal-website.md`, `content/projects/homelab.md`, `content/projects/data-analytics-transition.md`:

- Angelo provides specifics (any pre-fleet engineering case study from LinkedIn replacing one of the three is allowed).
- Constraints: codename-free language; no NDA leaks; English; consistent with the LinkedIn arc.
- Each case study is ≥2 paragraphs, ≤6 paragraphs. Each has a meaningful `summary` (≤140 chars), `period`, `stack[]`, and optional `links[]`.

- [ ] **Step 4: Verify build + lint**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet && echo "build OK"
grep -riE "fatherhood|pregnan|partner|wife|girlfriend|child|baby" public/ \
  | grep -v "favicon\|sitemap" \
  || echo "no content-rule violations found"
ls public/img/angelo.jpg
```

Expected: build OK; no lint hits; headshot present.

- [ ] **Step 5: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add static/img/angelo.jpg content/about.md content/projects/
git commit -m "content: integrate headshot; refine 3 project case studies"
```

---

### Task 17: Now / Uses / Reading refinement

**Files:**
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/content/now.md`
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/content/uses.md`
- Modify: `~/src/github.com/angelojpfonseca/afonseca.me/content/reading.md`

- [ ] **Step 1: Angelo provides current reading items**

Angelo names 1–3 currently-reading books/papers and 2–3 recently-finished items. For each: title, author, optionally a one-line take.

- [ ] **Step 2: Replace placeholders in `content/reading.md`**

Remove the `*(Angelo to fill in …)*` lines and replace with the actual list. Each item:

```markdown
- **Title** — Author. One-line take (optional).
```

- [ ] **Step 3: Refine Now and Uses against Angelo's feedback**

Angelo reads `content/now.md` and `content/uses.md` and asks for any specific edits. Apply them; keep within content rules (no fatherhood, no fleet codenames, no real-time location on Now).

- [ ] **Step 4: Update `lastmod` on all three files to today's ISO date**

- [ ] **Step 5: Verify build + lint**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --quiet && echo "build OK"
grep -riE "fatherhood|pregnan|partner|wife|girlfriend|child|baby" public/ \
  | grep -v "favicon\|sitemap" \
  || echo "no content-rule violations found"
grep -i "to fill in" public/ -r || echo "no placeholders remaining"
```

Expected: build OK, no lint hits, no remaining placeholder text.

- [ ] **Step 6: Commit**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git add content/now.md content/uses.md content/reading.md
git commit -m "content: refine Now, Uses, Reading from Angelo's material"
```

---

## Phase 6: Launch (3 tasks)

### Task 18: Lighthouse audit + accessibility verification

**Files:** none (audit task — no source changes unless regressions surface).

- [ ] **Step 1: Build production output**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
rm -rf public/
hugo --minify
```

- [ ] **Step 2: Serve and run Lighthouse**

Run a local Lighthouse audit. If `lighthouse` CLI is installed:

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
npx lighthouse http://localhost:1313 --only-categories=performance,accessibility,best-practices --quiet --chrome-flags="--headless" --output=json --output-path=/tmp/lighthouse.json &
hugo server --port 1313 &
sleep 8
wait
cat /tmp/lighthouse.json | jq '.categories | to_entries | map({key, score: .value.score})'
```

If `lighthouse` is not locally available, push current state (still on `main` if implementer has push rights, or to a `gh-pages` preview branch) and use Chrome DevTools → Lighthouse panel against the deployed URL.

Expected: Performance ≥ 0.9, Accessibility ≥ 0.9. If either is below 0.9, fix before proceeding.

- [ ] **Step 3: Manual accessibility check**

Open the deployed (or local) home page in a browser. Verify:

- Tab through every link from the top of the page — each focused element shows a visible focus indicator.
- Toggle the PaperMod theme switcher; verify dark→light contrast still meets the spec.
- Inspect the headshot's `alt` attribute (DevTools): not empty, not "image", not the filename.
- View source: `<html lang="en">` present.

- [ ] **Step 4: Fix any issues; commit if changes required**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git status
# If changes:
git add <files>
git commit -m "fix(a11y/perf): address Lighthouse / accessibility audit findings"
```

If no changes are required, no commit.

---

### Task 19: Integration review

**Files:** none (review task — surfaces issues which become follow-ups in their own tasks).

This is the single integration review for the content-vault pass, per the project memory `feedback_subagent-driven-for-content-vaults.md` (one integration reviewer at the end, no per-task quality review).

- [ ] **Step 1: Dispatch the integration reviewer subagent**

Use the `Agent` tool with `general-purpose` (or a code-reviewer agent if available). Prompt:

> You are a final-pass reviewer on the v0.1 implementation of the personal website at `~/src/github.com/angelojpfonseca/afonseca.me/`. The design spec is at `docs/superpowers/specs/2026-05-26-afonseca-me-redesign-design.md` and the implementation plan is at `docs/superpowers/plans/2026-05-26-afonseca-me-v0.1.md`.
>
> Read the spec, then walk through every committed file. Report:
> 1. Any **content-rule violations** (spec §Content & privacy constraints — fatherhood, partner, address, age, phone, codenames). Grep the rendered `public/` output and the source `content/` markdown.
> 2. Any **success-criteria gaps** (spec §Success criteria for v0.1).
> 3. Any **stale text** — placeholder language ("to fill in", "TODO", "Coming soon"), dead links, mismatched dates.
> 4. Any **technical debt** that should be opened as a GitHub issue before launch.
>
> Format: severity-ordered (must-fix → should-address → nice-to-have → notes). Under 600 words. Cite specific files and line ranges.

- [ ] **Step 2: Address must-fix items inline**

For each `must-fix` finding: edit the relevant file(s), verify the build, commit per the standard pattern (`fix(content/<area>): <description>`). Repeat until no `must-fix` items remain.

- [ ] **Step 3: Open `should-address` and below as new GitHub issues**

For each remaining finding, open an issue in `angelojpfonseca/afonseca.me` with `gh issue create`. Reference the file path and the relevant section of the spec.

- [ ] **Step 4: Confirm clean state before cutover**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git status
rm -rf public/
hugo --minify --quiet && echo "build OK"
```

Expected: clean working tree; build OK.

---

### Task 20: DNS cutover + TLS verification + smoke test

**Files:** none (live-deployment task — most steps are Angelo's at IONOS UI).

- [ ] **Step 1: Confirm Pages deployment is current**

```bash
gh run list -R angelojpfonseca/afonseca.me -L 1
```

Expected: most recent run is on `main` and shows `completed success`.

- [ ] **Step 2: Confirm Pages is serving at the GitHub default**

```bash
curl -sI https://angelojpfonseca.github.io/afonseca.me/ | head -3
```

Expected: `HTTP/2 200`.

- [ ] **Step 3: Angelo configures DNS records at IONOS**

In the IONOS control panel for `afonseca.me`, set:

- **A record (apex)** `@` → `185.199.108.153`
- **A record (apex)** `@` → `185.199.109.153`
- **A record (apex)** `@` → `185.199.110.153`
- **A record (apex)** `@` → `185.199.111.153`
- **CNAME record** `www` → `angelojpfonseca.github.io.`
- TTL: leave at IONOS default (3600 typically) or reduce to 300 for the cutover.

Remove any AAAA / conflicting A records.

- [ ] **Step 4: Wait for DNS propagation**

```bash
# Repeat every minute until all four addresses resolve.
dig +short afonseca.me
```

Expected (within ~30 minutes): the four GitHub Pages IPs appear.

- [ ] **Step 5: Configure custom domain in GitHub Pages**

In repo Settings → Pages → "Custom domain" enter `afonseca.me`. GitHub verifies the DNS and begins TLS provisioning. Tick "Enforce HTTPS" once available (may take up to 24h, usually under 1h).

- [ ] **Step 6: Verify TLS and end-to-end**

```bash
curl -sI https://afonseca.me | head -5
curl -sI https://afonseca.me/cv.pdf | head -3
curl -s https://afonseca.me/ | grep -o "afm-card--project" | head -5
curl -s https://afonseca.me/sitemap.xml | head -10
curl -sI https://afonseca.me/writing/ | head -3
```

Expected:
- Home and `cv.pdf` return `HTTP/2 200`.
- Home contains 3 `afm-card--project` matches.
- Sitemap is valid XML and includes the v0.1 pages.
- `/writing/` returns `HTTP/2 404`.

- [ ] **Step 7: Mobile + browser smoke test**

Open `https://afonseca.me` on a phone and a desktop browser. Verify:

- Home cards render correctly on both viewports.
- Theme switcher works.
- All header links resolve to 200 pages (About, Projects, Contact).
- All footer links resolve correctly (Now, Uses, Reading, Colophon).
- Headshot loads on About.
- `cv.pdf` downloads cleanly.

- [ ] **Step 8: Tag v0.1**

```bash
cd ~/src/github.com/angelojpfonseca/afonseca.me
git tag -a v0.1 -m "afonseca.me v0.1 — initial published site"
git push origin v0.1
```

- [ ] **Step 9: Close all phase-related TODOs**

In the repo's GitHub issues, close any issue that has effectively been resolved by v0.1 (e.g., none — most issues are deferred work). Sanity-check the issue list.

---

## Done

When Task 20 completes, `https://afonseca.me` is live with TLS, the v0.1 success criteria are met, the design spec and plan are committed and tagged. Subsequent enhancements proceed via the existing GitHub issues (#1–#10, #12).
