# JPPlayerVaults Documentation

Source of the JPPlayerVaults documentation site, built with [MkDocs](https://www.mkdocs.org/) +
[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

Every page is a plain Markdown file under `docs/` — the whole site is yours to edit.

## Preview locally

Requires Python 3.

```bash
py -m pip install -r requirements.txt
mkdocs serve
```

Open <http://127.0.0.1:8000> — the site reloads live while you edit files in `docs/`.

## Publish on GitHub Pages (free)

Everything is prepared for `https://github.com/Nameeees/jpvaults-docs`: the site links in `mkdocs.yml`
and the local `origin` remote already point there.

1. Create the **public** repository `jpvaults-docs` on the `Nameeees` account — **without** adding a
   README, .gitignore or license (keep it completely empty).
2. Push this folder:

   ```bash
   git push -u origin main
   ```

3. In the repository: **Settings → Pages → Build and deployment → Source: GitHub Actions**.
4. Done. Every push to `main` rebuilds and publishes the site at
   <https://nameeees.github.io/jpvaults-docs/>.

If you ever rename the repository or move it to another account, update the three URLs at the top of
`mkdocs.yml` and the git remote (`git remote set-url origin https://github.com/<user>/<repo>.git`).

## Editing later

- **From GitHub**: open any file under `docs/`, click the pencil (Edit) button and commit — the site
  rebuilds automatically. The “edit this page” link on every page takes you straight there.
- **Locally**: edit `docs/*.md`, check with `mkdocs serve`, push.
- Run `mkdocs build --strict` before pushing if you want the same check the deployment uses: it fails
  on broken internal links and on pages missing from the navigation.

## Structure

```text
mkdocs.yml            site configuration (theme, navigation, social links)
requirements.txt      Python dependencies
docs/                 all the content (Markdown)
  index.md            home with the feature cards
  getting-started/    installation, configuration, languages
  features.md         full feature tour
  reference/          commands, permissions
  data-safety.md      storage, protections, known limits
  compatibility.md    tested matrix and how one jar covers every version
  faq.md              frequent questions
  changelog.md        release notes
  assets/             logo, favicon, banner and extra.css
.github/workflows/    automatic deployment to GitHub Pages
```

## Notes

- Never edit anything inside `site/` — that folder is generated output.
- The content is maintained alongside the plugin: `README.md`, `CHANGELOG.md` and `docs/` in the
  plugin repository hold the same information, so update both when something changes.
- The Discord invite used by the social icon is the community server
  (<https://discord.gg/HATd9QgukF>).
