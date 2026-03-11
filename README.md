# CoReSSD Documentation

This repository contains the MkDocs source for the **CoReSSD** (*Co*ld *Re*gions *S*nowfall and *S*nowpack *D*ataset) documentation site, built with the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.

## Building Locally

1. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

2. Serve the site locally with live reload:

   ```bash
   mkdocs serve
   ```

   Then open <http://127.0.0.1:8000> in your browser.

3. Build static HTML:

   ```bash
   mkdocs build
   ```

   Output is written to the `site/` directory.

## Deploying

The site is automatically built and deployed to GitHub Pages on every push to `main` via the GitHub Actions workflow in `.github/workflows/deploy.yml`.
