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
| `IBMPlexSerif-Italic.woff2` | `ba5feed9ebae36e3b6ae3486052c90f4f42a294fa67a7c9415985175d19c4c82` |
| `IBMPlexSerif-SemiBold.woff2` | `030d808e82f99ebe5c21d50745bd06e5ce16ad9e94b360f5adcc19362beb5344` |
| `IBMPlexSerif-Bold.woff2` | `3b9eb99793dd9fed419aaf1af03559ea28bac17b7cb6146e7f8fc3db813621fe` |

## JetBrains Mono

- Source: <https://github.com/JetBrains/JetBrainsMono>, release `v2.304`
  (2023-01-14), asset `JetBrainsMono-2.304.zip`, files from `fonts/webfonts/`.
- License: SIL OFL 1.1 (`docs/fonts/LICENSE-JetBrains-Mono.txt`).

| File | sha256 |
|---|---|
| `JetBrainsMono-Regular.woff2` | `a9cb1cd82332b23a47e3a1239d25d13c86d16c4220695e34b243effa999f45f2` |
| `JetBrainsMono-Medium.woff2` | `086c48dfbea9ddaff1320f7e09399b8e2924e88ce67453721255db3bdbb5a353` |

## Weight map (what the CSS uses)

- Serif 400 regular + italic (body), 600 (headings, site name), 700 (`strong`).
- Mono 400 (code, nav), 500 (kickers, spec-table headers, title-block labels).

Total payload: ~472 KB across six files; each page loads at most four.
