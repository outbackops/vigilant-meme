# Tasks: Vigilant Meme Feature Specification Standards

**Input**: Design documents from `/specs/001-spec-template/`  
**Prerequisites**: plan.md ✅, spec.md ✅, research.md ✅, data-model.md ✅, quickstart.md ✅

**Tests**: Not explicitly requested in specification. Manual validation checklists included instead.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story?] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions (Static Site)

- **Content**: `docs/`, `docs/blog/posts/`
- **Styling**: `docs/stylesheets/extra.css`
- **Configuration**: `mkdocs.yml`, `requirements.txt`
- **Output**: `site/` (generated, gitignored)

---

## Phase 1: Setup

**Purpose**: Ensure local development environment is ready

- [ ] T001 Verify Python 3.8+ is installed (`py -3 --version` or `python3 --version`)
- [ ] T002 Install dependencies from requirements.txt (`py -3 -m pip install -r requirements.txt`)
- [ ] T003 Verify MkDocs serves locally (`py -3 -m mkdocs serve` at http://127.0.0.1:8000)

**Checkpoint**: Local development environment operational

---

## Phase 2: Foundational (Validation Infrastructure)

**Purpose**: Establish validation patterns that all user stories depend on

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T004 Verify `mkdocs build --strict` passes without warnings in repository root
- [ ] T005 [P] Verify light mode renders correctly on Home, Blog, and Contact pages
- [ ] T006 [P] Verify dark mode renders correctly on Home, Blog, and Contact pages
- [ ] T007 [P] Verify responsive layout at mobile viewport (<768px) on all pages
- [ ] T007b [P] Verify responsive layout at tablet viewport (768px-1024px) on all pages
- [ ] T007c [P] Verify touch targets are at least 44x44px on mobile (buttons, links, nav items)
- [ ] T008 Verify keyboard navigation works (Tab through all interactive elements)

**Checkpoint**: Foundation validated - user story implementation can now begin

---

## Phase 3: User Story 1 - Create New Blog Post (Priority: P1) 🎯 MVP

**Goal**: Authors can create consistent blog posts following established patterns

**Independent Test**: Create a sample blog post, verify it renders correctly in both modes, and appears in blog index

### Implementation for User Story 1

- [ ] T009 [US1] Create sample blog post file in docs/blog/posts/sample-post.md with required structure (Title, Date, Author)
- [ ] T010 [US1] Add hero image container to sample post using `<div class="hero-container">` pattern in docs/blog/posts/sample-post.md
- [ ] T011 [US1] Add Material admonitions (`!!! info`, `!!! tip`, `!!! example`) to sample post in docs/blog/posts/sample-post.md
- [ ] T012 [US1] Add link to sample post in docs/blog/index.md following existing link format
- [ ] T013 [US1] Verify sample post renders with proper formatting via `mkdocs serve`
- [ ] T014 [US1] Verify sample post appears in blog index navigation
- [ ] T015 [US1] Verify admonitions display correctly in light mode
- [ ] T016 [US1] Verify admonitions display correctly in dark mode
- [ ] T017 [US1] Verify hero image is responsive and has alt text
- [ ] T017b [US1] Verify all images in sample post have descriptive alt text (not just filenames)

**Checkpoint**: User Story 1 complete - blog post creation workflow validated

---

## Phase 4: User Story 2 - Modify Site Styling (Priority: P2)

**Goal**: Designers can modify CSS variables and see consistent changes across all pages

**Independent Test**: Modify a gradient variable, verify change appears on all pages in both color modes

### Implementation for User Story 2

- [ ] T018 [US2] Document existing CSS variables in docs/stylesheets/extra.css (add inline comments if missing)
- [ ] T019 [US2] Create test modification: add a new CSS variable `--test-color` in `:root` selector in docs/stylesheets/extra.css
- [ ] T020 [US2] Add dark mode variant of `--test-color` in `[data-md-color-scheme="slate"]` selector in docs/stylesheets/extra.css
- [ ] T021 [US2] Apply test variable to a visible element (e.g., `.hero-container` border) in docs/stylesheets/extra.css
- [ ] T022 [US2] Verify test variable appears on Home page in light mode
- [ ] T023 [US2] Verify test variable appears on Home page in dark mode
- [ ] T024 [US2] Verify test variable appears on Blog page in both modes
- [ ] T025 [US2] Verify test variable appears on Contact page in both modes
- [ ] T026 [US2] Add responsive override for test element at `@media (max-width: 768px)` in docs/stylesheets/extra.css
- [ ] T027 [US2] Verify responsive override applies on mobile viewport
- [ ] T028 [US2] Remove test modifications after validation (or keep if useful)

**Checkpoint**: User Story 2 complete - CSS variable system validated

---

## Phase 5: User Story 3 - Update Site Configuration (Priority: P3)

**Goal**: Maintainers can update navigation and add plugins with confidence

**Independent Test**: Add a test page to navigation, verify it builds and links correctly

### Implementation for User Story 3

- [ ] T029 [P] [US3] Create test page docs/test-page.md with minimal content
- [ ] T030 [US3] Add test page to `nav:` section in mkdocs.yml
- [ ] T031 [US3] Verify `mkdocs build --strict` passes after nav change
- [ ] T032 [US3] Verify test page appears in navigation tabs via `mkdocs serve`
- [ ] T033 [US3] Verify test page link resolves correctly
- [ ] T034 [US3] Test adding a markdown extension syntax (e.g., tabbed content with `===`) in docs/test-page.md
- [ ] T035 [US3] Verify extension syntax renders correctly after build
- [ ] T036 [US3] Remove test page from nav and delete docs/test-page.md after validation
- [ ] T037 [US3] Verify final `mkdocs build --strict` passes with clean state

**Checkpoint**: User Story 3 complete - configuration workflow validated

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Final validation and cleanup

- [ ] T038 [P] Run Lighthouse accessibility audit on Home page (target: 90+ score)
- [ ] T039 [P] Run Lighthouse accessibility audit on Blog page (target: 90+ score)
- [ ] T040 [P] Run Lighthouse accessibility audit on Contact page (target: 90+ score)
- [ ] T041 Verify all internal links resolve correctly via `mkdocs build --strict`
- [ ] T042 Verify site loads in under 3 seconds (browser DevTools Network tab, 3G throttle)
- [ ] T043 Run full validation using quickstart.md checklist
- [ ] T043b Test screen reader compatibility on Home page (NVDA, VoiceOver, or browser reader mode)
- [ ] T044 Clean up any test artifacts (sample-post.md if not needed for demo)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3-5)**: All depend on Foundational phase completion
  - Can proceed sequentially in priority order (P1 → P2 → P3)
  - Or in parallel if multiple contributors
