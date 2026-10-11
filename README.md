# Rezflix Help

Source for the Rezflix help center at <https://help.rezflixtv.com>.

Built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and deployed to
GitHub Pages by GitHub Actions on every push to `main`.

## Requirements

- Python 3.9 or newer
- Git

## Setup

Create a virtual environment in the project folder and install the pinned dependencies.
Everything stays inside `.venv/`, nothing is installed globally.

### macOS and Linux

```sh
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Windows (PowerShell)

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

If PowerShell blocks the activation script, run this once and try again:

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

## Usage

With the virtual environment active:

| Task | Command |
| ---- | ------- |
| Live preview, English, at <http://127.0.0.1:8000> | `mkdocs serve --watch-theme` |
| Live preview, Turkish, at <http://127.0.0.1:8001/tr/> | `mkdocs serve --watch-theme -f mkdocs.tr.yml -a 127.0.0.1:8001` |
| Build both languages (must pass with zero warnings) | `mkdocs build --strict` then `mkdocs build --strict -f mkdocs.tr.yml` |
| Leave the virtual environment | `deactivate` |

Build English first: it cleans `site/`, and the Turkish build then writes into `site/tr/`.

The language switcher only works when both languages are served together. To test it, build
both languages, then run `python -m http.server 8000 -d site` and open <http://127.0.0.1:8000>.

## Languages

English is the default language and lives at the site root. Turkish lives under `/tr/`.

- English pages: `docs/en/`, configured by `mkdocs.yml`
- Turkish pages: `docs/tr/`, configured by `mkdocs.tr.yml` (inherits `mkdocs.yml`)

Every page exists in both languages with the same file name, so the language switcher can keep
the reader on the same page.

## Project layout

```text
mkdocs.yml                   Site configuration (English)
mkdocs.tr.yml                Turkish overrides
requirements.txt             Pinned Python dependencies
docs/en/                     English content (Markdown)
docs/tr/                     Turkish content (Markdown)
docs/<lang>/assets/img/      Images
.github/workflows/deploy.yml Build and deploy to GitHub Pages
```

## Deployment

Pushing to `main` builds both languages in strict mode and publishes the site to GitHub Pages.
A failed build is never deployed. The workflow can also be started by hand from the Actions tab.
