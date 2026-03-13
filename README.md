# David Bueno — Personal Portfolio

Personal portfolio and blog built with [Hugo](https://gohugo.io) using a custom **Mana** theme.

🌐 **Live site:** https://davidbuenonnoleto.github.io

---

## 🚀 Quick Start (Local Development)

**Prerequisites:** [Hugo Extended](https://gohugo.io/installation/) v0.110+

```bash
# Clone the repo
git clone https://github.com/davidbuenonnoleto/davidbuenonnoleto.github.io.git
cd davidbuenonnoleto.github.io

# Start dev server
hugo server -D

# Visit http://localhost:1313
```

---

## 📝 Adding Content

### New Blog Post

```bash
hugo new posts/my-post-title.md
```

Then edit `content/posts/my-post-title.md` — set `draft: false` when ready to publish.

### New Project

```bash
hugo new projects/my-project.md
```

---

## 🏗️ Project Structure

```
.
├── .github/workflows/deploy.yml   # Auto-deploy on push to main
├── content/
│   ├── about.md                   # About page
│   ├── posts/                     # Blog posts
│   └── projects/                  # Project showcase
├── themes/hugo-mana-theme/        # Mana theme (bundled)
│   ├── layouts/                   # HTML templates
│   └── static/css/main.css        # All styles
└── hugo.toml                      # Site configuration
```

---

## ⚙️ Configuration

Edit `hugo.toml` to update:

- `title` — site name
- `params.author` — your name
- `params.description` — tagline
- `params.github` — GitHub profile URL
- `params.email` — contact email

---

## 🚢 Deployment

Deployment is fully automated via GitHub Actions (`.github/workflows/deploy.yml`).

**One-time setup:**
1. Go to your repo → **Settings → Pages**
2. Set **Source** to `GitHub Actions`
3. Push to `main` — the site deploys automatically ✅

The workflow:
1. Installs Hugo Extended
2. Runs `hugo --minify` to build
3. Deploys the `public/` directory to GitHub Pages

---

## 📄 License

MIT
