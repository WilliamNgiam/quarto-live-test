# Quarto Live Starter for GitHub Pages & Canvas LMS

A ready-to-deploy [Quarto](https://quarto.org/) starter project using [`r-wasm/quarto-live`](https://r-wasm.github.io/quarto-live/) to run interactive Python (`Pyodide`) and R (`webR`) code cells client-side via WebAssembly—hosted statically on **GitHub Pages** and embeddable inside **Canvas LMS** via `<iframe>`.

## Project Structure

```text
quarto-live-test/
├── .github/workflows/deploy.yml   # Auto-builds & deploys to GitHub Pages on push to main
├── _extensions/r-wasm/live/       # Quarto Live WebAssembly extension
├── _quarto.yml                    # Project configuration (website, live-html format)
├── .nojekyll                      # Prevents GitHub Pages from ignoring _extensions / *_files
├── index.qmd                      # Overview page & Canvas iframe snippet reference
└── lessons/
    ├── python-lesson.qmd          # Clean Canvas-ready Python lesson (navbar: false)
    └── r-lesson.qmd               # Clean Canvas-ready R lesson (navbar: false)
```

## Local Preview

From this directory, run:

```bash
quarto preview
```

Because each `.qmd` page sets `engine: markdown` and all code runs in the browser via WebAssembly, **no local Python or R environment is required** to render or preview the site.

## Deploying to GitHub Pages

1. **Create a new GitHub repository** named `quarto-live-test` (public, empty—no README needed).
2. **Push this project** to `main`:
   ```bash
   git add .
   git commit -m "Initial Quarto Live starter"
   git branch -M main
   git remote add origin https://github.com/<YOUR-USERNAME>/quarto-live-test.git
   git push -u origin main
   ```
3. **Configure GitHub Pages source**:
   * In your GitHub repo, go to **Settings → Pages**.
   * Under **Build and deployment → Source**, select **GitHub Actions**.
   * If the first workflow run happened before changing this setting, go to the **Actions** tab and click **Re-run jobs** (or trigger **Deploy Quarto Live to GitHub Pages → Run workflow**).

Once deployed, your site will be live at:
`https://<YOUR-USERNAME>.github.io/quarto-live-test/`

## Embedding in Canvas LMS

1. Open a Canvas Page (or Assignment/Discussion) and click **Edit**.
2. Click the **HTML Editor (`</>`)** toggle at the bottom-right of the Rich Content Editor.
3. Paste the iframe pointing to a specific lesson HTML URL (always use `https://`):

```html
<iframe
  src="https://<YOUR-USERNAME>.github.io/quarto-live-test/lessons/python-lesson.html"
  width="100%"
  height="750px"
  style="border: 1px solid #e0e0e0; border-radius: 8px;"
  title="Interactive Python Lesson">
</iframe>
<p style="font-size: 0.9em; margin-top: 0.4em;">
  <a href="https://<YOUR-USERNAME>.github.io/quarto-live-test/lessons/python-lesson.html" target="_blank" rel="noopener">
    Open interactive activity in a new tab ↗
  </a>
</p>
```

### Canvas & Browser Notes

* **Cross-Origin Iframe Compatibility:** `quarto-live` runs Pyodide in a dedicated Web Worker and initializes webR in `Automatic` channel mode (gracefully falling back to `postMessage` when `SharedArrayBuffer` headers are absent on GitHub Pages / Canvas iframes—no Service Worker hacks needed).
* **Distraction-Free Layout:** Files inside `lessons/` set `navbar: false` and `toc: false` so Canvas pages don't show nested site navigation bars.
* **Institution CSP:** If an iframe saves in Canvas HTML view but displays blank/blocked, ask your Canvas administrator to ensure `*.github.io` is permitted in your institution's Canvas Content Security Policy (CSP) allowlist.