- **Polish (Phase 6)**: Depends on all user stories being complete

### User Story Dependencies

| Story | Depends On | Can Start After |
|-------|------------|-----------------|
| User Story 1 (P1) | Foundational only | T008 complete |
| User Story 2 (P2) | Foundational only | T008 complete |
| User Story 3 (P3) | Foundational only | T008 complete |

All user stories are **independently testable** - they do not depend on each other.

### Within Each User Story

- Create artifacts first (files, config changes)
- Verify renders second (visual inspection)
- Validate modes third (light/dark, responsive)
- Cleanup last (if test artifacts)

### Parallel Opportunities

**Within Phase 2 (Foundational)**:
```
T005 (light mode) | T006 (dark mode) | T007 (responsive)
```

**Across User Stories (if team capacity)**:
```
US1 (T009-T017) | US2 (T018-T028) | US3 (T029-T037)
```

**Within Phase 6 (Polish)**:
```
T038 (Home audit) | T039 (Blog audit) | T040 (Contact audit)
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup (T001-T003)
2. Complete Phase 2: Foundational (T004-T008)
3. Complete Phase 3: User Story 1 (T009-T017)
4. **STOP and VALIDATE**: Blog post workflow works independently
5. Deploy/demo if ready - authors can now create posts confidently

### Incremental Delivery

1. Setup + Foundational → Environment validated
2. User Story 1 → Blog post workflow complete → **MVP!**
3. User Story 2 → CSS styling workflow complete → Demo
4. User Story 3 → Configuration workflow complete → Demo
5. Polish → Full validation complete → Production ready

---

## Summary

| Metric | Count |
|--------|-------|
| Total Tasks | 48 |
| Phase 1 (Setup) | 3 |
| Phase 2 (Foundational) | 7 |
| User Story 1 (P1) | 10 |
| User Story 2 (P2) | 11 |
| User Story 3 (P3) | 9 |
| Phase 6 (Polish) | 8 |
| Parallel Opportunities | 14 tasks marked [P] |

### Format Validation ✅

All 44 tasks follow the required format:
- ✅ Checkbox prefix (`- [ ]`)
- ✅ Task ID (T001-T044)
- ✅ [P] marker where applicable
- ✅ [US#] label for user story tasks
- ✅ File paths included in descriptions

### Independent Test Criteria

| User Story | Independent Test |
|------------|------------------|
| US1 | Create sample post → renders in both modes → appears in blog index |
| US2 | Modify CSS variable → change visible on all pages in both modes |
| US3 | Add page to nav → builds without errors → link resolves |

### Suggested MVP Scope

**MVP = Phase 1 + Phase 2 + User Story 1 (T001-T017)**

This delivers a validated blog post creation workflow that authors can use immediately.
