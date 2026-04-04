# Documentation Pipeline Architecture

## Flow

Author → GitHub → CI/CD → Build → Deploy → Live Docs

---

## Explanation

1. Content is written in Markdown
2. Pushed to GitHub repository
3. GitHub Actions triggers build
4. MkDocs generates static site
5. Deployed to GitHub Pages
