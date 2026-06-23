# UI/UX Improvement Roadmap

## نظام الدولية للموارد البشرية (ElDawliya HRMS)

**Plan Version:** 1.0  
**Date:** June 23, 2026  
**Based On:** UI/UX Audit Report (v1.0) & Design System v3.0  
**Total Duration:** 12 days  
**Status:** 📋 Planned

---

## Table of Contents

1. [Overview & Strategy](#overview--strategy)
2. [Phase 1: Foundation (Days 1-2)](#phase-1-foundation-days-1-2)
3. [Phase 2: Design System Enforcement (Days 3-4)](#phase-2-design-system-enforcement-days-3-4)
4. [Phase 3: Dashboard Enhancement (Day 5)](#phase-3-dashboard-enhancement-day-5)
5. [Phase 4: Table Experience (Days 6-7)](#phase-4-table-experience-days-6-7)
6. [Phase 5: Form Enhancement (Days 8-9)](#phase-5-form-enhancement-days-8-9)
7. [Phase 6: Mobile & Accessibility (Day 10)](#phase-6-mobile--accessibility-day-10)
8. [Phase 7: Performance UX (Day 11)](#phase-7-performance-ux-day-11)
9. [Phase 8: Polish & Final QA (Day 12)](#phase-8-polish--final-qa-day-12)
10. [Risk Register](#risk-register)
11. [Success Metrics](#success-metrics)

---

## Overview & Strategy

### Problem Statement

The ElDawliya HRMS suffers from a fragmented user interface caused by two competing design systems (v2.0 at `static/css/style.css` and v3.0 at `frontend/static/css/`), two base templates with different sidebar implementations (`templates/base.html` and `frontend/templates/base/base_enhanced.html`), module-specific gradient color schemes that visually disconnect pages, and critical usability issues in core forms and tables.

### Approach

This roadmap resolves issues **bottom-up**: foundation first (templates, CSS, navigation), then design consistency, then feature enhancements, then mobile/accessibility, then performance, and finally quality assurance. Each phase produces a shippable increment.

### Guiding Principles

1. **Single source of truth** — v3.0 design system becomes canonical; v2.0 is deprecated
2. **Progressively enhance, never regress** — each change improves UX without breaking existing functionality
3. **RTL-first Arabic UX** — all components designed for `dir="rtl"` first
4. **Accessibility is not optional** — WCAG 2.1 AA minimum at every phase

### Key Decision: Base Template Strategy

**Decision:** Adopt `base_enhanced.html` as the single base template. Rewrite `templates/base.html` to extend from it, or migrate all templates that extend `base.html` to extend `base/base_enhanced.html` directly. The v2.0 base template's inline CSS sidebar (~400 lines) must be replaced with the v3.0 `sidebar-nav-enhanced` component.

---

## Phase 1: Foundation (Days 1-2)

**Goal:** Stop the bleeding. Unify templates, consolidate CSS, fix critical UX failures.

### 1.1 Unify Base Templates

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Make `base_enhanced.html` the canonical base | `templates/base.html`, `frontend/templates/base/base_enhanced.html` | Rewrite `templates/base.html` to `{% extends "base/base_enhanced.html" %}` with only overrides; or replace its content entirely with the v3.0 base. Ensure all `{% block %}` names match. | ✅ Consistent navigation UX — same sidebar, navbar, breadcrumbs everywhere | 🔴 Critical |
| Migrate all `extends 'base.html'` templates | 45+ templates listed in audit Appendix C | Change `{% extends 'base.html' %}` to `{% extends 'base/base_enhanced.html' %}` in every template. Update any block names that differ. | ✅ Removes dual-sidebar confusion, enables accessibility features (skip-to-content, ARIA landmarks) | 🔴 Critical |
| Standardize `{% block %}` names | All templates | Audit blocks used (`title`, `content`, `extra_css`, `extra_js`, `breadcrumb`, `page_title`, `breadcrumb_title`, `page_description`) and ensure all templates use the same set. Map v2.0 blocks to v3.0 equivalents. | ✅ Predictable template inheritance — developers know which blocks to use | 🟠 High |
| Add favicon/apple-touch-icon | `templates/base.html` (after migration) | Copy favicon links from `base_enhanced.html` to the canonical base. | ✅ Professional appearance in browser tabs | 🟢 Medium |

### 1.2 Consolidate CSS Files

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Audit v2.0 CSS usage | `static/css/style.css` (3161 lines) | Identify all classes used by legacy templates. Map each to v3.0 equivalent (e.g. `.sidebar` → `.sidebar-nav-enhanced`, `.nav-link` → `.nav-link-enhanced`). Document unmapped classes. | ✅ Clear migration path; no unexpected breakage | 🔴 Critical |
| Deprecate v2.0 style.css | `static/css/style.css` | Remove from all template `{% block extra_css %}` and `<link>` tags. Replace with v3.0 equivalents (`design-system.css`, `components.css`, `theme-system.css`). Keep file as backup but remove from pipeline. | ✅ Eliminates CSS conflicts, reduces payload by ~3161 lines | 🔴 Critical |
| Resolve overlapping token values | `frontend/static/css/design-system.css`, `static/css/style.css` | Audit all overlapping CSS custom properties (especially `--info-*` palette which diverges). Standardize on v3.0 values. Update any v2.0-dependent code that uses diverged values. | ✅ Consistent `--info-600` color across all components | 🔴 Critical |
| Consolidate module-specific inline CSS | `templates/companies/company_list.html`, `templates/org/departments.html`, `templates/employees/index.html`, `templates/hr/hr_dashboard.html`, `templates/attendance/dashboard.html` | Remove all inline `<style>` blocks from these templates. Move reusable styles to `components.css`. Replace `.hr-stat-card`, `.employee-stat-card`, `.attendance-stat-card`, `.company-card`, `.department-card` with a single `.stats-card` component. | ✅ Visual consistency across modules; -300+ lines of duplicate CSS | 🟠 High |
| Merge validation/error styles | All template inline CSS | Standardize `.is-invalid`, `.invalid-feedback`, `.valid-feedback` usage across all forms. Remove duplicate definitions. | ✅ Unified validation appearance | 🟢 Medium |

### 1.3 Fix Critical Form Templates

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Rewrite leave form with v3.0 components | `templates/leaves/employee_leave_form.html` | Replace `{{ form.as_p }}` with `{% include "components/form_field.html" %}` for each field. Add breadcrumb, page header card, submit/cancel button group with loading states. Add CSRF and validation display. | ✅ From "looks like an error page" to a proper styled form | 🔴 Critical |
| Rewrite leave list with v3.0 table | `templates/leaves/employee_leave_list.html` | Replace bare `<table>` with `{% include "components/data_table.html" %}`. Add search, sortable columns, pagination, responsive wrapper, empty state with icon. | ✅ Usable data browsing instead of a raw table | 🔴 Critical |
| Add page headers to both | `templates/leaves/employee_leave_form.html`, `templates/leaves/employee_leave_list.html` | Use `{% block page_title %}`, `{% block breadcrumb_title %}`, `{% block page_description %}` from enhanced base. | ✅ Contextual page identification | 🟠 High |

### 1.4 Fix Sidebar Navigation URLs

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Replace `#` hash links with real URLs | `templates/base.html` sidebar (lines 483-710) | Map each `href="#"` to actual Django URL names. Use `{% url %}` tags. Where views don't exist yet, create stub views or link to admin. | ✅ Navigation actually navigates; no dead clicks | 🔴 Critical |
| Add active state detection | `templates/base.html` sidebar | Apply `{% if request.resolver_match.app_name == '...' and ... %}` patterns to all nav links for `.active` class. | ✅ Visual indication of current page in sidebar | 🟠 High |
| Port sidebar to v3.0 markup | `templates/base.html` → `frontend/templates/base/base_enhanced.html` | Convert sidebar HTML from v2.0 classes (`.sidebar`, `.nav-link`, `.nav-icon`) to v3.0 classes (`.sidebar-nav-enhanced`, `.nav-link-enhanced`, `.nav-icon-enhanced`). | ✅ Consistent sidebar styling with theme support | 🟠 High |

### 1.5 Critical Accessibility Fixes

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add skip-to-content link | `templates/base.html` (line 441) | Copy from `base_enhanced.html`: `<a class="skip-to-content" href="#mainContent">تخطى إلى المحتوى</a>` | ✅ Keyboard users can bypass navigation | 🔴 Critical |
| Add `lang="ar"` on all pages | All templates with `<html>` tag | Ensure every page has `<html lang="ar" dir="rtl">`. | ✅ Screen readers detect Arabic language | 🔴 Critical |
| Add `aria-label` to icon buttons | All table action buttons, sidebar toggle | Add `aria-label="تعديل"`, `aria-label="حذف"`, `aria-label="فتح القائمة"` etc. | ✅ Screen readers announce icon button purpose | 🟠 High |
| Add `aria-hidden="true"` to decorative icons | All `<i class="fas ...">` icons used decoratively | Audit and add `aria-hidden="true"` to all Font Awesome icons that are not accompanied by visible text. | ✅ Screen readers skip irrelevant decorative content | 🟠 High |

**Phase 1 Exit Criteria:**
- All templates extend a single base template
- `static/css/style.css` no longer loaded on any page
- `employee_leave_form.html` and `employee_leave_list.html` are fully styled
- Every sidebar link has a real URL
- Skip-to-content link present on all pages

---

## Phase 2: Design System Enforcement (Days 3-4)

**Goal:** Make every page visually consistent with the v3.0 design system.

### 2.1 Design Token Application

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Replace all inline gradients with CSS classes | `templates/companies/company_list.html` (purple gradient), `templates/org/departments.html` (purple gradient), `templates/employees/index.html` (amber gradient), `templates/hr/hr_dashboard.html` (blue gradient), `templates/attendance/dashboard.html` (blue gradient), `templates/inventory/product_form.html` (indigo gradient), `templates/reporting/dashboard.html` | Remove all `style="background: linear-gradient(...)"` in HTML and `<style>` blocks. Replace with `.card-primary`, `.stats-card`, or `.page-header-enhanced` classes that use `--primary-*` tokens. | ✅ Unified blue primary color — no more color shock when switching modules | 🟠 High |
| Standardize card components | `templates/companies/company_list.html`, `templates/org/departments.html`, `templates/employees/index.html`, `templates/hr/hr_dashboard.html`, `templates/attendance/dashboard.html`, `templates/reporting/dashboard.html`, `templates/payrolls/dashboard.html`, `templates/insurance/dashboard.html`, `templates/evaluations/dashboard.html` | Replace `.company-card`, `.department-card`, `.employee-stat-card`, `.hr-stat-card`, `.attendance-stat-card` with `.stats-card` component. Use `{% include "components/stats_card.html" %}` with appropriate `stats_type` parameter. | ✅ Same card component renders consistently everywhere; hover effects, shadows, transitions identical | 🟠 High |
| Apply typography tokens | All templates | Ensure headings use `--font-size-3xl` (h1), `--font-size-2xl` (h2), `--font-size-xl` (h3) from design system. Remove ad-hoc `font-size: 24px` etc. | ✅ Consistent typographic hierarchy | 🟢 Medium |

### 2.2 Standardize Buttons, Forms, Tables

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Audit and normalize all button variants | All templates | Replace non-standard buttons with `.btn-primary`, `.btn-secondary`, `.btn-success`, `.btn-error`, `.btn-ghost`, `.btn-outline-primary`. Ensure all use v3.0 sizes: `.btn-xs`, `.btn-sm`, `.btn-lg`, `.btn-xl`. | ✅ Predictable button appearance and interaction | 🟠 High |
| Standardize table classes | All list templates | Replace bare `class="table"` with `class="table-enhanced"` and wrap in `class="table-responsive"`. Apply `.table-striped` consistently. | ✅ Visual consistency on all data tables | 🟠 High |
| Standardize form inputs | All form templates | Replace `class="form-control"` with `class="form-control-enhanced"` where appropriate. Ensure all inputs have `.form-group-enhanced` wrapping. | ✅ Modern input appearance with 2px border | 🟢 Medium |

### 2.3 Page Headers & Breadcrumbs

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add consistent page headers | All templates that extend base | Use `{% block page_title %}`, `{% block breadcrumb_title %}`, `{% block page_description %}` in every template. Ensure the enhanced base template renders these as a `.page-header-enhanced` section. | ✅ Every page has a clear title, breadcrumb trail, and description | 🟠 High |
| Implement breadcrumbs on all pages | All templates | Populate `{% block breadcrumb_items %}` with the correct breadcrumb trail (e.g. Home → Employees → Edit Employee). Use `<li class="breadcrumb-item"><a href="...">...</a></li>` pattern. | ✅ Users always know where they are in the system | 🟠 High |
| Remove flat `<h1>` tags | `templates/companies/company_detail.html`, `templates/leaves/leave_type_list.html`, `templates/leaves/public_holiday_list.html` | Replace standalone `<h1>{{ title }}</h1>` with the page header block system. | ✅ Consistent page header appearance | 🟢 Medium |

### 2.4 Theme System Integration

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add theme toggle to navbar | `templates/base.html` → copied to `base_enhanced.html` | Add theme toggle button using `[data-theme="dark"]` toggle from `theme-system.css`. Ensure all pages respect the theme preference. | ✅ Dark mode support across entire system | 🟢 Medium |
| Store theme preference | `frontend/static/js/` | Save theme selection to `localStorage` so it persists across sessions. | ✅ User preference remembered | 🟢 Low |

**Phase 2 Exit Criteria:**
- No inline gradient styles remain in any template
- All module-specific stat cards replaced with unified `.stats-card` component
- Every page has breadcrumbs via the enhanced base template
- Dark/light theme toggle functional

---

## Phase 3: Dashboard Enhancement (Day 5)

**Goal:** Transform the legacy dashboard into a modern KPI-driven control center.

### 3.1 Replace Legacy Dashboard

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Archive old dashboard | `templates/home_dashboard.html` | Move to `templates/archived/home_dashboard_backup.html`. Add deprecation notice. | ✅ Removes 1832 lines of unmaintainable code from active use | 🔴 Critical |
| Migrate to new dashboard | `frontend/templates/pages/dashboard.html`, `accounts/views.py` (or equivalent) | Set `frontend/templates/pages/dashboard.html` as the new home page. Update the URL configuration and view to point to the new dashboard. Ensure the view passes the required context variables. | ✅ Clean, component-based dashboard replaces monolithic legacy page | 🔴 Critical |

### 3.2 KPI Stat Cards

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add real metrics to stats cards | `frontend/templates/pages/dashboard.html`, context view | Wire up `{% include "components/stats_card.html" %}` with actual backend data: `total_employees`, `present_today`, `pending_leaves`, `pending_evaluations`, `active_loans`. Each card gets an icon, color variant, trend indicator. | ✅ At-a-glance KPI overview replaces empty stat boxes | 🟠 High |
| Add Chart.js visualizations | `frontend/templates/pages/dashboard.html` | Chart.js is already loaded but unused. Add three charts: employee distribution by department (pie), attendance trend last 30 days (line), leave balance by type (bar). Use `canvas` elements with data passed from Django context as JSON. | ✅ Visual data comprehension — trends visible immediately | 🟠 High |

### 3.3 Quick Action Cards

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add contextual quick actions | `frontend/templates/pages/dashboard.html` | Create action cards for: New Employee, New Leave Request, Process Payroll, Generate Report. Each card has an icon, description, and working URL. Group by role/permission. | ✅ Common tasks accessible in 1 click from dashboard | 🟢 Medium |

### 3.4 Activity Feed & Notifications

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add recent activity feed | `frontend/templates/pages/dashboard.html`, context view | Add "آخر النشاطات" section showing 10 most recent system actions (employee added, leave approved, etc.) using a timeline-style list. | ✅ Users see what's happening in the system | 🟢 Medium |
| Add notification summary | `frontend/templates/pages/dashboard.html`, context view | Add pending items summary: leave requests awaiting approval, pending evaluations, overdue tasks. Show counts with links to respective pages. | ✅ Users see what needs their attention | 🟢 Medium |

**Phase 3 Exit Criteria:**
- `home_dashboard.html` is archived, new dashboard is the default
- 4-6 KPI stat cards display real data
- 3 Chart.js charts render with backend data
- Quick actions, activity feed, and notification summary sections exist

---

## Phase 4: Table Experience (Days 6-7)

**Goal:** Every data table is searchable, sortable, paginated, exportable, and responsive.

### 4.1 Table Component Audit

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Identify all table templates | All templates with `<table>` or `data_table` include | Inventory of all tables across the system. Categorize as: enhanced table already, bare table, card-view-only. | ✅ Complete picture of table work needed | 🔴 Critical |

### 4.2 Apply Table Enhancements

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add search to all tables | `templates/leaves/leave_type_list.html`, `templates/leaves/public_holiday_list.html`, `templates/leaves/employee_leave_list.html`, `templates/administrator/user_list.html`, `templates/administrator/group_list.html`, `templates/administrator/department_list.html`, `templates/administrator/module_list.html`, `audit/templates/audit/auditlog_list.html`, `apps/projects/tasks/templates/tasks/unified_task_list.html`, `apps/projects/tasks/templates/tasks/task_list.html`, `apps/projects/tasks/templates/tasks/my_tasks.html`, `apps/projects/tasks/templates/tasks/completed_tasks.html`, `notifications/templates/notifications/list.html`, `apps/inventory/templates/inventory/invoice_detail.html` (product table) | Add search input above each table with JavaScript client-side filtering (or server-side via AJAX for large datasets). Use the `.table-search` pattern from design system. | ✅ Find records instantly instead of scanning | 🟠 High |
| Add pagination to all tables | Same as above | Add Django `Paginator` in views and render pagination controls using `{% include "components/pagination.html" %}` (or Bootstrap `.pagination`). Show "Page X of Y" and items-per-page selector. | ✅ No more infinite scroll through hundreds of rows | 🟠 High |
| Add sortable column headers | Same as above | Add `class="sortable"` to `<th>` elements with click-to-sort. Implement client-side or server-side sorting. Show asc/desc indicator icon. | ✅ Users can reorder data by any column | 🟢 Medium |
| Add export (CSV/Excel) buttons | Same as above | Add "تصدير CSV" and "تصدير Excel" buttons above the table. Implement server-side export endpoints or client-side table-to-CSV conversion. | ✅ Data can be downloaded for offline analysis | 🟢 Medium |
| Add responsive wrapper | Same as above | Wrap all tables in `<div class="table-responsive">` | ✅ Tables scroll horizontally on mobile instead of breaking layout | 🟠 High |

### 4.3 Standardize Action Buttons

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Replace bare action links | All table templates | Replace `<a class="btn btn-sm btn-secondary">` and `<a class="btn btn-sm btn-danger">` with `.table-action-btn.btn-view`, `.table-action-btn.btn-edit`, `.table-action-btn.btn-delete` from design system. Use icon-only buttons with `aria-label`. | ✅ Clean, consistent action column appearance | 🟢 Medium |
| Add delete confirmation modals | All list templates with delete actions | Wire delete buttons to trigger the `{% include "components/modal.html" %}` confirmation dialog instead of navigating directly to a delete page. | ✅ Prevents accidental deletions, better UX flow | 🟠 High |

### 4.4 Empty States

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add illustration-based empty states | All list templates | Replace bare `<tr><td colspan="N">لا توجد بيانات</td></tr>` with a proper empty state component containing an SVG illustration and helpful message. | ✅ Empty tables look intentional, not broken | 🟢 Medium |

**Phase 4 Exit Criteria:**
- Every list template has search, pagination, sorting
- Export buttons present on all data tables
- All tables are responsive
- Delete confirmations use modal dialogs

---

## Phase 5: Form Enhancement (Days 8-9)

**Goal:** Every form follows a consistent pattern with proper validation, help text, and responsive layout.

### 5.1 Form Audit & Migration

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Audit all form templates | All templates with `<form>` tags | Inventory of form rendering methods: `{{ form.as_p }}`, manual grid, component include. | ✅ Baseline for migration | 🔴 Critical |
| Convert `{{ form.as_p }}` forms | `templates/leaves/public_holiday_form.html`, `templates/leaves/leave_type_form.html`, `templates/companies/company_form.html`, `templates/org/branch_add.html`, `templates/org/department_add.html`, `templates/org/job_add.html` | Replace with `{% include "components/form_field.html" %}` for each field. Add field grouping, proper validation display, and responsive grid. | ✅ Consistent form appearance and behavior | 🔴 Critical |

### 5.2 Form Section Grouping

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Group long forms into sections | `templates/inventory/product_form.html`, `templates/administrator/user_create.html`, `templates/administrator/user_edit.html`, `apps/projects/tasks/templates/tasks/task_form.html` | Wrap logical field groups in `<fieldset>` with `<legend>` or section cards with titles. Use `<div class="form-section">` pattern. Add section progress indicator for very long forms. | ✅ Long forms are scannable and less overwhelming | 🟠 High |
| Add floating labels | `templates/leaves/employee_leave_form.html`, `templates/companies/company_form.html` | Use `.form-floating-enhanced` pattern from design system for input fields. Labels animate up on focus. | ✅ Modern input UX, saves vertical space | 🟢 Medium |

### 5.3 Validation & Help Text

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add proper validation UI | All form templates | Use `.is-invalid` / `.is-valid` classes on inputs. Render `.invalid-feedback` / `.valid-feedback` divs for each field. Enable Bootstrap validation styles. | ✅ Clear error indication — users know exactly which field failed | 🟠 High |
| Add required field indicators | All form templates | Ensure all required fields have `required` class on label, `required` attribute on input, and visual indicator (red asterisk). | ✅ Users know which fields are mandatory | 🟠 High |
| Add help text consistently | All form templates | Render `field.help_text` below each input using `<small class="form-text text-muted">` or `.help-text-enhanced`. | ✅ Context-sensitive guidance reduces errors | 🟢 Medium |

### 5.4 Form Responsiveness

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Make all forms responsive | All form templates | Ensure form grids use Bootstrap responsive column classes: `<div class="col-md-6 col-lg-4">`. Test at 576px, 768px, 992px widths. | ✅ Forms usable on tablets and phones | 🟠 High |
| Add form submission loading states | All form templates | Ensure submit buttons show spinner and disable on click. Use `class="btn btn-primary" disabled` pattern with `<span class="spinner spinner-sm"></span>`. | ✅ Prevents double-submission, gives feedback | 🟢 Medium |

**Phase 5 Exit Criteria:**
- Zero `{{ form.as_p }}` usages remain
- All forms use `form_field.html` component or consistent manual pattern
- Required field indicators present on all fields
- Validation messages display correctly
- All forms work at mobile widths

---

## Phase 6: Mobile & Accessibility (Day 10)

**Goal:** Full responsive coverage and WCAG 2.1 AA compliance.

### 6.1 Mobile Responsiveness

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Test all pages at 375px, 768px, 1024px | All templates | Browser DevTools audit of every page. Document issues per page. | ✅ Baseline measurement | 🟠 High |
| Fix sidebar on mobile | `frontend/templates/base/base_enhanced.html`, `frontend/static/css/components.css` | Ensure sidebar slides off-screen at <768px, toggle button visible in navbar, overlay backdrop present, touch swipe to close. | ✅ Mobile users can navigate | 🟠 High |
| Fix table horizontal scroll | All table templates | Ensure `.table-responsive` wraps all tables. Test at 375px — if columns still overflow, implement collapsible row pattern (show key data, expand to see all). | ✅ Tables usable on phones | 🟠 High |
| Fix form inputs on mobile | All form templates | Ensure inputs don't overflow viewport; font-size ≥16px to prevent iOS zoom; proper spacing between stacked fields. | ✅ Forms usable on phones | 🟠 High |
| Fix navbar on mobile | `frontend/templates/base/base_enhanced.html` | Ensure navbar items don't overflow, search truncates gracefully, user menu is accessible. | ✅ Top navigation usable on small screens | 🟢 Medium |

### 6.2 Accessibility (WCAG 2.1 AA)

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add proper ARIA landmarks | `frontend/templates/base/base_enhanced.html`, all templates with `<nav>`, `<main>`, `<aside>` | Ensure `role="navigation"`, `role="main"`, `role="banner"`, `role="contentinfo"`, `role="complementary"` are present. Add `aria-label` to each landmark. | ✅ Screen reader navigation between sections | 🔴 Critical |
| Ensure keyboard navigation | All interactive templates | Test tab order through every page. Ensure: all interactive elements reachable by Tab, no focus traps, Escape closes modals, Enter activates links. Fix any broken tab order. | ✅ Keyboard-only users can operate the system | 🔴 Critical |
| Add focus indicators | `frontend/static/css/components.css`, all templates | Add `:focus-visible` styles for all interactive elements: `outline: 2px solid var(--primary-600); outline-offset: 2px;`. Ensure focus ring has sufficient contrast (3:1 minimum). | ✅ Keyboard users can see where they are | 🟠 High |
| Test with screen reader | All templates | Test with NVDA (Windows) or VoiceOver (macOS) on: navigation flow, form filling, table browsing, error announcement. Fix missing labels, unannounced changes, broken reading order. | ✅ Screen reader users can complete all tasks | 🔴 Critical |
| Add ARIA labels to all icon-only buttons | All templates | Audit every `<button>` and `<a>` that contains only an icon. Add `aria-label="..."`. | ✅ Icon purpose announced to screen readers | 🟠 High |
| Ensure color contrast compliance | All templates | Check all text/background combinations against WCAG AA (4.5:1 normal text, 3:1 large text). Fix failures in: navbar text on gradient, badge text, muted help text, placeholder text. | ✅ Readable for low-vision users | 🟠 High |

### 6.3 Internationalization (i18n) Check

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Ensure `dir="rtl"` and `lang="ar"` on all pages | All templates | Check every template's `<html>` tag. Verify RTL-aware CSS (padding/margin sides, float, text-align) works correctly. | ✅ Consistent RTL rendering | 🔴 Critical |
| Check mixed English/Arabic content | All templates | Verify English text (code snippets, system names) renders correctly in LTR within RTL context. Use `<bdi>` or `dir="ltr"` wrappers where needed. | ✅ No jumbled bidirectional text | 🟢 Medium |

**Phase 6 Exit Criteria:**
- All pages tested and working at 375px, 768px, 1024px
- WCAG 2.1 AA checklist passed (all criteria green from audit report §4.1)
- Keyboard navigation verified on 5 key user flows
- Screen reader test passed on 3 core tasks

---

## Phase 7: Performance UX (Day 11)

**Goal:** Perceived performance improvements and better feedback for system states.

### 7.1 Skeleton Loaders

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add skeleton loader to dashboard stats | `frontend/templates/pages/dashboard.html` | Add `.skeleton-card` placeholders that display while data loads. Replace with real content on DOMContentLoaded or AJAX completion. | ✅ Dashboard appears to load instantly; users don't stare at blank white space | 🟢 Medium |
| Add skeleton loader to tables | All list templates | Add `.skeleton` rows (3-5 placeholder rows) that display before table data renders. | ✅ Tables appear to load progressively | 🟢 Medium |

### 7.2 Empty States & Error Handling

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add empty state illustrations | All list templates | Create reusable `{% include "components/empty_state.html" %}` with SVG illustrations for: no data, no results (search), no notifications, no activities. Include helpful CTA button. | ✅ Empty pages feel intentional and helpful | 🟢 Medium |
| Improve error pages | `templates/errors/404.html`, `templates/errors/403.html`, `templates/errors/500.html` | Add branded illustration, search suggestion on 404, "report problem" link, and home button. Use enhanced base template. | ✅ Error pages are helpful, not dead ends | 🟢 Medium |
| Add toast notification system | `frontend/templates/base/base_enhanced.html` | Add `{% include "components/toast_container.html" %}` to base template. Replace all `alert()` calls with `window.showToast(message, type)` function. Types: `success`, `error`, `warning`, `info`. | ✅ Non-blocking feedback replaces disruptive `alert()` dialogs | 🟠 High |

### 7.3 Progress Indicators

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Add progress bars for timed operations | `templates/inventory/product_form.html` (file upload), payroll generation views | Add `.progress` with `.progress-bar` for long-running operations. Show percentage and estimated time. | ✅ Users know operations are progressing, not stuck | 🟢 Medium |
| Enhance loading overlay | `frontend/templates/base/base_enhanced.html` | Update `.loading-overlay-enhanced` with message text ("جاري التحميل...", "جاري الحفظ...", "جاري التصدير...") passed via showLoading function parameter. | ✅ Loading state communicates what's happening | 🟢 Medium |

### 7.4 CSS Delivery Optimization

| Task | Files | Changes | UX Impact | Priority |
|------|-------|---------|-----------|----------|
| Combine CSS files for production | Build pipeline (if any) or `base_enhanced.html` | In production, concatenate `design-system.css` + `components.css` + `theme-system.css` into a single `design-system.min.css`. Use Django's `compress` or static file pipeline. | ✅ Fewer HTTP requests, faster initial render | 🟢 Medium |
| Add `preload` for critical CSS | `frontend/templates/base/base_enhanced.html` | Add `<link rel="preload" href="{% static 'css/design-system.css' %}" as="style">` for above-the-fold styles. | ✅ Critical CSS loads before render-blocking resources | 🟢 Low |
| Add `defer` for non-critical JS | `frontend/templates/base/base_enhanced.html` | Add `<script defer>` for Bootstrap, jQuery, and custom JS that doesn't need to block rendering. | ✅ Faster initial page load | 🟢 Low |

**Phase 7 Exit Criteria:**
- Skeleton loaders on dashboard and all tables
- Toast notification system replaces all `alert()` calls
- Empty state components used everywhere
- Error pages enhanced with illustrations and CTAs

---

## Phase 8: Polish & Final QA (Day 12)

**Goal:** Ensure everything works together across browsers, devices, and edge cases.

### 8.1 Cross-Browser Testing

| Browser | Priority | Notes |
|---------|----------|-------|
| Chrome 120+ | 🔴 Critical | Primary development browser |
| Firefox 120+ | 🔴 Critical | Significant user base |
| Edge 120+ | 🟠 High | Chromium-based, but test RTL/PDF rendering |
| Safari 17+ | 🟠 High | iOS users, test touch interactions |

**Test checklist:**
- All form submissions work
- All table sorting/pagination/search works
- Sidebar toggle works
- Theme toggle works
- Modal dialogs open/close correctly
- Print preview works

### 8.2 Responsive Testing

| Device | Viewport | Priority |
|--------|----------|----------|
| iPhone SE | 375x667 | 🔴 Critical |
| iPhone 14 Pro | 390x844 | 🔴 Critical |
| iPad Air | 820x1180 | 🟠 High |
| iPad Pro 12.9" | 1024x1366 | 🟠 High |
| Desktop 1920x1080 | 1920x1080 | 🔴 Critical |

### 8.3 Accessibility Audit

| Criterion | Method | Passing Threshold |
|-----------|--------|-------------------|
| Automated audit | axe DevTools / WAVE | Zero critical/high violations |
| Color contrast | Contrast checker | 100% of text passes AA |
| Keyboard navigation | Manual test | All flows completable with Tab only |
| Screen reader | NVDA / VoiceOver | 3 core tasks completable |
| Focus indicators | Visual inspection | All interactive elements visible on focus |

### 8.4 Performance Review

| Metric | Tool | Target |
|--------|------|--------|
| First Contentful Paint (FCP) | Lighthouse | <1.5s |
| Largest Contentful Paint (LCP) | Lighthouse | <2.5s |
| Cumulative Layout Shift (CLS) | Lighthouse | <0.1 |
| Time to Interactive (TTI) | Lighthouse | <3.5s |
| CSS file size | Build output | <100KB total |
| JS file size | Build output | <150KB total |
| Lighthouse Performance Score | Lighthouse | ≥85 |

### 8.5 Final Consistency Check

| Check | Method |
|-------|--------|
| All pages use same base template | Grep all `{% extends %}` statements |
| No inline `<style>` blocks remain | Grep `<style>` across all templates |
| No inline `onclick` handlers remain | Grep `onclick=` across all templates |
| No `{{ form.as_p }}` usages remain | Grep `form.as_p` across all templates |
| No `alert()` calls in JS | Grep `alert(` across all JS files |
| No `href="#"` in navigation | Grep `href="#"` in sidebar templates |
| All pages have breadcrumbs | Manual navigation check |
| All forms have validation UI | Manual form submission test |
| All tables have responsive wrapper | Manual resize check |
| All icon buttons have aria-label | Grep `<button.*>` without `aria-label` |
| All decorative icons have aria-hidden | Grep `<i class="fas` without `aria-hidden` |

**Phase 8 Exit Criteria:**
- Automated accessibility audit: zero violations
- Lighthouse score ≥85 on all page types
- All consistency checks pass
- Sign-off from stakeholder review

---

## Risk Register

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Template migration breaks existing views | Medium | High | Run full test suite after Phase 1; test each migrated template individually |
| CSS consolidation removes needed v2.0 class | Medium | Medium | Use carefully; keep v2.0 file as backup; run visual regression on all pages |
| Dashboard replacement removes legacy features | Low | High | Audit all legacy dashboard features before archiving; port any missing functionality |
| Design changes not approved by stakeholders | Medium | Medium | Show progress at end of each phase; maintain v2.0 files until sign-off |
| Mobile changes break desktop layout | Low | Medium | Test on both viewport sizes after every CSS change |

---

## Success Metrics

| Metric | Current | Target | Measurement Method |
|--------|---------|--------|-------------------|
| Template inheritance consistency | 2 bases, mixed | 1 base, 100% consistent | `grep "extends"` on all templates |
| CSS payload | 3161 lines (v2.0) + 1254 (v3.0) = 4415 lines | ~3500 lines (consolidated v3.0) | `wc -l` on CSS files |
| Inline `<style>` blocks | **~15 templates** with inline CSS | 0 | `grep -r "<style>" templates/` |
| `href="#"` in sidebar | **~20 links** | 0 | `grep -r 'href="#"' templates/` |
| `{{ form.as_p }}` usages | **2+ templates** | 0 | `grep -r "form.as_p" templates/` |
| Accessible pages (ARIA landmarks) | Only base_enhanced.html | All pages | Axe DevTools audit |
| Mobile-responsive tables | ~3 tables | All tables | Manual resize check |
| Lighthouse Performance score | Unknown | ≥85 | Lighthouse CI |
| Toast notifications replacing `alert()` | 0 | All feedback uses toasts | `grep "alert("` in JS files |

---

## File Reference Quick Map

| Path | Contents | Role |
|------|----------|------|
| `templates/base.html` | v2.0 base template (1062 lines) | 🔴 To be replaced by `base_enhanced.html` |
| `frontend/templates/base/base_enhanced.html` | v3.0 base template (731 lines) | ✅ Canonical base template — will be the single source |
| `templates/home_dashboard.html` | Legacy dashboard (1832 lines) | 🔴 To be archived and replaced |
| `frontend/templates/pages/dashboard.html` | New dashboard (1641 lines) | ✅ Replacement dashboard — needs KPI wiring |
| `frontend/templates/components/data_table.html` | Enhanced table component | ✅ Use for all list views |
| `frontend/templates/components/form_field.html` | Enhanced form field component | ✅ Use for all form fields |
| `frontend/templates/components/stats_card.html` | KPI stat card component | ✅ Use for all module dashboards |
| `frontend/templates/components/modal.html` | Modal dialog component | ✅ Use for all confirmations/forms in modals |
| `frontend/static/css/design-system.css` | v3.0 design tokens (1254 lines) | ✅ Canonical CSS — single source of tokens |
| `frontend/static/css/components.css` | v3.0 component styles (1797 lines) | ✅ Single source for component classes |
| `frontend/static/css/theme-system.css` | Dark/light theme (594 lines) | ✅ Theme system — extend if needed |
| `static/css/style.css` | v2.0 design system (3161 lines) | 🔴 To be deprecated and removed |
| `templates/leaves/employee_leave_form.html` | Bare form (10 lines) | 🔴 Critical fix needed |
| `templates/leaves/employee_leave_list.html` | Bare table (30 lines) | 🔴 Critical fix needed |

---

## Appendix: Priority Legend

| Priority | Color | Definition | Action |
|----------|-------|-----------|--------|
| 🔴 Critical | Red | Blocks user task, security risk, major inconsistency | Must complete in this phase |
| 🟠 High | Orange | Significant UX degradation, development friction | Should complete in this phase |
| 🟢 Medium | Green | Visual inconsistency, minor usability issue | Complete if time permits this phase |
| ⚪ Low | Gray | Enhancement, nice-to-have | Schedule for future iteration |

---

*End of Roadmap — Next action: Review and stakeholder sign-off before Phase 1 execution.*
