# Rezflix Help

Help center for **Rezflix**, my private media streaming service. It is the first place my users go
when they need help, before contacting me directly.

- **Site URL:** https://help.rezflixtv.com
- **Hosting:** GitHub Pages, deployed with GitHub Actions on every push to `main`
- **DNS:** Cloudflare (managed by me)
- **Generator:** Material for MkDocs
- **Rezflix services** follow the pattern `[service].rezflixtv.com`:
  - **Jellyfin** ("Rezflix" to users): the streaming server (watching movies and TV shows)
  - **Seerr** ("Rezflix Request" to users): where users request new movies and TV shows
  - Exact subdomains: to be confirmed by me (see "Decisions log")

## Naming (user-facing)

- **Rezflix:** the name users know for Jellyfin (the watch service). Use "Rezflix", not "Jellyfin".
- **Rezflix Request:** the name users know for Seerr (the request service). Never say "Seerr".
- Mention "Jellyfin" only where users must see the real name, for example installing the Jellyfin
  app from an app store. Explain it once there ("Rezflix runs on the Jellyfin app").

## Audience

Rezflix users: friends, family, and invited people. Mostly non-technical. They watch on TVs, phones,
browsers, and streaming devices. Write for them, not for developers.

## Planned content (structure not final)

These are the areas the site must cover. Page names, order, wording, look, and images are decided
by me during development. Until I decide, use simple placeholders and do not invent final content.

- **Landing page:** short intro, Rezflix and Rezflix Request buttons, search hint, cards
  linking to the main sections, a "Common problems" list linking to troubleshooting entries, and a
  pointer to contact.
- **Quick start:** the single entry point for new users. Concise steps from invite to watching
  (logging in, apps per device, watching, requesting). Each step links to a detail page instead
  of explaining everything inline.
- **Detail pages:** an expanding tree of short, focused, interlinked pages (for example one per
  device app, player setting, or user setting), linked from Quick start and from each other, so
  existing users can find one topic fast without reading new-user instructions. There is no
  separate "New user's guide" page.
- **Troubleshooting:** common problems and how users can try to fix them first: subtitles, video
  quality, playback errors and buffering, audio and subtitle language, login problems.
- **FAQ and more:** to be defined later.
- **Contact:** how to reach me when the docs did not solve the problem.

Do not make claims about Jellyfin or Seerr features, settings, or app behavior from memory without
flagging them for me to verify. Accuracy matters more than completeness here.

## Keeping this file up to date

This file is the source of truth for project decisions. Whenever I decide something (page names,
nav structure, theme colors, logo, wording rules, service URLs, recommended apps, anything), update
the relevant section of this file and add a dated line to the "Decisions log" in the same task.
Replace placeholders in this file once they are decided. Keep this file concise.

This file is committed and public, like the rest of the repo. Never write critical or private
information into it (see "Security"). Personal, machine-specific, or private notes go in
`CLAUDE.local.md`, which is gitignored.

## Tech stack

- Material for MkDocs on top of MkDocs, Python.
- Exact versions pinned in `requirements.txt` for reproducible builds.
- Keep it simple: no extra plugins or tooling unless they solve a real problem I have.
  Allowed by default: `search`, `glightbox` (click to zoom screenshots).
- Pinned: `mkdocs==1.6.1`, `mkdocs-material==9.7.7`, `mkdocs-glightbox==0.5.2`,
  `pymdown-extensions==10.21.3`. Do not upgrade to MkDocs 2.x (incompatible with Material).
- Material for MkDocs reaches end of life on 2026-11-05 (fixes only until then). Its successor
  Zensical reads `mkdocs.yml` and supports `search` and `glightbox`, but is still alpha (0.0.x,
  needs Python 3.11+). Revisit migrating once Zensical reaches beta.
- CI builds with Python 3.13. Local builds work on Python 3.9 or newer.

## Local development (Python venv, required)

All Python work happens inside a project-local virtual environment in `.venv/`. Never install
packages globally or with `--user`, and never modify system PATH or system Python.

Setup on any OS:

```
python -m venv .venv
# macOS / Linux:
source .venv/bin/activate
# Windows (PowerShell):
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

- Preview: `mkdocs serve --watch-theme` (English, http://127.0.0.1:8000),
  `mkdocs serve --watch-theme -f mkdocs.tr.yml -a 127.0.0.1:8001` (Turkish, http://127.0.0.1:8001/tr/).
  `--watch-theme` makes the preview reload on changes in `overrides/`.
- Check: `mkdocs build --strict` and then `mkdocs build --strict -f mkdocs.tr.yml` must both pass
  with zero warnings before any task is called done (English first, it cleans `site/`).
- Keep everything cross-platform: relative paths, no OS-specific scripts unless asked,
  LF line endings (add a `.gitattributes` with `* text=auto eol=lf`).
- The README must explain setup for macOS, Linux, and Windows.

## Project structure (initial, may change)

```
rezflix-help-page/
  README.md
  mkdocs.yml          English config (default)
  mkdocs.tr.yml       Turkish config, INHERIT: mkdocs.yml
  overrides/          Theme custom_dir (assets/stylesheets/rezflix.css)
  requirements.txt
  .gitignore
  .gitattributes
  .github/workflows/deploy.yml
  docs/
    en/               English pages, built to site/
      index.md
      assets/img/
    tr/               Turkish pages, same file names, built to site/tr/
      index.md
      assets/img/
