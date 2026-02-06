# Data Model: Vigilant Meme Feature Specification Standards

**Feature**: 001-spec-template  
**Date**: 2026-02-06  
**Purpose**: Define entities, their attributes, and relationships

## Entities

### Blog Post

A Markdown file representing an article in the blog section.

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| title | Markdown H1 | Yes | First `# Heading` in file becomes post title |
| date | Text | Yes | Published date in format `**Published: YYYY-MM-DD**` |
| author | Text | Yes | Author name in format `**Author: Name**` |
| content | Markdown | Yes | Body content with sections, admonitions, code blocks |
| hero_image | HTML/SVG | No | Optional `<div class="hero-container">` with image |
| admonitions | Markdown | No | Material admonitions (`!!! type "Title"`) |

**File Location**: `docs/blog/posts/{slug}.md`  
**Naming Convention**: Lowercase, hyphen-separated slug (e.g., `getting-started.md`)

**Relationships**:
- Listed in: `docs/blog/index.md` (manual link entry)
- Uses images from: `docs/images/`

### Page

A top-level Markdown file representing a main section of the site.

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| title | Markdown H1 | Yes | First `# Heading` in file |
| hero_image | HTML/SVG | No | Optional hero container |
| content | Markdown | Yes | Page body content |

**File Location**: `docs/{name}.md`  
**Current Pages**: `index.md` (Home), `contact.md` (Contact)

**Relationships**:
- Defined in navigation: `mkdocs.yml` under `nav:`
- Blog index: `docs/blog/index.md` is a hybrid (page + post listing)

### Hero Image

An SVG graphic displayed prominently at the top of pages.

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| file | SVG | Yes | Vector graphic file |
| alt_text | String | Yes | Descriptive text for accessibility |
| container | HTML | Yes | Wrapped in `<div class="hero-container">` |

**File Location**: `docs/images/hero-{page}.svg`  
**Current Images**: `hero-home.svg`, `hero-blog.svg`, `hero-contact.svg`

**Relationships**:
- Referenced by: Page or Blog Post via HTML embed
- Styled by: `.hero-container` rules in `extra.css`

### CSS Variable

A custom property controlling visual styling.

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| name | CSS identifier | Yes | Variable name (e.g., `--gradient-primary`) |
| light_value | CSS value | Yes | Value in `:root` selector |
| dark_value | CSS value | Yes | Value in `[data-md-color-scheme="slate"]` |
| usage | Description | Yes | Where the variable is applied |

**File Location**: `docs/stylesheets/extra.css`

**Current Variables**:

| Variable | Light Mode | Dark Mode | Usage |
|----------|------------|-----------|-------|
| `--md-primary-fg-color` | `#667eea` | `#9b6dff` | Primary foreground |
| `--gradient-primary` | `linear-gradient(135deg, #667eea 0%, #764ba2 100%)` | `linear-gradient(135deg, #7c3aed 0%, #a855f7 100%)` | Headers, primary actions |
| `--gradient-accent` | `linear-gradient(135deg, #f093fb 0%, #f5576c 100%)` | (inherits) | Highlights, secondary |
| `--gradient-info` | `linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)` | (inherits) | Informational sections |

### Site Configuration

The central configuration file for the MkDocs site.

| Section | Purpose | Key Settings |
|---------|---------|--------------|
| `site_*` | Metadata | name, description, author, url |
| `theme` | Material config | palette, features, font, icons |
| `nav` | Navigation structure | Page hierarchy and ordering |
| `markdown_extensions` | Markdown features | pymdownx, admonition, toc |
| `plugins` | Site plugins | search, (blog) |
| `extra_css` | Custom styling | Path to extra.css |

**File Location**: `mkdocs.yml` (repository root)

## Entity Relationship Diagram

```text
┌─────────────────┐     references     ┌─────────────────┐
│   Blog Post     │───────────────────▶│   Hero Image    │
│                 │                    │                 │
│ - title         │                    │ - file (SVG)    │
│ - date          │                    │ - alt_text      │
│ - author        │                    └─────────────────┘
│ - content       │                            ▲
└─────────────────┘                            │
        │                                      │
        │ linked from                 references
        ▼                                      │
┌─────────────────┐                    ┌───────┴─────────┐
│ Blog Index Page │                    │      Page       │
│ (blog/index.md) │                    │                 │
└─────────────────┘                    │ - title         │
        │                              │ - content       │
        │ listed in                    └─────────────────┘
        ▼                                      │
┌─────────────────┐                            │
│ Site Config     │◀───────────────────────────┘
│ (mkdocs.yml)    │      defined in nav
│                 │
│ - nav structure │
│ - theme config  │
│ - extensions    │
└─────────────────┘
        │
        │ imports
        ▼
┌─────────────────┐
│   extra.css     │
│                 │
│ - CSS Variables │
│ - Hero styles   │
│ - Responsive    │
└─────────────────┘
```

## Validation Rules

### Blog Post Validation
- Title (H1) MUST appear within first 5 lines
- Date MUST follow format `**Published: YYYY-MM-DD**`
- All images MUST have non-empty alt text
- Internal links MUST use relative paths

### CSS Variable Validation
- Variable names MUST start with `--`
- Light mode values MUST be defined in `:root`
- Dark mode values MUST be defined in `[data-md-color-scheme="slate"]`

### Configuration Validation
- All `nav:` entries MUST reference existing .md files
- All plugins MUST be listed in `requirements.txt`
- `mkdocs build --strict` MUST pass without warnings
