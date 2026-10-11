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
| Live preview at <http://127.0.0.1:8000> | `mkdocs serve` |
| Build the site (must pass with zero warnings) | `mkdocs build --strict` |
| Leave the virtual environment | `deactivate` |

## Project layout

```text
mkdocs.yml                   Site configuration
requirements.txt             Pinned Python dependencies
docs/                        Site content (Markdown)
docs/assets/img/             Images
.github/workflows/deploy.yml Build and deploy to GitHub Pages
```

## Deployment

Pushing to `main` builds the site with `mkdocs build --strict` and publishes it to GitHub Pages.
A failed build is never deployed. The workflow can also be started by hand from the Actions tab.
