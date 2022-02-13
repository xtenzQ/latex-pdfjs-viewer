# LaTeX-to-PDF with GitHub Pages deployment

[![CI](https://github.com/xtenzQ/cv/actions/workflows/blank.yml/badge.svg)](https://github.com/xtenzQ/cv/actions/workflows/blank.yml) [![pages-build-deployment](https://github.com/xtenzQ/cv/actions/workflows/pages/pages-build-deployment/badge.svg?branch=gh-pages)](https://github.com/xtenzQ/cv/actions/workflows/pages/pages-build-deployment)

⚠ **Please delete `CNAME` from `pdfjs` folder and `gh-pages` branch after forking!** ⚠

Builds LaTeX doc into PDF and then publishes it on GitHub Pages so we can now embed our PDF with the following link:
```
https://<username>.github.io/<repo>/
```
**[Example](https://rusetskii.dev/cv)**

## Structure

- `pdfjs` - contains PDF.js viewer built from sources
- `doc.tex` - the LaTeX document to be published

## Deployment

Using GitHub Actions, LaTeX document is converted into PDF and deployed to GitHub Pages together with PDF.js viewer.

## Building from scratch

Want to build your own converter with viewer?
1. Clone PDF.JS
```
$ git clone https://github.com/mozilla/pdf.js.git
$ cd pdf.js
```
2. Install dependencies
```
$ npm install
```
3. Build from sources
```
$ gulp generic
```
4. Copy built sources to your repo's `pdfjs` folder
```
$ cp -r build/generic <your repo>/pdfjs
```
5. Create `index.html` in your repo on the path `<your repo>/pdfjs`
```HTML
<!DOCTYPE html>
<html dir="ltr" mozdisallowselectionprint>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <meta name="google" content="notranslate">
  </head>
  <body style="height: 100%; width: 100%; overflow: hidden; margin:0px; background-color: rgb(82, 86, 89);">
    <iframe src="web/viewer.html" title="CV" frameBorder="0" style="position:absolute; left: 0; top: 0;" width="100%" height="100%">
  </body>
</html>
```
6. Put your LaTeX file in the root of your repo and rename it to `doc.tex`.
7. Create GitHub Actions workflow `build.yml` on the path `.github\workflows`:
```YML
name: LaTeX-to-PDF

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]
    paths:
      - 'doc.tex'
      - 'pdfjs/**'
      - '.github/workflows/blank.yml'
  pull_request:
    branches: [ main ]
    paths:
      - 'doc.tex'

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  build_latex:
    runs-on: ubuntu-latest
    steps:
      - name: Set up Git repository
        uses: actions/checkout@v2
        
      - name: Compile LaTeX document
        uses: xu-cheng/latex-action@v2
        with:
          root_file: doc.tex
          
      - name: Copy file
        run: |
          sudo mv doc.pdf pdfjs

      - name: Deploy to gh-pages
        uses: s0/git-publish-subdir-action@develop
        env:
          REPO: self
          BRANCH: gh-pages
          FOLDER: pdfjs
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          MESSAGE: "deploy website"
```
8. Set your GitHub Pages in Settings to the branch `gh-pages`.
