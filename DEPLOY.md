# Deploy `flickme-legal` to GitHub Pages

This folder is a complete, self-contained static site. Push it to a public GitHub repo and GitHub will host it for free at:

```
https://will-miraglia.github.io/flickme-legal/
```

…with these specific URLs (which all 9 docs in `appstore-submission/` already reference):

- `https://will-miraglia.github.io/flickme-legal/privacy.html`
- `https://will-miraglia.github.io/flickme-legal/terms.html`
- `https://will-miraglia.github.io/flickme-legal/community-guidelines.html`
- `https://will-miraglia.github.io/flickme-legal/support.html`

---

## One-time setup (~10 minutes)

### 1. Create the GitHub repo

1. Go to https://github.com/new
2. **Repository name:** `flickme-legal` (must be exactly this — the URL depends on it)
3. **Visibility:** Public *(required for free GitHub Pages)*
4. Do **not** initialize with README or .gitignore — we'll push them
5. Click **Create repository**

### 2. Push this folder

Open Terminal at this folder and run (replace nothing — these commands are correct as written):

```bash
cd /Users/willmiraglia/Documents/Work/FlickMe/flickme-legal
git init
git add .
git commit -m "Initial commit: FlickMe legal pages"
git branch -M main
git remote add origin https://github.com/will-miraglia/flickme-legal.git
git push -u origin main
```

If you've never used Git on this machine, the push will prompt you to authenticate. The easiest path is to use the **GitHub CLI**: `brew install gh`, then `gh auth login`, then re-run `git push`.

### 3. Turn on GitHub Pages

1. On github.com, go to your new `flickme-legal` repo
2. Click **Settings** (top right)
3. In the left sidebar, click **Pages**
4. Under **Source**, choose: **Deploy from a branch**
5. Under **Branch**, choose: `main` and `/ (root)` and click **Save**
6. Wait ~30–60 seconds. The page will show a green box with your live URL.

### 4. Verify

Open these URLs in your browser. All four must load without error:

- https://will-miraglia.github.io/flickme-legal/
- https://will-miraglia.github.io/flickme-legal/privacy.html
- https://will-miraglia.github.io/flickme-legal/terms.html
- https://will-miraglia.github.io/flickme-legal/community-guidelines.html
- https://will-miraglia.github.io/flickme-legal/support.html

If any 404s, double-check the repo is public, the branch is `main`, and the source is `/ (root)`.

---

## Updating the pages later

When you change any of the legal documents in `appstore-submission/*.md`, regenerate the HTML and push:

```bash
# From the FlickMe folder root:
python3 - <<'EOF'
import subprocess
subprocess.run(["python3", "/path/to/build_site.py"])  # see appstore-submission/scripts if regenerated
EOF

cd flickme-legal
git add .
git commit -m "Update legal pages"
git push
```

GitHub Pages auto-rebuilds within a minute of push. You do **not** need to resubmit the iOS app to the App Store — the URL is fixed; only the contents at the URL change.

---

## Why these specific URLs

Every reference in `appstore-submission/*.md` (privacy policy URL field in App Store Connect, in-app browser links, the App Store description, the App Review notes, the EULA checkbox copy in the registration screen) points at this exact URL pattern. **Don't change the repo name** — if you do, you'll have to find-and-replace across every doc again.

If you later move to a custom domain (e.g. `flickme.app`):
1. Buy the domain
2. In the GitHub repo Settings → Pages, add a "Custom domain"
3. Add a `CNAME` file to this folder containing just the domain
4. Update the URLs across the `appstore-submission/` docs (one find-and-replace)

---

## What's in this folder

| File | What it is |
|------|-----------|
| `index.html` | Public homepage at root URL — links to all the legal docs |
| `privacy.html` | Generated from `appstore-submission/01_PRIVACY_POLICY.md` |
| `terms.html` | Generated from `appstore-submission/02_TERMS_OF_SERVICE.md` |
| `community-guidelines.html` | Generated from `appstore-submission/03_COMMUNITY_GUIDELINES.md` |
| `support.html` | Stand-alone support page with FAQ + email |
| `404.html` | Custom 404 page |
| `DEPLOY.md` | This file (won't be served — Markdown isn't auto-rendered by Pages without Jekyll) |

CSS and structure are inlined in each HTML file — there's no build step, no dependencies, and nothing external to break.