```

## GitHub Pages deployment

Use the current standard approach, nothing more:

- Pages source: **GitHub Actions** (not the `gh-pages` branch, not `mkdocs gh-deploy`).
- One workflow `.github/workflows/deploy.yml`:
  - Triggers: push to `main`, and `workflow_dispatch`.
  - Permissions: `contents: read`, `pages: write`, `id-token: write`. Concurrency group `pages`.
  - Build job: checkout, setup-python with pip cache, `pip install -r requirements.txt`,
    `mkdocs build --strict`, `mkdocs build --strict -f mkdocs.tr.yml`, upload `site/` with `actions/upload-pages-artifact`.
  - Deploy job: `actions/deploy-pages`, environment `github-pages`.
  - Use the current major version of each official action.
- Custom domain `help.rezflixtv.com` is set in the repo's Pages settings. No `CNAME` file needed.
- `site_url: https://help.rezflixtv.com/` in `mkdocs.yml`, `https://help.rezflixtv.com/tr/` in
  `mkdocs.tr.yml`.

Manual steps for me (list them when relevant, do not attempt them):
1. Create the GitHub repo and push.
2. Settings > Pages > Source: GitHub Actions.
3. Add custom domain `help.rezflixtv.com`.
4. Cloudflare: CNAME `help` to `<github-username>.github.io`, DNS only (grey cloud).
5. Enable "Enforce HTTPS" once the certificate is issued.
6. Verify `rezflixtv.com` in GitHub account settings > Pages.

## Images

- Images live in the repo, in `docs/<lang>/assets/img/` or a per-section `img/` folder (I will
  decide). Each language has its own copy, since screenshots may differ by language.
- Relative paths, meaningful alt text, lazy loading:
  `![Rezflix home screen](img/home.webp){ loading=lazy }`
- Keep them small: max about 1600 px wide, compressed, WebP or optimized PNG.
- Never modify or re-encode my images without asking. Suggest compression if a file is large.

## Look and theme

- Dark and minimal, dark only (no light mode, no toggle): `palette` is a single `slate` scheme.
- Blackish background (never pure `#000`), faded grey content, and a washed dark red used only
  on small details: card icons, box icons, link underlines, hover borders, and the one filled
  button. Do not spread red to larger areas (header, titles, backgrounds).
- All overrides live in one file, `overrides/assets/stylesheets/rezflix.css`
  (`theme.custom_dir: overrides`, loaded with `extra_css`). Change colors through the `--rz-*`
  tokens at the top of that file.
- Tokens: background `#0e0e0f`, surface `#161617`, border `#26262a`, body text `#bdbdc1`,
  headings `#e6e6e8`, muted `#85858b`, link text `#d6d6d9`, red `#a8525c` (icons, underlines,
  borders), red text `#c0707a` (hover), button `#6e2f37` (hover `#803741`).
- Header and footer match the background, separated by a hairline. Material's default font.
- Restyle every admonition type the site uses (border, title, icon, arrow, focus glow), so no
  default Material colors (green, blue, teal) show through.
- Keep text contrast at WCAG AA (4.5:1) or better when changing colors.

## Languages

- English is the primary and default language, at the site root. Turkish is at `/tr/`.
- Two separate builds with Material's language switcher (`extra.alternate`), no i18n plugin, so
  the site stays compatible with Zensical.
- Every page exists in both `docs/en/` and `docs/tr/` with the same file name and nav position,
  so the switcher keeps readers on the same page. Untranslated pages get a Turkish stub.
- Write English first. Turkish headings that are linked to get the English anchor explicitly,
  for example `## Giriş yapamıyorum { #i-cannot-log-in }`, so links match in both languages.
- Turkish text written by Claude must be flagged for me to review.

## Writing rules (for the site content)

- Plain, friendly, short sentences. Lead with the answer, then details.
- Numbered steps for procedures. One action per step.
- Admonitions for tips and warnings only where they help (`!!! tip`, `!!! warning`).
- Troubleshooting entries: symptom as the heading or question, then "Try this" steps in order,
  simplest first, then "Still not working?" pointing to contact.
- Interlink generously: link to the detail page instead of repeating its content. Links to page
  sections (`page.md#heading`) are checked by the strict build (`validation: anchors: warn`).

## No emojis

