# UI/UX Audit Report

## نظام الدولية للموارد البشرية (ElDawliya HRMS)

**Report Date:** June 23, 2026  
**Version:** 1.0  
**Auditor:** Automated UI/UX Analysis  
**Scope:** Django templates, CSS design systems, JavaScript components, accessibility, mobile responsiveness

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Page-by-Page Analysis](#2-page-by-page-analysis)
3. [Design Consistency Analysis](#3-design-consistency-analysis)
4. [Accessibility Analysis](#4-accessibility-analysis)
5. [Priority & Recommendations Summary](#5-priority--recommendations-summary)

---

## 1. Executive Summary

### Strengths

- **Comprehensive modular design system** — v3.0 (`frontend/static/css/`) establishes CSS custom properties, design tokens, theme variables, and a component library with reusable building blocks.
- **Enhanced base template** (`base_enhanced.html`) includes accessibility affordances (skip-to-content link, `aria-*` attributes, semantic HTML5 landmarks) and modern UX patterns (global search, notification system, user menu, breadcrumb navigation).
- **Reusable component templates** — `data_table.html`, `form_field.html`, `modal.html`, `stats_card.html` provide consistent, parameterized UI primitives.
- **Dark/light theme system** via `theme-system.css` with CSS custom properties.
- **Thoughtful RTL support** out of the box — Cairo font, Bootstrap 5 RTL, Arabic-first UI text.
- **Keyboard navigation** within sidebar (arrow keys, Home/End) implemented in `base.html`.
- **Loading overlay** and form submission loading states present in base template.

### Weaknesses

- **Dual design system conflict** — v2.0 (`static/css/style.css`, 3161 lines) and v3.0 (`frontend/static/css/design-system.css`, 1254 lines) define overlapping tokens with slightly different values, creating ambiguity.
- **Two sidebar implementations** with different markup, CSS classes, and behavior — `base.html` inline sidebar vs. `base_enhanced.html` sidebar.
- **Inconsistent template inheritance** — some templates extend `base.html`, others extend `base/base_enhanced.html`, and some (`inventory/product_form.html`) extend `inventory/base_inventory.html`.
- **Severe gradient inconsistency** — every module defines its own color scheme (purple for companies/departments, orange/amber for employees, blue for attendance, blue for HR dashboard).
- **Bare-bones forms** — `employee_leave_form.html` uses raw `{{ form.as_p }}` with no styling.
- **Bare-bones tables** — `employee_leave_list.html` has no search, pagination, sorting, or responsive behavior.
- **Massive legacy dashboard** (`home_dashboard.html`, 1832 lines) with embedded CSS, hardcoded data, and duplicate sidebar/navbar definitions.
- **17 JavaScript files** across `static/js/` (11 files) and `frontend/static/js/` (6 files) with overlapping concerns (e.g. two `components.js` files).
- **No skeleton loaders, empty states, or progress indicators** beyond a single spinner overlay.
- **Hardcoded `#` hash links** in sidebar navigation — many items lack real URLs.

### Critical Issues

| # | Issue | Impact | Location |
|---|-------|--------|----------|
| CR-01 | Two conflicting design systems (v2.0 vs v3.0) | CSS conflicts, bloated payload, unpredictable styling | `static/css/style.css` vs `frontend/static/css/*.css` |
| CR-02 | Two base templates with different sidebars | Inconsistent navigation UX, duplicated maintenance | `templates/base.html` vs `frontend/templates/base/base_enhanced.html` |
| CR-03 | Legacy 1832-line dashboard with inline CSS | Extremely slow to render, impossible to maintain, duplicates base template layout | `templates/home_dashboard.html` |
| CR-04 | Unstyled `{{ form.as_p }}` on leave form | Broken user experience, looks like an error page | `templates/leaves/employee_leave_form.html` |

---

## 2. Page-by-Page Analysis

### 2.1 Home Dashboard (`templates/home_dashboard.html`)

**Base:** `base.html`  
**Lines:** 1832  
**Rating:** 🔴 CRITICAL

| Aspect | Assessment |
|--------|-----------|
| Structure | Embeds its own complete sidebar, navbar, and layout — completely ignores base template's sidebar. Defines duplicate CSS for sidebar, navbar, toggle button, and department list. |
| Inline CSS | ~600+ lines of `<style>` block with hardcoded gradients (`#0a58ca`, `#063b8c`), fixed positioning logic, and media queries competing with base template. |
| Hardcoded Data | Department list built via JavaScript array `[ {name:"ادارة الحسابات", count: 10}, ... ]` — not server-rendered. |
| JavaScript | Manual sidebar collapse/expand, inline `onclick` handlers, custom toggle logic. |
| Mobile | Separate `@media (max-width: 992px)` rules for sidebar overlay. |
| Accessibility | No ARIA landmarks, no skip-to-content, no `aria-expanded` on toggle. |

### 2.2 New Dashboard (`frontend/templates/pages/dashboard.html`)

**Base:** `base/base_enhanced.html`  
**Lines:** ~988+  
**Rating:** 🟡 MEDIUM

| Aspect | Assessment |
|--------|-----------|
| Structure | Clean component-based design with welcome section, quick actions grid, and section detail views. |
| Reusability | Uses `card-enhanced`, `section-card`, `detail-card` classes from v3.0 design system. |
| Issues | No real-time data, no chart visualizations (Chart.js loaded but unused), many `#` links in detail-card buttons. Hardcoded "back" navigation with JS-controlled show/hide for section details. |
| Mobile | Uses Bootstrap grid (`col-xl-3 col-lg-4 col-md-6`) — responsive by default. |

### 2.3 Employee Leave List (`templates/leaves/employee_leave_list.html`)

**Base:** `base.html`  
**Lines:** 30  
**Rating:** 🔴 CRITICAL

| Aspect | Assessment |
|--------|-----------|
| Table | Bare `<table class="table table-striped">` — no responsive wrapper, no search, no pagination, no sorting, no export. |
| Empty State | Single `<td colspan="6">لا توجد إجازات</td>` — no illustration or icon. |
| Actions | `btn btn-sm btn-secondary` / `btn btn-sm btn-danger` — minimal, no confirmation dialog on delete. |
| Header | Flat `<h1>` without any page header container. |

### 2.4 Employee Leave Form (`templates/leaves/employee_leave_form.html`)

**Base:** `base.html`  
**Lines:** 10  
**Rating:** 🔴 CRITICAL

| Aspect | Assessment |
|--------|-----------|
| Form Rendering | `{{ form.as_p }}` — completely unstyled, no grid, no labels (Django auto-generates bare `<label>` tags), no validation styling. |
| Layout | Flat `<h1>{{ title }}</h1>` without breadcrumb or page header. |
| Submit | Single `<button class="btn btn-primary">حفظ</button>` — no loading state, no confirmation. |

### 2.5 Company List (`templates/companies/company_list.html`)

**Base:** `base.html`  
**Lines:** 255  
**Rating:** 🟡 MEDIUM

| Aspect | Assessment |
|--------|-----------|
| Style | Card-based layout with purple gradient header (`linear-gradient(135deg, #6f42c1, #5a2d91)`). Heavy inline `<style>` block (67 lines). |
| Cards | `.company-card` with hover effects, shadow, transition — visually polished but inconsistent with other module colors. |
| No table option | Only card view available — no table toggle for dense data browsing. |
| Navigation | Uses `<a href="{% url 'companies:create' %}">` — properly linked. |

### 2.6 Company Detail (`templates/companies/company_detail.html`)

**Base:** `base.html`  
**Lines:** 10  
**Rating:** 🟠 HIGH

| Aspect | Assessment |
|--------|-----------|
| Content | Bare `<h1>{{ company.name }}</h1>` with `<p>` tags for each field. No cards, no styling, no breadcrumb. |
| Navigation | Plain `<a href="...">عودة للقائمة</a>` — not styled as a button. |
| Empty State | No handling for missing fields (e.g. `{{ company.email }}` shows nothing if empty). |

### 2.7 Company Form (`templates/companies/company_form.html`)

**Base:** `base.html`  
**Lines:** 25  
**Rating:** 🟠 HIGH

| Aspect | Assessment |
|--------|-----------|
| Form | Uses Django form fields with manual grid (`<div class="row g-3">` and `<div class="col-md-6">`). |
| Labels | Proper `<label class="form-label">` with `{{ field.label }}`. |
| Validation | Manual error display with `<div class="text-danger small">{{ field.errors.0 }}</div>`. |
| Issues | No `required` indicators, no `for` attribute on labels, no help text rendering. Uses `{{ field }}` directly — inconsistent with v3.0 `form_field.html` component. |

### 2.8 Departments (`templates/org/departments.html`)

**Base:** `base.html`  
**Lines:** 289  
**Rating:** 🟡 MEDIUM

| Aspect | Assessment |
|--------|-----------|
| Style | Purple gradient header (`#8b5cf6` to `#7c3aed`) — same as companies but different purple shade. |
| Cards | `.department-card` with hover effects — visually consistent within page but inconsistent across modules. |
| Actions | `btn-outline-success` redefined with custom colors in inline CSS rather than using Bootstrap variables. |
| Mobile | Custom `@media (max-width: 576px)` rules for flex-direction column on actions. |

### 2.9 Employees Index (`templates/employees/index.html`)

**Base:** `base.html`  
**Lines:** 268  
**Rating:** 🟡 MEDIUM

| Aspect | Assessment |
|--------|-----------|
| Style | Amber/orange gradient (`#f59e0b` to `#d97706`) — completely different from purple used by companies/departments. |
| Cards | `.employee-stat-card` with `.employee-stat-icon` — same pattern but different variable names. |
| Consistency | Has `hover` effects, shadow, border — but class names are prefixed with `employee-` instead of generic. |

### 2.10 HR Dashboard (`templates/hr/hr_dashboard.html`)

**Base:** `base.html`  
**Lines:** 606  
**Rating:** 🟠 HIGH

| Aspect | Assessment |
|--------|-----------|
| Redefines CSS variables | `:root { --hr-primary: #2563eb; ... }` — duplicates values already in design system. |
| Gradient | Blue gradient (`#2563eb` to `#1e40af`) — yet another color. |
| Inline CSS | Massive `<style>` block (72+ lines) defining `.hr-stat-card`, `.hr-stat-icon`, `.hr-stat-value` — classes prefixed with `hr-` making them non-reusable. |
| Pattern | Same card pattern repeated with different prefix for every module. |

### 2.11 Attendance Dashboard (`templates/attendance/dashboard.html`)

**Base:** `base.html`  
**Lines:** 374  
**Rating:** 🟠 HIGH

| Aspect | Assessment |
|--------|-----------|
| Style | Blue gradient (`#3b82f6` to `#1d4ed8`) — similar to HR but different shade. |
| Classes | `.attendance-stat-card`, `.attendance-stat-icon` — yet another module-specific prefix. |
| Redundancy | Same HTML pattern, same CSS rules, different class names — 30+ lines of inline CSS each time. |

### 2.12 Inventory Product Form (`templates/inventory/product_form.html`)

**Base:** `inventory/base_inventory.html`  
**Lines:** 539  
**Rating:** 🟡 MEDIUM

| Aspect | Assessment |
|--------|-----------|
| Form | Well-structured with form sections (`form-section`), proper labels with `required-label`, input groups, and help text. |
| Features | Modal for inline category/unit creation, file upload for images, section grouping with icons. |
| Style | Custom indigo accent (`#3f51b5`, `#303f9f`) — still different from other modules. |
| JavaScript | 180+ lines of inline JS for modal handling, form validation, and hidden field management. Debug `console.log` statements left in production code. |
| Issues | `alert()` for user feedback instead of toast notifications. Inline `<style>` block (80 lines). |

### 2.13 Error Pages (`templates/errors/404.html`, `403.html`, `500.html`)

**Base:** `base.html`  
**Rating:** 🟢 GOOD

| Aspect | Assessment |
|--------|-----------|
| 404 | Clean centered layout with display heading, descriptive text, and home button. |
| Missing | No branded illustration, no search suggestion, no "report problem" link. |

---

## 3. Design Consistency Analysis

### 3.1 Color Scheme Fragmentation

| Module | Gradient Used | Accent Color | File |
|--------|-------------|-------------|------|
| Companies (list) | Purple `#6f42c1` → `#5a2d91` | Purple | `templates/companies/company_list.html` |
| Departments | Purple `#8b5cf6` → `#7c3aed` | Purple | `templates/org/departments.html` |
| Employees | Amber `#f59e0b` → `#d97706` | Amber | `templates/employees/index.html` |
| HR Dashboard | Blue `#2563eb` → `#1e40af` | Blue | `templates/hr/hr_dashboard.html` |
| Attendance | Blue `#3b82f6` → `#1d4ed8` | Blue | `templates/attendance/dashboard.html` |
| Inventory Form | Indigo `#3f51b5` → `#303f9f` | Indigo | `templates/inventory/product_form.html` |
| Base v2.0 | Dark `#1e293b` → `#334155` | Blue `#2563eb` | `templates/base.html` |
| Base v3.0 | Dark `#0f172a` → `#1e293b` | Blue `#2563eb` | `frontend/templates/base/base_enhanced.html` |

**Problem:** Every page is visually disconnected. A user navigating from Employees (amber) → Departments (purple) → Inventory (indigo) experiences 3 different color identities.

### 3.2 Class Naming Convention Inconsistency

| Pattern | Examples | Where |
|---------|----------|-------|
| Prefix with module | `hr-stat-card`, `employee-stat-card`, `attendance-stat-card`, `company-card`, `department-card` | Legacy templates |
| Generic enhanced | `card-enhanced`, `stats-card`, `form-group-enhanced`, `table-enhanced` | v3.0 design system |
| Bootstrap-only | `table table-striped`, `card`, `btn btn-primary` | Minimal templates (leaves) |
| Custom inline | `form-card`, `form-section`, `section-title` | Inventory form |

### 3.3 Form Rendering Inconsistency

| Template | Rendering Method | Label Style | Validation |
|----------|-----------------|-------------|------------|
| `employee_leave_form.html` | `{{ form.as_p }}` | Auto-generated | None |
| `company_form.html` | Manual grid + `{{ field }}` | `<label class="form-label">` | Manual `.errors.0` |
| `product_form.html` | Manual HTML | `<label class="form-label required-label">` | Bootstrap validation + manual |
| v3.0 `form_field.html` | Component include | `<label class="form-label-enhanced">` | `.is-invalid` + `.invalid-feedback` |

### 3.4 Table/Card View Inconsistency

| Template | Display Mode | Features |
|----------|-------------|----------|
| `employee_leave_list.html` | Bare table | None |
| `company_list.html` | Custom cards | Hover effects |
| v3.0 `data_table.html` | Enhanced table | Search, sort, pagination, column types (badge, avatar, currency, progress) |

### 3.5 Page Header Fragmentation

Several different patterns for page headers exist across the codebase:

1. **Gradient header div** — companies, employees, departments, HR dashboard, attendance (each with module-specific colors)
2. **Simple `<h1>`** — leave list, leave form, company detail, company form
3. **`page-header` component** — v3.0 dashboard and enhanced base template
4. **`.top-navbar` + breadcrumb** — `base.html` inline

---

## 4. Accessibility Analysis

### 4.1 WCAG Compliance Checklist

| Criteria | Status | Location |
|----------|--------|----------|
| **1.1.1 Non-text Content** | ⚠️ Partial | Icons in sidebar lack `aria-hidden="true"` or screen-reader text |
| **1.4.3 Contrast (Minimum)** | ❌ Not verified | No automated contrast check performed; gradient text overlays may fail |
| **2.1.1 Keyboard** | ⚠️ Partial | Sidebar has arrow-key nav; modal traps not implemented |
| **2.4.1 Bypass Blocks** | ✅ Present | `base_enhanced.html` has skip-to-content link; `base.html` does NOT |
| **2.4.4 Link Purpose** | ⚠️ Partial | "عرض الكل", "إعدادات" links use generic text |
| **2.4.7 Focus Visible** | ⚠️ Partial | `base.html` has `.nav-link:focus` styles; most other elements lack custom focus indicators |
| **3.3.2 Labels or Instructions** | ❌ Missing | `employee_leave_form.html` has no proper label associations; many forms lack `for` attributes |
| **4.1.2 Name, Role, Value** | ⚠️ Partial | `base_enhanced.html` uses `aria-label` and `aria-expanded`; legacy templates do not |
| **ARIA Landmarks** | ⚠️ Partial | `base_enhanced.html` uses `<nav role="navigation">`, `<main>`, `<header role="banner">`, `<footer role="contentinfo">`; `base.html` does not |

### 4.2 Specific Accessibility Violations

| Violation | Severity | Location |
|-----------|----------|----------|
| No `lang="ar"` on some pages | High | Child templates overriding `<html>` tag |
| Skip-to-content missing | High | `templates/base.html` line 441 |
| `aria-hidden` not used on decorative icons | Medium | All sidebar icon `<i>` elements |
| Notification bell lacks screen-reader text | Medium | `base.html` line 732 |
| `alert()` used for user feedback | High | `product_form.html` line 436 |
| No focus trap in modals | High | Inventory modals (`product_form.html`) |
| Empty table cells not marked as such | Low | `employee_leave_list.html` |
| Insufficient color contrast on navbar | Medium | White text on gradient backgrounds |

### 4.3 Keyboard Navigation Gaps

| Feature | Supported? | Notes |
|---------|-----------|-------|
| Sidebar arrow-key nav | ✅ Yes | `base.html` lines 929-966 |
| Tab through form fields | ⚠️ Partial | Works for simple forms; complex modals lack focus management |
| Escape to close modal | ✅ Yes | Bootstrap modal default |
| Enter to submit | ✅ Yes | Default HTML behavior |
| Skip-to-content | ⚠️ Partial | Only in `base_enhanced.html` |

### 4.4 Screen Reader Considerations

- **Positive:** `base_enhanced.html` includes `aria-label` on navigation, `aria-current="page"` on active links, `role="banner"`, `role="contentinfo"`.
- **Negative:** `base.html` has none of these. All legacy templates inherit from `base.html`.
- **Negative:** Icon-only buttons (edit, delete in tables) lack `aria-label`.
- **Negative:** Form fields in `employee_leave_form.html` are not properly associated with labels.

---

## 5. Priority & Recommendations Summary

### Priority Definitions

| Priority | Definition | Target Response |
|----------|-----------|----------------|
| **Critical** | Blocks user task, security risk, or major inconsistency | Immediate |
| **High** | Significant UX degradation, development friction | This sprint |
| **Medium** | Visual inconsistency, minor usability issue | Next sprint |
| **Low** | Enhancement, nice-to-have | Backlog |

### 5.1 Critical Issues

| ID | Issue | Recommendation |
|----|-------|---------------|
| CR-01 | **Dual design system conflict** | Deprecate `static/css/style.css` and consolidate all tokens into `frontend/static/css/design-system.css`. Create a migration guide mapping old v2.0 classes to v3.0 equivalents. |
| CR-02 | **Two base templates** | Choose `base_enhanced.html` as the single source of truth. Rewrite `base.html` to extend or redirect to it, or remove it entirely after confirming no template depends on it uniquely. |
| CR-03 | **Legacy 1832-line dashboard** | Replace `home_dashboard.html` with the new `frontend/templates/pages/dashboard.html`. The old file should be archived; its JavaScript department list should be moved to the backend context processor. |
| CR-04 | **Bare `{{ form.as_p }}` on leave form** | Rewrite using the v3.0 `form_field.html` component. Add proper validation, labels, and responsive grid layout. |

### 5.2 High Priority Issues

| ID | Issue | Recommendation |
|----|-------|---------------|
| HI-01 | **Module-specific gradient colors** | Adopt a single brand color system (use `--primary` variants) across all modules. Remove inline gradient styles from all templates. |
| HI-02 | **Duplicate card/stat CSS per module** | Consolidate into reusable classes in `components.css`. Remove `.hr-stat-card`, `.employee-stat-card`, `.attendance-stat-card`, `.company-card`, `.department-card` inline CSS. |
| HI-03 | **Bare-bones table (`employee_leave_list`)** | Use `data_table.html` component with search, pagination, and responsive wrapper. |
| HI-04 | **Bare company detail page** | Apply card layout with proper styling, breadcrumb, and action buttons. |
| HI-05 | **17 overlapping JS files** | Audit all JS files, remove unused code, consolidate into modular files under `frontend/static/js/`. Remove duplicate `components.js`. |
| HI-06 | **Skip-to-content missing in base.html** | Add the skip-to-content link from `base_enhanced.html` to all legacy templates. |
| HI-07 | **`alert()` calls in production** | Replace with a centralized toast notification service using Bootstrap toasts. |
| HI-08 | **Hash links (`#`) in sidebar** | Replace with actual named URLs or stub the views and wire up proper navigation. |

### 5.3 Medium Priority Issues

| ID | Issue | Recommendation |
|----|-------|---------------|
| MI-01 | **No form sections/grouping guidelines** | Create a form composition pattern using `form_field.html` components with optional `<fieldset>` and `<legend>` wrappers. |
| MI-02 | **No required field indicators** | Add `required` CSS class to labels via the `form_field.html` component consistently. |
| MI-03 | **No table export functionality** | Add CSV/PDF export buttons to `data_table.html`. |
| MI-04 | **No skeleton loaders** | Implement skeleton screen components for dashboard stats cards and data tables. |
| MI-05 | **No empty state illustrations** | Add icon illustrations and descriptive empty-state messages using a reusable `empty_state.html` component. |
| MI-06 | **No real-time data indicators** | Add "last updated" timestamps and auto-refresh indicators to dashboard widgets. |
| MI-07 | **No form confirmation on delete** | Implement the `modal.html` component for delete confirmations across all list views. |
| MI-08 | **Hardcoded department JS data** | Move department data to Django context and render server-side. |
| MI-09 | **No chart visualizations** | Chart.js is loaded but unused. Add real KPI charts to dashboards. |
| MI-10 | **Active state inconsistency** | Ensure `request.resolver_match` pattern is used consistently across all sidebar links. |

### 5.4 Low Priority Issues

| ID | Issue | Recommendation |
|----|-------|---------------|
| LO-01 | **No favicon/apple-touch-icon** | Add favicon support to `base.html` (already present in `base_enhanced.html`). |
| LO-02 | **Missing SEO meta tags** | Add `meta description` and `meta keywords` to legacy templates. |
| LO-03 | **Console.log in production** | Remove debug logging from `product_form.html` and other JS files. |
| LO-04 | **No keyboard shortcut** | Add `Ctrl+K` for global search, `Ctrl+E` for employee search. |
| LO-05 | **No PWA support** | Service worker is registered but no manifest or offline page exists. |
| LO-06 | **Font Awesome integrity hashes** | Add `integrity` attribute to Font Awesome CDN link (present in enhanced, missing in base). |

---

## Appendices

### A. File Inventory

| File | Lines | Role |
|------|-------|------|
| `templates/base.html` | 1062 | Original base template |
| `frontend/templates/base/base_enhanced.html` | 731 | Enhanced base template (v3.0) |
| `templates/home_dashboard.html` | 1832 | Legacy main dashboard |
| `frontend/templates/pages/dashboard.html` | 988+ | New dashboard |
| `static/css/style.css` | 3161 | v2.0 design system |
| `frontend/static/css/design-system.css` | 1254 | v3.0 design system |
| `frontend/static/css/components.css` | 1797 | Component library |
| `frontend/static/css/theme-system.css` | 594 | Dark/light theme |
| `frontend/static/css/grid-system.css` | 608 | Responsive grid |
| `static/js/` (11 files) | — | Legacy JS |
| `frontend/static/js/` (6 files) | — | Enhanced JS |

### B. CSS Token Overlap (v2.0 vs v3.0)

| Token | v2.0 Value | v3.0 Value |
|-------|-----------|-----------|
| `--secondary-50` | `#f8fafc` | Defined as `--gray-50: #f8fafc` |
| `--secondary-100` | `#f1f5f9` | `--gray-100: #f1f5f9` |
| `--secondary-200` | `#e2e8f0` | `--gray-200: #e2e8f0` |
| `--success-50` | `#f0fdf4` | `#f0fdf4` (same) |
| `--success-500` | `#22c55e` | `#22c55e` (same) |
| `--info-500` | `#06b6d4` | `#0ea5e9` (different!) |
| `--info-600` | `#0891b2` | `#0284c7` (different!) |
| `--info-700` | `#0e7490` | `#0369a1` (different!) |

The `--info` palette diverges between v2.0 and v3.0, which would cause inconsistent coloring if both systems are loaded simultaneously.

### C. Template Extension Map

```
extends 'base.html'
├── home_dashboard.html
├── leaves/employee_leave_form.html
├── leaves/employee_leave_list.html
├── leaves/leave_type_*.html
├── leaves/public_holiday_*.html
├── companies/company_list.html
├── companies/company_form.html
├── companies/company_detail.html
├── companies/company_confirm_delete.html
├── employees/index.html
├── hr/hr_dashboard.html
├── hr/profile.html
├── hr/settings.html
├── hr/my_payslips.html
├── hr/notifications.html
├── hr/my_leaves.html
├── attendance/dashboard.html
├── attendance/leave_balance_list.html
├── attendance/calculate_overtime.html
├── org/departments.html
├── org/department_*.html
├── org/branches.html / branch_add.html
├── org/jobs.html / job_*.html
├── reporting/dashboard.html
├── payrolls/dashboard.html
├── evaluations/dashboard.html
├── insurance/dashboard.html
├── errors/404.html, 403.html, 500.html
├── components/showcase.html
├── components/breadcrumb.html

extends 'base/base_enhanced.html'
├── pages/dashboard.html

extends 'inventory/base_inventory.html' (unknown base)
├── inventory/product_form.html
```

---

*End of Report*
