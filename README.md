<p align="center">⚜️ VITE DEPLOY ⚜️</p>

Below are the steps to deploy a Vite React app.

💻 Deployment
<hr>

01. Create a vite react app

<div className="p-10 bg-slate-300">
        <h1>npm create vite@latest</h1>
</div>

02. Set the base on vite.config

<div className="p-10 bg-slate-300">
        <h1>base: "/[REPO_NAME]/"</h1>
</div>

03. Create ./github/workflows/deploy.yml and add the code bellow

 name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 16

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v2
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v2
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist


04. Create a new repository on GitHub and initialize GIT

<div>
        <h1>git init</h1>
        <h1>git add .</h1>
        <h1>git commit -m "add: initial files"</h1>
        <h1>git branch -M main</h1>
        <h1>git remote add origin https://github.com/[USER]/[REPO_NAME]</h1>
        <h1>git push -u origin main</h1>
</div>

05. Active workflow

<div>
        <h1>Config -> Actions -> General -> Workflow permissions -> Read and Write permissions</h1>
        <h1>Actions -> failed deploy -> re-run-job failed jobs</h1>
        <h1>Config -> Pages -> gh-pages -> save</h1>
</div>
