<!--
SYNC IMPACT REPORT
==================
Version change: N/A → 1.0.0 (initial creation)
Modified principles: N/A (initial creation)
Added sections:
  - Core Principles (5 principles)
  - Technical Constraints
  - File Structure & Conventions
  - Governance
Removed sections: N/A
Templates requiring updates:
  - .specify/templates/plan-template.md ✅ aligned (static site context)
  - .specify/templates/spec-template.md ✅ aligned (user story format compatible)
  - .specify/templates/tasks-template.md ✅ aligned (phase structure applicable)
Follow-up TODOs: None
-->

# Vigilant Meme Constitution

A static documentation and blog site serving as an AI tools encyclopedia with modern, accessible design.

## Core Principles

### I. Accessibility First

All content and design decisions MUST prioritize accessibility:

- WCAG 2.1 Level AA compliance is mandatory
- Semantic HTML structure required for all pages
- All images MUST have descriptive alt text
- Keyboard navigation MUST work for all interactive elements
- Color contrast ratios MUST meet WCAG AA minimums (4.5:1 for text, 3:1 for UI)
- Screen reader compatibility MUST be verified before deployment

**Rationale**: Everyone should be able to access content regardless of ability.

### II. Responsive Design

Mobile-first approach with progressive enhancement:

- Base styles target mobile devices first
- Three breakpoints MUST be supported:
  - Mobile: < 768px
  - Tablet: 768px - 1024px
  - Desktop: > 1024px
- All content MUST be readable and navigable at every breakpoint
- Touch targets MUST be at least 44x44px on mobile

**Rationale**: Users access content from diverse devices; experience must be consistent.

### III. Static-First Performance

Minimize runtime dependencies and maximize static generation:

- No backend or server-side code permitted
- JavaScript MUST be minimal and non-blocking
- All pages MUST be pre-rendered at build time
- External dependencies MUST be justified and minimal
- Page load performance target: < 3s on 3G connection

**Rationale**: Static sites are faster, more secure, and simpler to deploy.

### IV. Content Quality

All content MUST follow consistent structure and standards:

- Blog posts MUST include: Title, Date, Author metadata
- Technical content SHOULD include ELI5 (Explain Like I'm 5) summaries
- Use Material admonitions consistently: `!!! info`, `tip`, `example`, `success`, `warning`
- Internal links MUST use relative Markdown format: `[Text](posts/filename.md)`
- Hero images MUST use the `<div class="hero-container">` wrapper

**Rationale**: Consistent structure improves readability and maintainability.

### V. Visual Consistency

Maintain cohesive visual design through the gradient color system:

- CSS variables MUST be used for all colors (no hardcoded values)
- Three gradient types are defined:
  - `--gradient-primary`: Purple to indigo (headers, primary actions)
  - `--gradient-accent`: Pink to rose (highlights, secondary elements)
  - `--gradient-info`: Blue to cyan (informational sections)
- Light mode and dark mode variants MUST both be styled
- Dark mode uses `[data-md-color-scheme="slate"]` selector

**Rationale**: Visual consistency reinforces brand and improves user experience.

## Technical Constraints

Technology stack boundaries that MUST NOT be violated:

| Component | Requirement | Notes |
|-----------|-------------|-------|
| Generator | MkDocs 1.6+ | Static site generator |
| Theme | Material for MkDocs 9.7+ | Primary theme |
| Python | 3.8+ | Build environment |
| Content | Markdown only | No HTML pages as source |
| Styling | CSS custom properties | Defined in extra.css |
| Hosting | GitHub Pages (primary) | Azure Static Web Apps (secondary) |
| Backend | None permitted | Static files only |

**Deployment requirements**:
- `mkdocs build` MUST complete without errors
- Output MUST be generated to `site/` directory
- All internal links MUST resolve after build

## File Structure & Conventions

Standard directory structure that MUST be maintained:

```text
docs/                     # Source content (Markdown)
├── index.md              # Home page
├── contact.md            # Contact page
├── blog/
│   ├── index.md          # Blog listing page
│   └── posts/            # Individual blog articles
├── images/               # Image assets (SVG preferred)
└── stylesheets/
    └── extra.css         # Custom CSS overrides

site/                     # Built output (generated, gitignored)
mkdocs.yml                # Site configuration
requirements.txt          # Python dependencies
```

**Navigation structure** (defined in mkdocs.yml):
- Home → Blog → Contact (3-section tabs, MUST NOT exceed without amendment)

## Governance

This constitution supersedes all other development practices for the Vigilant Meme project.

**Amendment process**:
1. Propose change with rationale in PR description
2. Document impact on existing content/styling
3. Update constitution version following semantic versioning:
   - MAJOR: Principle removal or fundamental redefinition
   - MINOR: New principle or significant expansion
   - PATCH: Clarifications, wording improvements
4. Update dependent templates if affected

**Compliance verification**:
- All PRs MUST verify changes align with principles
- Accessibility checks MUST pass before merge
- `mkdocs build` MUST succeed without warnings
- Light and dark mode MUST both be tested

**Version**: 1.0.0 | **Ratified**: 2026-02-06 | **Last Amended**: 2026-02-06
