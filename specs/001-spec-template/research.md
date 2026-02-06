# Research: Vigilant Meme Feature Specification Standards

**Feature**: 001-spec-template  
**Date**: 2026-02-06  
**Purpose**: Resolve all NEEDS CLARIFICATION items and document technology decisions

## Research Tasks Completed

### 1. MkDocs Material Theme Best Practices

**Decision**: Use Material for MkDocs 9.7+ with built-in features  
**Rationale**: The project already uses Material theme correctly; no changes needed  
**Alternatives Considered**:
- Custom theme: Rejected (unnecessary complexity, lose Material updates)
- Different static generator (Hugo, Jekyll): Rejected (already invested in MkDocs ecosystem)

**Key Findings**:
- Material theme provides built-in light/dark toggle via `palette` config
- Admonitions are natively supported via `pymdownx` extensions
- Hero images require custom CSS (already implemented in `extra.css`)

### 2. Accessibility Standards for Static Sites

**Decision**: Target WCAG 2.1 Level AA compliance  
**Rationale**: AA is industry standard; AAA is often impractical for decorative elements  
**Alternatives Considered**:
- WCAG 2.0: Rejected (outdated, missing mobile considerations)
- WCAG 2.1 AAA: Rejected (too restrictive for gradient designs)

**Key Findings**:
- Color contrast: 4.5:1 for normal text, 3:1 for large text/UI components
- Keyboard navigation: Material theme handles this natively for navigation
- Alt text: Must be descriptive, not just filename
- Focus indicators: Material theme provides default focus styles

### 3. CSS Variable System for Theming

**Decision**: Use CSS custom properties in `:root` and `[data-md-color-scheme="slate"]`  
**Rationale**: This is already the established pattern in `extra.css`  
**Alternatives Considered**:
- Sass/SCSS variables: Rejected (adds build complexity)
- Inline styles: Rejected (not maintainable, violates DRY)

**Key Findings**:
- Three gradient variables cover all use cases (primary, accent, info)
- Dark mode selector is specific to Material theme
- Browser fallback not needed (CSS variables have 97%+ support)

### 4. Blog Post Structure Standards

**Decision**: Require Title, Date, Author at file top; recommend ELI5 summaries  
**Rationale**: Consistency with existing 18+ posts; metadata enables future automation  
**Alternatives Considered**:
- YAML frontmatter: Rejected (not currently used, would require migration)
- No metadata: Rejected (loses organization and discoverability)

**Key Findings**:
- Current posts use Markdown headings for title, bold text for metadata
- Material theme doesn't require specific frontmatter format
- Blog plugin uses file dates if not specified in content

### 5. Responsive Breakpoint Strategy

**Decision**: Use 768px as primary mobile breakpoint  
**Rationale**: Aligns with Material theme defaults and iPad portrait width  
**Alternatives Considered**:
- 640px (Tailwind default): Rejected (too narrow, misses tablets)
- 1024px only: Rejected (ignores mobile-specific needs)

**Key Findings**:
- Material theme handles navigation responsively at 768px
- Hero images need custom responsive handling (already in `extra.css`)
- Touch targets auto-sized by Material theme buttons

## Resolved NEEDS CLARIFICATION Items

| Item | Resolution |
|------|------------|
| Testing approach | Manual browser testing + Lighthouse CLI for CI |
| Color contrast verification | Use browser DevTools accessibility panel |
| Build validation | `mkdocs build --strict` flags warnings as errors |
| Deployment verification | Preview deploy before production push |

## Technology Decisions Summary

| Area | Technology | Version | Notes |
|------|------------|---------|-------|
| Generator | MkDocs | 1.6+ | Static site generator |
| Theme | Material for MkDocs | 9.7+ | Primary theme with built-in features |
| Styling | CSS Custom Properties | N/A | No preprocessor needed |
| Markdown | pymdownx extensions | Bundled | Admonitions, tabs, code blocks |
| Hosting | GitHub Pages | N/A | Primary deployment target |
| Secondary Hosting | Azure Static Web Apps | N/A | Alternative deployment option |

## Dependencies

All dependencies are already in `requirements.txt`:
- `mkdocs>=1.6.0` - Core static site generator
- `mkdocs-material>=9.7.0` - Material theme
- `mkdocs-blog-plugin>=0.25` - Blog functionality

**No new dependencies required.**

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Accessibility regression | Low | High | Lighthouse CI check on PR |
| Broken links after refactor | Medium | Medium | `mkdocs build --strict` fails on bad links |
| CSS variable browser support | Very Low | Low | 97%+ browser support, no action needed |
| Dark mode styling mismatch | Medium | Low | Manual testing checklist before merge |
