# Self-hosted fonts — provenance and pins

The site serves its own webfonts from `static/fonts/` (same origin, no font
CDN — the no-third-party-requests posture holds). Both families are licensed
under the SIL Open Font License 1.1; license texts are vendored in
`docs/fonts/`. Upgrade policy: same as everything else — manual, after
testing, never automatic. If you replace a file, update its hash here.

Added 2026-07-04 (v0.3). Rationale: the previous system-font stack rendered
differently per OS (Iowan Old Style on macOS, Georgia/DejaVu elsewhere).
IBM Plex Serif was already an accepted face in the stack and is freely
redistributable; Iowan Old Style and Charter are not cleanly so.

## IBM Plex Serif

- Source: <https://github.com/IBM/plex>, release `@ibm/plex-serif@2.0.0`
  (2026-02-02), asset `ibm-plex-serif.zip`, files from `fonts/complete/woff2/`.
- License: SIL OFL 1.1 (`docs/fonts/LICENSE-IBM-Plex-Serif.txt`).

| File | sha256 |
|---|---|
| `IBMPlexSerif-Regular.woff2` | `024ebce13cec984b46e350dd85fa7c01105c777e116bfe95f097ad7fa93f39f2` |
| `IBMPlexSerif-SemiBold.woff2` | `030d808e82f99ebe5c21d50745bd06e5ce16ad9e94b360f5adcc19362beb5344` |

## JetBrains Mono

- Source: <https://github.com/JetBrains/JetBrainsMono>, release `v2.304`
  (2023-01-14), asset `JetBrainsMono-2.304.zip`, files from `fonts/webfonts/`.
- License: SIL OFL 1.1 (`docs/fonts/LICENSE-JetBrains-Mono.txt`).

| File | sha256 |
|---|---|
| `JetBrainsMono-Regular.woff2` | `a9cb1cd82332b23a47e3a1239d25d13c86d16c4220695e34b243effa999f45f2` |

## Weight map (what the CSS uses)

- Serif 400 (body), 600 (headings, site name, and `strong`/`b` via an explicit
  rule — no separate Bold file). Italic is browser-synthesized (a single `em`
  exists site-wide); revisit if italic prose grows.
- Mono 400 (code, nav, kickers, spec-table headers, title-block labels).

Three files only, all preloaded; total payload ~236 KB. Trimmed from six files
on 2026-07-04 after review: Medium/Bold/Italic bought no visible difference at
the sizes used (review finding, PR #18).
