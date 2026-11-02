# IT-Hub

A place where I keep IT snippets, tutorials, practice exercises and resources.

Site: <https://surikata3k.github.io/it-hub> (built with [Zensical](https://zensical.org)).

## Edit locally

```bash
python -m venv .venv
.venv\Scripts\Activate.ps1        # Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
zensical serve                    # preview at http://localhost:8000
```

If `zensical` is not found, run it through Python: `python -m zensical serve`.

## Add content

Pages live in `docs/`, one folder per section. The navigation is generated automatically
from the folder structure, so a new `.md` file appears in the menu without touching
`zensical.toml`.

## Publish

Every push to `main` builds and publishes the site through `.github/workflows/docs.yml`.
The Zensical version is pinned in `requirements.txt`; update it there when upgrading.
