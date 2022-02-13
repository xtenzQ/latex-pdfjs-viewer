# LaTeX-to-PDF with GitHub Pages deployment

[![CI](https://github.com/xtenzQ/cv/actions/workflows/blank.yml/badge.svg)](https://github.com/xtenzQ/cv/actions/workflows/blank.yml) [![pages-build-deployment](https://github.com/xtenzQ/cv/actions/workflows/pages/pages-build-deployment/badge.svg?branch=main)](https://github.com/xtenzQ/cv/actions/workflows/pages/pages-build-deployment)

Builds LaTeX doc into PDF and then publishes it on GitHub pages so we can now embed our PDF with the following link:
```
https://<username>.github.io/<repo>/
```
**[Example](https://rusetskii.dev/cv)**

## Structure

- `pdfjs` - contains PDF.js viewer built from sources
- `doc.tex` - the LaTeX document to be published

## Deployment

Using GitHub Actions, LaTeX document is converted into PDF and deployed to GitHub Pages together with PDF.js viewer.
