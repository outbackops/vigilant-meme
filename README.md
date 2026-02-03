# Vigilant Meme

A modern, responsive, and accessible website built with MkDocs and Material theme, featuring gradient color themes and support for light/dark modes.

## Features

- ✨ **Responsive Design**: Works seamlessly on all devices
- ♿ **Accessible**: WCAG 2.1 compliant with proper semantic HTML
- 🌓 **Light/Dark Mode**: Toggle between themes with preference persistence
- 🎨 **Gradient Color Theme**: Beautiful gradient designs throughout
- 🖼️ **Hero Images**: Eye-catching gradient hero images on all pages
- 📝 **Blog Support**: Easy-to-add blog posts with markdown
- 📞 **Contact Page**: Ready-to-use contact form template

## Pages

- **Home**: Welcome page with features overview
- **Blog**: Collection of posts with easy navigation
- **Contact**: Contact form and information

## Quick Start

### Prerequisites

- Python 3.8 or higher
- pip

> **Windows Users**: Use `py -3` instead of `python` if `python` is not recognized.

### Installation

```bash
# Windows (recommended)
py -3 -m pip install -r requirements.txt

# macOS/Linux
pip install -r requirements.txt

# Or install manually
pip install mkdocs mkdocs-material mkdocs-blog-plugin
```

### Local Development

```bash
# Windows
py -3 -m mkdocs serve

# macOS/Linux
mkdocs serve

# Access at http://127.0.0.1:8000
```

### Build

```bash
# Windows
py -3 -m mkdocs build

# macOS/Linux
mkdocs build

# Output will be in the ./site directory
```

## Deployment

### GitHub Pages (Recommended)

**Option 1: One-command deploy**

This builds the site and pushes to the `gh-pages` branch automatically:

```bash
# Windows
py -3 -m mkdocs gh-deploy --force

# macOS/Linux
mkdocs gh-deploy --force
```

Then enable GitHub Pages in your repo:
1. Go to **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: `gh-pages` / `/ (root)`
4. Click **Save**

Your site will be live at `https://<username>.github.io/<repo-name>/`

**Option 2: GitHub Actions (automatic)**

Push to `main` branch and the workflow in `.github/workflows/deploy.yml` will build and deploy automatically.

```bash
git add .
git commit -m "Your commit message"
git push origin main
```

### Azure Static Web Apps

To deploy to Azure Static Web Apps:

1. Create an Azure Static Web App resource
2. Configure the build settings:
   - App location: `/`
   - Output location: `site`
   - Build command: `pip install -r requirements.txt && mkdocs build`

## Customization

### Colors

Edit `mkdocs.yml` to customize the color scheme:

```yaml
theme:
  palette:
    - scheme: default
      primary: indigo  # Change primary color
      accent: purple   # Change accent color
```

### Hero Images

Replace SVG files in `docs/images/`:
- `hero-home.svg`
- `hero-blog.svg`
- `hero-contact.svg`

### Adding Blog Posts

1. Create a new markdown file in `docs/blog/posts/`
2. Update `docs/blog/index.md` to link to the new post
3. Build and deploy

## License

See [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