No emojis anywhere in the project: no Unicode emoji characters and no emoji shortcodes such as
`:smile:`. Plain monochrome SVG icons (`:material-...:`, `:octicons-...:`) are preferred for UI
elements like cards and links; use them consistently throughout the site. Before finishing any task, check the repo for emojis and remove any found.

## No em dashes, anywhere

The em dash character (U+2014) must never appear anywhere in this project: site content, Markdown,
config, code, comments, README, workflow files, commit messages, and this file. Use a colon, comma,
parentheses, or a new sentence instead. Before finishing any task, search the repo for it and remove
any found.

## Git and commits

- Commit only when I ask. Push only when I ask.
- Commit messages: Conventional Commits style, short and professional, imperative mood, subject
  under about 60 characters, no trailing period. Add a body only when it adds real information.
  Examples: `feat: add quick start page`, `fix: correct subtitle steps`, `docs: update readme`,
  `ci: add pages deploy workflow`, `chore: pin mkdocs-material version`.
- Never mention Claude, Claude Code, Anthropic, or AI in commits, commit trailers, branch names,
  PR text, code comments, site content, or any file other than `CLAUDE.md` itself.
  No `Co-Authored-By` lines, no "Generated with" footers.
- Commits are authored by my own git identity only.

## .gitignore must include

```
.venv/
site/
__pycache__/
.cache/
.DS_Store
Thumbs.db
CLAUDE.local.md
.claude/
```

`CLAUDE.md` is committed to the repo. `CLAUDE.local.md` and the `.claude/` folder (local settings
and session data) stay local and are never committed. If either is ever tracked by git, stop and
tell me.

## Security (the repo is public)

- No secrets, tokens, passwords, API keys, private IPs, internal hostnames, or user personal data
  in any file.
- Public service URLs under `rezflixtv.com` are fine. Anything about how the homelab is built
  internally does not belong here. Ask if unsure.
- If anything that looks like a secret appears in the repo, stop and tell me.
- This applies to `CLAUDE.md` too: no credentials, private IPs, internal hostnames, server or
  homelab internals, user names or emails, personal contact details, or anything I would not post
  publicly. Describe decisions, not secrets (for example "contact method: Discord server", never
  the private invite or my personal handle unless I say it is public). Check this file for private
  information whenever you edit it.

## First task (initial scaffold, done 2026-10-12)

1. Report the current status of Material for MkDocs and propose versions to pin.
2. Create the venv, `requirements.txt`, `mkdocs.yml` (Material theme, light and dark toggle following
   system preference, search, code copy, admonitions, collapsible blocks, tabs, `attr_list`,
   glightbox), `.gitignore`, `.gitattributes`, the deploy workflow, a minimal `docs/index.md`
   placeholder, and a README.
3. Run `mkdocs build --strict` and confirm `mkdocs serve` works.
4. Check for em dashes, then make one commit on `main` (the GitHub repo already existed). Do not push.
5. List the manual GitHub and Cloudflare steps for me.

## Decisions log

Add one dated line per decision, newest last.

- 2026-10-12: Project created. Material for MkDocs, GitHub Pages via Actions, help.rezflixtv.com.
- 2026-10-12: `CLAUDE.md` is committed and public. `CLAUDE.local.md` and `.claude/` stay ignored.
- 2026-10-12: Stay on Material for MkDocs 9.7.7 (MkDocs 1.6.1) for now; revisit Zensical at beta.
- 2026-10-12: No separate New user's guide. Quick start is the concise entry point, linking to a
  tree of short interlinked detail pages.
- 2026-10-12: Landing page: intro, Watch and Request buttons, search hint, section cards, common
  problems list, contact pointer.
- 2026-10-12: No emojis anywhere. Plain Material SVG icons are used throughout.
- 2026-10-12: Landing page common problems are collapsible, with a short fix and a link.
- 2026-10-12: Turkish support at `/tr/` via two builds (`mkdocs.tr.yml`), no i18n plugin.
- 2026-10-12: Jellyfin is called "Rezflix" and Seerr "Rezflix Request" in all user-facing text.
- 2026-10-12: Theme: dark red and near-black, no pure black, custom CSS in `overrides/`.
- 2026-10-12: Landing page service buttons fill the width, side by side or stacked on phones.
- 2026-10-12: Third theme color: deep wine for header and footer, soft wine-tinted surfaces.
- 2026-10-12: Wine is the only red; the brighter red is removed. Muted gold accent under test.
- 2026-10-12: Theme reworked to sleek and minimal: warm neutrals, one soft wine accent, Inter.
- 2026-10-12: Theme simplified: Material defaults plus one wine color, warm dark mode, no Inter.
- 2026-10-12: Theme: dark only, blackish background, faded greys, washed red on small details.
- TBD: Rezflix and Rezflix Request subdomains (landing buttons link to `#` until then).
- TBD: Page names, nav structure, logo.
- TBD: Recommended client apps per device.
- TBD: Contact method.
