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

### Installation

```bash
# Install dependencies
pip install -r requirements.txt

# Or install manually
pip install mkdocs mkdocs-material mkdocs-blog-plugin
```

### Local Development

```bash
# Serve the site locally with live reload
mkdocs serve

# Access at http://127.0.0.1:8000
```

### Build

```bash
# Build the static site
mkdocs build

# Output will be in the ./site directory
```

## Deployment

### GitHub Pages

The site is configured to automatically deploy to GitHub Pages when changes are pushed to the `main` branch. The workflow is defined in `.github/workflows/deploy.yml`.

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
