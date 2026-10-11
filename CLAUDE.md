# Rezflix Help

Help center for **Rezflix**, my private media streaming service. It is the first place my users go
when they need help, before contacting me directly.

- **Site URL:** https://help.rezflixtv.com
- **Hosting:** GitHub Pages, deployed with GitHub Actions on every push to `main`
- **DNS:** Cloudflare (managed by me)
- **Generator:** Material for MkDocs
- **Rezflix services** follow the pattern `[service].rezflixtv.com`:
  - **Jellyfin:** the streaming server (watching movies and TV shows)
  - **Seerr:** where users request new movies and TV shows
  - Exact subdomains: to be confirmed by me (see "Decisions log")

## Audience

Rezflix users: friends, family, and invited people. Mostly non-technical. They watch on TVs, phones,
browsers, and streaming devices. Write for them, not for developers.

## Planned content (structure not final)

These are the areas the site must cover. Page names, order, wording, look, and images are decided
by me during development. Until I decide, use simple placeholders and do not invent final content.

- **Quick start:** the fastest path from invite to watching.
- **New user's guide:** what Rezflix is, the watch link and the request link, how to log in to each,
  recommended client apps per device, how to use Jellyfin and how to request in Seerr.
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

- Preview: `mkdocs serve` (http://127.0.0.1:8000)
- Check: `mkdocs build --strict` must pass with zero warnings before any task is called done.
- Keep everything cross-platform: relative paths, no OS-specific scripts unless asked,
  LF line endings (add a `.gitattributes` with `* text=auto eol=lf`).
- The README must explain setup for macOS, Linux, and Windows.

## Project structure (initial, may change)

```
rezflix-help-page/
  README.md
  mkdocs.yml
  requirements.txt
  .gitignore
  .gitattributes
  .github/workflows/deploy.yml
  docs/
    index.md
    assets/
      img/
    (other pages to be decided)
```

## GitHub Pages deployment

Use the current standard approach, nothing more:

- Pages source: **GitHub Actions** (not the `gh-pages` branch, not `mkdocs gh-deploy`).
- One workflow `.github/workflows/deploy.yml`:
  - Triggers: push to `main`, and `workflow_dispatch`.
  - Permissions: `contents: read`, `pages: write`, `id-token: write`. Concurrency group `pages`.
  - Build job: checkout, setup-python with pip cache, `pip install -r requirements.txt`,
    `mkdocs build --strict`, upload `site/` with `actions/upload-pages-artifact`.
  - Deploy job: `actions/deploy-pages`, environment `github-pages`.
  - Use the current major version of each official action.
- Custom domain `help.rezflixtv.com` is set in the repo's Pages settings. No `CNAME` file needed.
- `site_url: https://help.rezflixtv.com/` in `mkdocs.yml`.

Manual steps for me (list them when relevant, do not attempt them):
1. Create the GitHub repo and push.
2. Settings > Pages > Source: GitHub Actions.
3. Add custom domain `help.rezflixtv.com`.
4. Cloudflare: CNAME `help` to `<github-username>.github.io`, DNS only (grey cloud).
5. Enable "Enforce HTTPS" once the certificate is issued.
6. Verify `rezflixtv.com` in GitHub account settings > Pages.

## Images

- Images live in the repo, in `docs/assets/img/` or a per-section `img/` folder (I will decide).
- Relative paths, meaningful alt text, lazy loading:
  `![Jellyfin home screen](img/home.webp){ loading=lazy }`
- Keep them small: max about 1600 px wide, compressed, WebP or optimized PNG.
- Never modify or re-encode my images without asking. Suggest compression if a file is large.

## Writing rules (for the site content)

- Plain, friendly, short sentences. Lead with the answer, then details.
- Numbered steps for procedures. One action per step.
- Admonitions for tips and warnings only where they help (`!!! tip`, `!!! warning`).
- Troubleshooting entries: symptom as the heading or question, then "Try this" steps in order,
  simplest first, then "Still not working?" pointing to contact.

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
- TBD: Jellyfin and Seerr subdomains.
- TBD: Page names, nav structure, look and colors, logo.
- TBD: Recommended client apps per device.
- TBD: Contact method.
