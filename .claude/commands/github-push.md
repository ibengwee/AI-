You are performing a full GitHub deployment sequence. Follow each step in order.

## Step 1 — Generate or update README.md

Read the project files to understand what the app does, then write a professional `README.md` that includes:
- **Project name and one-line description** (infer from the code)
- **Features** — bullet list of what the app does
- **Tech stack** — languages, APIs, and services used
- **Setup instructions** — how to open/run the app locally (no build step if it's a static HTML app)
- **Screenshots** — a placeholder section with a note to add screenshots
- **Live demo link** — infer the GitHub Pages URL from the git remote (format: `https://{username}.github.io/{repo}/`)

Write the file even if a README.md already exists — overwrite it with the improved version.

## Step 2 — Create or update GitHub Actions deploy workflow

Write the file `.github/workflows/deploy.yml` with this exact content (do not modify it):

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to gh-pages branch
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: .
          exclude_assets: '.github,CLAUDE.md,README.md'
```

## Step 3 — Stage and commit all changes

Run the following in sequence:
1. `git add -A`
2. Inspect what changed with `git diff --cached --stat`
3. Write a concise, descriptive commit message that summarises the staged changes
4. Commit with that message (include `Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>` trailer)

If there is nothing to commit, skip this step and note it.

## Step 4 — Push to GitHub

Run `git push origin main`.

If the push fails due to no upstream set, run `git push -u origin main`.

If it fails due to authentication, stop and tell the user: "Push failed — run `ssh -T git@github.com` to verify your SSH key is working, then try again."

## Step 5 — Enable GitHub Pages via gh CLI

Run:
```
gh api repos/{owner}/{repo}/pages \
  --method POST \
  -f build_type=workflow \
  --silent 2>/dev1/null || true
```

Where `{owner}` and `{repo}` are inferred from `git remote get-url origin`.

If the `gh` command is not found, skip this step and tell the user to enable Pages manually: **Repo Settings → Pages → Source → GitHub Actions → Save**.

## Step 6 — Update repo description and topics via gh CLI

Run:
```
gh repo edit --description "{one-line description of the project}" --add-topic "apify" --add-topic "html" --add-topic "javascript"
```

Infer the description from the project. Add relevant topics based on the tech stack (e.g. `apify`, `html`, `javascript`, `github-pages`, `price-comparison`, `lead-generation`).

If `gh` is not found, skip this step silently.

## Step 7 — Print the live URL

Infer the GitHub Pages URL from the remote: `https://{username}.github.io/{repo}/`

Print:

```
✅ Done! Your app will be live at:
https://{username}.github.io/{repo}/

The GitHub Actions workflow is deploying now.
Check progress at: https://github.com/{owner}/{repo}/actions
```

If any step failed, summarise what succeeded and what needs manual action.
