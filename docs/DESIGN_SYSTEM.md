# ElDawliya HRMS Design System v3.0

**نظام الدولية للموارد البشرية — نظام التصميم الموحد**

A comprehensive design system for the Arabic HRMS platform, built on Bootstrap 5 RTL with CSS custom properties (design tokens).

---

## Table of Contents

1. [Color System](#1-color-system)
2. [Typography System](#2-typography-system)
3. [Spacing System](#3-spacing-system)
4. [Components](#4-components)
5. [Navigation](#5-navigation)
6. [Responsive Breakpoints](#6-responsive-breakpoints)
7. [Shadows](#7-shadows)
8. [Border Radius](#8-border-radius)

---

## 1. Color System

All colors are defined as CSS custom properties on `:root` for the light theme and `[data-theme="dark"]` for the dark theme. Colors follow a 10-step scale (50–900) per hue.

### 1.1 Primary — Blue (`#2563eb`)

Used for primary actions, navigation highlights, and key interactive elements.

| Token | Hex | Usage |
|-------|-----|-------|
| `--primary-50` | `#eff6ff` | Background tint |
| `--primary-100` | `#dbeafe` | Badge/alert backgrounds |
| `--primary-200` | `#bfdbfe` | Alert borders |
| `--primary-300` | `#93c5fd` | Hover borders |
| `--primary-400` | `#60a5fa` | Light accent |
| `--primary-500` | `#3b82f6` | Focus ring |
| `--primary-600` | `#2563eb` | **Default** — buttons, links, active states |
| `--primary-700` | `#1d4ed8` | Hover state |
| `--primary-800` | `#1e40af` | Active/pressed state |
| `--primary-900` | `#1e3a8a` | Dark text accent |

### 1.2 Secondary — Slate (`#64748b`)

Used for secondary actions, neutral UI, and muted elements.

| Token | Hex | Usage |
|-------|-----|-------|
| `--gray-50` | `#f8fafc` | Page background |
| `--gray-100` | `#f1f5f9` | Card headers, hover rows |
| `--gray-200` | `#e2e8f0` | Borders, dividers |
| `--gray-300` | `#cbd5e1` | Input borders |
| `--gray-400` | `#94a3b8` | Disabled text, placeholder |
| `--gray-500` | `#64748b` | **Default** — secondary buttons |
| `--gray-600` | `#475569` | Muted text |
| `--gray-700` | `#334155` | Body text |
| `--gray-800` | `#1e293b` | **Dark** — headings |
| `--gray-900` | `#0f172a` | Strong text, dark backgrounds |

### 1.3 Success — Green (`#16a34a`)

Used for positive confirmations, completion states, and success feedback.

| Token | Hex | Usage |
|-------|-----|-------|
| `--success-50` | `#f0fdf4` | Background |
| `--success-100` | `#dcfce7` | Badge/alert bg |
| `--success-200` | `#bbf7d0` | Border |
| `--success-300`–`--success-500` | Scale | Hover/accents |
| `--success-600` | `#16a34a` | **Default** — buttons, badges |
| `--success-700`–`--success-900` | Scale | Dark accents |

### 1.4 Warning — Amber (`#d97706`)

Used for alerts, cautions, and pending states.

| Token | Hex |
|-------|-----|
| `--warning-50` | `#fffbeb` |
| `--warning-100` | `#fef3c7` |
| `--warning-200` | `#fde68a` |
| `--warning-300`–`--warning-500` | Scale |
| `--warning-600` | `#d97706` **(Default)** |
| `--warning-700`–`--warning-900` | Scale |

### 1.5 Danger — Red (`#dc2626`)

Used for errors, destructive actions, and critical states.

| Token | Hex |
|-------|-----|
| `--error-50` | `#fef2f2` |
| `--error-100` | `#fee2e2` |
| `--error-200` | `#fecaca` |
| `--error-300`–`--error-500` | Scale |
| `--error-600` | `#dc2626` **(Default)** |
| `--error-700`–`--error-900` | Scale |

### 1.6 Info — Sky (`#0284c7`)

Used for informational messages and neutral notifications.

| Token | Hex |
|-------|-----|
| `--info-50` | `#f0f9ff` |
| `--info-100` | `#e0f2fe` |
| `--info-200` | `#bae6fd` |
| `--info-300`–`--info-500` | Scale |
| `--info-600` | `#0284c7` **(Default)** |
| `--info-700`–`--info-900` | Scale |

### 1.7 Purple — AI Accent (`#9333ea`)

Used for AI-related features and special accents.

| Token | Hex |
|-------|-----|
| `--purple-50`–`--purple-900` | `#faf5ff` → `#581c87` |

### 1.8 Text Color Utility Classes

```css
.text-primary { color: var(--primary-600); }
.text-secondary { color: var(--gray-600); }
.text-success { color: var(--success-600); }
.text-warning { color: var(--warning-600); }
.text-error { color: var(--error-600); }
.text-info { color: var(--info-600); }
.text-gray-50  ... .text-gray-900  /* full gray scale */
```

### 1.9 Background Utility Classes

```css
.bg-primary { background-color: var(--primary-600); }
.bg-secondary { background-color: var(--gray-600); }
.bg-success { background-color: var(--success-600); }
.bg-warning { background-color: var(--warning-600); }
.bg-error { background-color: var(--error-600); }
.bg-info { background-color: var(--info-600); }
.bg-gray-50 ... .bg-gray-900  /* full gray scale */
```

### 1.10 Dark Theme

The `[data-theme="dark"]` selector inverts the gray scale and adjusts surface/background colors:

```css
[data-theme="dark"] {
    --gray-50: #0f172a;     /* darkest becomes page bg */
    --gray-100: #1e293b;
    --gray-200: #334155;
    --gray-300: #475569;
    --gray-400: #64748b;
    --gray-500: #94a3b8;
    --gray-600: #cbd5e1;
    --gray-700: #e2e8f0;
    --gray-800: #f1f5f9;
    --gray-900: #f8fafc;    /* lightest becomes text */
    --bg-primary: var(--gray-900);
    --bg-secondary: var(--gray-800);
    --bg-tertiary: var(--gray-700);
    --text-primary: var(--gray-100);
    --text-secondary: var(--gray-300);
    --text-tertiary: var(--gray-400);
    --border-primary: var(--gray-700);
    --border-secondary: var(--gray-600);
}
```

---

## 2. Typography System

### 2.1 Font Families

| Token | Value | Usage |
|-------|-------|-------|
| `--font-family-arabic` | `'Cairo', 'Segoe UI', Tahoma, sans-serif` | Arabic body/headings |
| `--font-family-english` | `'Inter', 'Segoe UI', Tahoma, sans-serif` | English content |
| `--font-family-mono` | `'JetBrains Mono', 'Fira Code', 'Consolas', monospace` | Code/monospace |

### 2.2 Font Sizes

| Token | Value | Class |
|-------|-------|-------|
| `--font-size-xs` | `0.75rem` (12px) | `.text-xs` |
| `--font-size-sm` | `0.875rem` (14px) | `.text-sm` |
| `--font-size-base` | `1rem` (16px) | `.text-base` |
| `--font-size-lg` | `1.125rem` (18px) | `.text-lg` |
| `--font-size-xl` | `1.25rem` (20px) | `.text-xl` |
| `--font-size-2xl` | `1.5rem` (24px) | `.text-2xl` |
| `--font-size-3xl` | `1.875rem` (30px) | `.text-3xl` |
| `--font-size-4xl` | `2.25rem` (36px) | `.text-4xl` |
| `--font-size-5xl` | `3rem` (48px) | `.text-5xl` |
| `--font-size-6xl` | `3.75rem` (60px) | `.text-6xl` |

### 2.3 Font Weights

| Token | Value | Class |
|-------|-------|-------|
| `--font-weight-light` | `300` | `.font-light` |
| `--font-weight-normal` | `400` | `.font-normal` |
| `--font-weight-medium` | `500` | `.font-medium` |
| `--font-weight-semibold` | `600` | `.font-semibold` |
| `--font-weight-bold` | `700` | `.font-bold` |
| `--font-weight-extrabold` | `800` | `.font-extrabold` |
| `--font-weight-black` | `900` | `.font-black` |

### 2.4 Line Heights

| Token | Value | Class |
|-------|-------|-------|
| `--line-height-tight` | `1.25` | `.leading-tight` |
| `--line-height-snug` | `1.375` | `.leading-snug` |
| `--line-height-normal` | `1.5` | `.leading-normal` |
| `--line-height-relaxed` | `1.625` | `.leading-relaxed` |
| `--line-height-loose` | `2` | `.leading-loose` |

### 2.5 Heading Hierarchy

| Level | Size | Weight | Token |
|-------|------|--------|-------|
| **H1** | `1.875rem` (30px) | Bold (700) | `--font-size-3xl` |
| **H2** | `1.5rem` (24px) | Bold (700) | `--font-size-2xl` |
| **H3** | `1.25rem` (20px) | Semibold (600) | `--font-size-xl` |
| **H4** | `1.125rem` (18px) | Semibold (600) | `--font-size-lg` |
| **H5** | `1rem` (16px) | Medium (500) | `--font-size-base` |
| **H6** | `0.875rem` (14px) | Medium (500) | `--font-size-sm` |
| **Body** | `1rem` (16px) | Regular (400) | `--font-size-base` |
| **Small** | `0.875rem` (14px) | Regular (400) | `--font-size-sm` |
| **Caption** | `0.75rem` (12px) | Regular (400) | `--font-size-xs` |

### 2.6 Base Body Styles

```css
body {
    font-family: var(--font-family-arabic);
    font-size: var(--font-size-base);
    font-weight: var(--font-weight-normal);
    line-height: var(--line-height-normal);
    color: var(--gray-900);
    background-color: var(--gray-50);
    direction: rtl;
    text-align: right;
    -webkit-font-smoothing: antialiased;
}
```

---

## 3. Spacing System

### 3.1 Spacing Scale (4px base)

| Token | Rem | Pixels | Usage |
|-------|-----|--------|-------|
| `--spacing-0` | `0` | `0` | None |
| `--spacing-px` | `1px` | `1px` | Border |
| `--spacing-0-5` | `0.125rem` | `2px` | Micro |
| `--spacing-1` | `0.25rem` | `4px` | Tight |
| `--spacing-1-5` | `0.375rem` | `6px` | Extra-small |
| `--spacing-2` | `0.5rem` | `8px` | Small |
| `--spacing-2-5` | `0.625rem` | `10px` | Input padding |
| `--spacing-3` | `0.75rem` | `12px` | Compact |
| `--spacing-3-5` | `0.875rem` | `14px` | Tight |
| `--spacing-4` | `1rem` | `16px` | **Base unit** |
| `--spacing-5` | `1.25rem` | `20px` | Comfortable |
| `--spacing-6` | `1.5rem` | `24px` | Card padding |
| `--spacing-7` | `1.75rem` | `28px` | Spacious |
| `--spacing-8` | `2rem` | `32px` | **Section spacing** |
| `--spacing-9` | `2.25rem` | `36px` | Generous |
| `--spacing-10` | `2.5rem` | `40px` | Wide |
| `--spacing-11` | `2.75rem` | `44px` | Extra |
| `--spacing-12` | `3rem` | `48px` | Large section |
| `--spacing-14` | `3.5rem` | `56px` | Extra large |
| `--spacing-16` | `4rem` | `64px` | Page section |
| `--spacing-20` | `5rem` | `80px` | Hero section |
| `--spacing-24` | `6rem` | `96px` | Ultra |
| `--spacing-28` | `7rem` | `112px` | Maximum |
| `--spacing-32` | `8rem` | `128px` | Extreme |

### 3.2 Grid System

- **12-column CSS Grid** via Bootstrap 5 grid + custom `.grid` utility
- Default gap: `--spacing-4` (1rem)
- Column classes: `.grid-cols-1` through `.grid-cols-12`
- Span classes: `.col-span-1` through `.col-span-full`
- Responsive grid suffixes: `-sm`, `-md`, `-lg`, `-xl`

### 3.3 Key Spacing Conventions

| Context | Value | Token |
|---------|-------|-------|
| Section spacing | `2rem` | `--spacing-8` |
| Card padding (body) | `1.5rem` | `--spacing-6` |
| Card padding (header/footer) | `1rem 1.5rem` | `--spacing-4`/`--spacing-6` |
| Form group margin | `1rem` | `--spacing-4` |
| Button padding (default) | `0.5rem 1rem` | `--spacing-2`/`--spacing-4` |
| Table cell padding | `0.75rem 1rem` | `--spacing-3`/`--spacing-4` |

### 3.4 Spacing Utility Classes

```css
.m-0 ... m-12          /* margin all sides */
.mt-0 ... mt-12        /* margin-top */
.mr-0 ... mr-12        /* margin-right */
.mb-0 ... mb-12        /* margin-bottom */
.ml-0 ... ml-12        /* margin-left */
.mx-0 ... mx-12        /* margin left + right */
.my-0 ... my-12        /* margin top + bottom */
.mx-auto               /* horizontal center */

.p-0 ... p-12          /* padding all sides */
.pt-0 ... pt-12        /* padding-top */
.pr-0 ... pr-12        /* padding-right */
.pb-0 ... pb-12        /* padding-bottom */
.pl-0 ... pl-12        /* padding-left */
.px-0 ... px-12        /* padding left + right */
.py-0 ... py-12        /* padding top + bottom */
```

---

## 4. Components

### 4.1 Buttons

#### 4.1.1 Base Button (`.btn`)

```css
.btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: var(--spacing-2);
    padding: var(--spacing-2) var(--spacing-4);
    border: 1px solid transparent;
    border-radius: var(--radius-md);
    font-size: var(--font-size-sm);
    font-weight: var(--font-weight-medium);
    line-height: var(--line-height-tight);
    cursor: pointer;
    transition: var(--transition-all);
    white-space: nowrap;
}
```

#### 4.1.2 Variants

| Class | Background | Hover | Active |
|-------|-----------|-------|--------|
| `.btn-primary` | `--primary-600` | `--primary-700` | `--primary-800` |
| `.btn-secondary` | `--gray-600` | `--gray-700` | — |
| `.btn-success` | `--success-600` | `--success-700` | — |
| `.btn-warning` | `--warning-600` | `--warning-700` | — |
| `.btn-error` | `--error-600` | `--error-700` | — |
| `.btn-info` | `--info-600` | `--info-700` | — |
| `.btn-ghost` | Transparent | `--gray-100` | — |
| `.btn-outline-primary` | Transparent, border `--primary-600` | Filled `--primary-600` | — |
| `.btn-outline-secondary` | Transparent, border `--gray-600` | Filled `--gray-600` | — |

#### 4.1.3 Sizes

| Size | Padding | Font Size | Class |
|------|---------|-----------|-------|
| Extra small | `0.25rem 0.5rem` | `0.75rem` | `.btn-xs` |
| Small | `0.375rem 0.75rem` | `0.875rem` | `.btn-sm` |
| Default | `0.5rem 1rem` | `0.875rem` | `.btn` |
| Large | `0.75rem 1.5rem` | `1.125rem` | `.btn-lg` |
| Extra large | `1rem 2rem` | `1.25rem` | `.btn-xl` |

#### 4.1.4 Icon Button (`.btn-icon`)

Square buttons with icon-only content.

| Size | Width/Height | Class |
|------|-------------|-------|
| Small | 32px | `.btn-icon.btn-sm` |
| Default | 40px | `.btn-icon` |
| Large | 48px | `.btn-icon.btn-lg` |
| XL | 56px | `.btn-icon.btn-xl` |

#### 4.1.5 Loading State

Use the `.spinner` element inside `.btn`:

```html
<button class="btn btn-primary" disabled>
    <span class="spinner spinner-sm"></span>
    جاري الحفظ...
</button>
```

#### 4.1.6 Usage Examples

```html
<!-- Standard -->
<button class="btn btn-primary">
    <i class="fas fa-save"></i>
    حفظ
</button>

<!-- Outline -->
<button class="btn btn-outline-primary">
    <i class="fas fa-times"></i>
    إلغاء
</button>

<!-- Ghost -->
<button class="btn btn-ghost">
    <i class="fas fa-ellipsis-v"></i>
</button>

<!-- Sizes -->
<button class="btn btn-success btn-xs">صغير جداً</button>
<button class="btn btn-success btn-sm">صغير</button>
<button class="btn btn-success btn-lg">كبير</button>
<button class="btn btn-success btn-xl">كبير جداً</button>

<!-- Button Group -->
<div class="btn-group-enhanced">
    <button class="btn btn-primary">أمر 1</button>
    <button class="btn btn-primary">أمر 2</button>
    <button class="btn btn-primary">أمر 3</button>
</div>

<!-- Floating Action Button -->
<button class="btn-fab" title="إضافة جديد">
    <i class="fas fa-plus"></i>
</button>
```

### 4.2 Forms

#### 4.2.1 Text Input (`.form-control`)

```css
.form-control {
    display: block;
    width: 100%;
    padding: var(--spacing-2-5) var(--spacing-3);
    border: 1px solid var(--gray-300);
    border-radius: var(--radius-md);
    font-size: var(--font-size-base);
    color: var(--gray-900);
    background-color: white;
    transition: var(--transition-colors);
}

.form-control:focus {
    border-color: var(--primary-500);
    box-shadow: 0 0 0 3px rgb(59 130 246 / 0.1);
}
```

#### 4.2.2 Enhanced Input (`.form-control-enhanced`)

Uses a 2px border and larger border-radius for a more modern look:

```css
.form-control-enhanced {
    padding: var(--spacing-3) var(--spacing-4);
    border: 2px solid var(--gray-300);
    border-radius: var(--radius-lg);
    transition: var(--transition-all);
}
.form-control-enhanced:focus {
    border-color: var(--primary-500);
    box-shadow: 0 0 0 4px rgb(59 130 246 / 0.1);
    transform: translateY(-1px);
}
```

#### 4.2.3 Input Sizes

| Size | Padding | Font Size | Class |
|------|---------|-----------|-------|
| Small | `0.375rem 0.625rem` | `0.875rem` | `.form-control-sm` |
| Default | `0.625rem 0.75rem` | `1rem` | `.form-control` |
| Large | `0.75rem 1rem` | `1.125rem` | `.form-control-lg` |

#### 4.2.4 Validation States

```css
.form-control.is-invalid {
    border-color: var(--error-500);
}
.form-control.is-valid {
    border-color: var(--success-500);
}
.invalid-feedback { color: var(--error-600); }
.valid-feedback { color: var(--success-600); }
```

#### 4.2.5 Select (`.form-select`)

Custom dropdown arrow using inline SVG positioned on the left (RTL-friendly `padding-left: 2.5rem`).

#### 4.2.6 Checkboxes & Radios (`.form-check`)

```html
<div class="form-check">
    <input class="form-check-input" type="checkbox" id="chk1">
    <label class="form-check-label" for="chk1">خيار 1</label>
</div>
```

#### 4.2.7 Input Group (`.input-group-enhanced`)

```html
<div class="input-group-enhanced">
    <input type="text" class="form-control-enhanced" placeholder="بحث...">
    <span class="input-group-text"><i class="fas fa-search"></i></span>
</div>
```

#### 4.2.8 Floating Label (`.form-floating-enhanced`)

```html
<div class="form-floating-enhanced">
    <input type="email" class="form-control-enhanced" id="email" placeholder="البريد الإلكتروني">
    <label class="form-label-enhanced" for="email">البريد الإلكتروني</label>
</div>
```

#### 4.2.9 Complete Form Example

```html
<form>
    <div class="form-group-enhanced">
        <label class="form-label-enhanced required">
            <i class="form-label-icon fas fa-user"></i>
            الاسم الكامل
        </label>
        <input type="text" class="form-control-enhanced" placeholder="أدخل الاسم">
        <div class="invalid-feedback">هذا الحقل مطلوب</div>
    </div>

    <div class="form-group">
        <label class="form-label">البريد الإلكتروني</label>
        <input type="email" class="form-control is-invalid" value="invalid">
        <div class="invalid-feedback">البريد الإلكتروني غير صحيح</div>
    </div>

    <div class="form-group-enhanced">
        <label class="form-label-enhanced">القسم</label>
        <select class="form-control-enhanced">
            <option>الموارد البشرية</option>
            <option>المالية</option>
            <option>تقنية المعلومات</option>
        </select>
    </div>
</form>
```

### 4.3 Tables

#### 4.3.1 Base Table (`.table`)

```css
.table {
    width: 100%;
    border-collapse: collapse;
    font-size: var(--font-size-sm);
    color: var(--gray-900);
}
.table th {
    background-color: var(--gray-50);
    font-weight: var(--font-weight-semibold);
    border-bottom: 2px solid var(--gray-200);
}
.table th, .table td {
    padding: var(--spacing-3) var(--spacing-4);
    text-align: right;
    border-bottom: 1px solid var(--gray-200);
}
.table tbody tr:hover {
    background-color: var(--gray-50);
}
```

#### 4.3.2 Variants

| Class | Description |
|-------|-------------|
| `.table` | Default unstyled |
| `.table-enhanced` | Bordered with shadow, rounded corners |
| `.table-striped` | Alternating row colors |
| `.table-bordered` | All cells bordered |
| `.table-responsive` | Horizontal scroll wrapper |

#### 4.3.3 Sortable Headers

```html
<th class="sortable">الاسم</th>
<th class="sortable asc">الراتب</th>     <!-- ascending -->
<th class="sortable desc">التاريخ</th>    <!-- descending -->
```

Sortable headers include a Font Awesome sort icon that changes on `.asc`/`.desc`.

#### 4.3.4 Action Columns

```html
<div class="table-actions">
    <button class="table-action-btn btn-view" title="عرض">
        <i class="fas fa-eye"></i>
    </button>
    <button class="table-action-btn btn-edit" title="تعديل">
        <i class="fas fa-edit"></i>
    </button>
    <button class="table-action-btn btn-delete" title="حذف">
        <i class="fas fa-trash"></i>
    </button>
</div>
```

Colors: `.btn-view` (info), `.btn-edit` (warning), `.btn-delete` (error).

#### 4.3.5 Table Search

```html
<div class="table-search">
    <input type="text" class="form-control-enhanced" placeholder="بحث...">
</div>
```

#### 4.3.6 Complete Table Example

```html
<div class="table-responsive">
    <table class="table-enhanced">
        <thead>
            <tr>
                <th class="sortable">#</th>
                <th class="sortable asc">الاسم</th>
                <th class="sortable">القسم</th>
                <th class="sortable">الحالة</th>
                <th>الإجراءات</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>أحمد محمد</td>
                <td>الموارد البشرية</td>
                <td><span class="badge badge-success">نشط</span></td>
                <td>
                    <div class="table-actions">
                        <button class="table-action-btn btn-view"><i class="fas fa-eye"></i></button>
                        <button class="table-action-btn btn-edit"><i class="fas fa-edit"></i></button>
                        <button class="table-action-btn btn-delete"><i class="fas fa-trash"></i></button>
                    </div>
                </td>
            </tr>
        </tbody>
    </table>
</div>
```

### 4.4 Cards

#### 4.4.1 Base Card (`.card`)

```css
.card {
    background-color: white;
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-sm);
    border: 1px solid var(--gray-200);
    overflow: hidden;
    transition: var(--transition-shadow);
}
.card:hover {
    box-shadow: var(--shadow-md);
}
```

#### 4.4.2 Card Sections

| Element | Padding | Background |
|---------|---------|------------|
| `.card-header` | `1rem 1.5rem` | `--gray-50` |
| `.card-body` | `1.5rem` | White |
| `.card-footer` | `1rem 1.5rem` | `--gray-50` |
| `.card-title` | Margin bottom `0.5rem` | — |
| `.card-subtitle` | Margin bottom `1rem`, `--gray-600` | — |

#### 4.4.3 Enhanced Card (`.card-enhanced`)

```css
.card-enhanced {
    border-radius: var(--radius-xl);
    box-shadow: var(--shadow-sm);
    border: 1px solid var(--gray-200);
    transition: var(--transition-all);
}
.card-enhanced:hover {
    box-shadow: var(--shadow-lg);
    transform: translateY(-2px);
}
```

#### 4.4.4 Card Color Variants

| Class | Border | Header Background |
|-------|--------|-------------------|
| `.card-primary` | `--primary-200` | Gradient `--primary-50` → `--primary-100` |
| `.card-interactive` | Default | Cursor pointer |

#### 4.4.5 Stats Card (`.stats-card`)

```html
<div class="stats-card stats-success">
    <div class="stats-header">
        <h4 class="stats-title">إجمالي الموظفين</h4>
        <div class="stats-icon stats-success">
            <i class="fas fa-users"></i>
        </div>
    </div>
    <p class="stats-value">1,234</p>
    <div class="stats-change positive">
        <i class="fas fa-arrow-up"></i>
        <span>+12% هذا الشهر</span>
    </div>
</div>
```

Top accent bar variants: `.stats-success`, `.stats-warning`, `.stats-error`, `.stats-info`.

#### 4.4.6 Usage Examples

```html
<!-- Standard card -->
<div class="card">
    <div class="card-header">
        <h3 class="card-title">
            <i class="card-title-icon fas fa-chart-bar text-primary"></i>
            تقرير الإجازات
        </h3>
    </div>
    <div class="card-body">
        <p class="card-text">محتوى التقرير...</p>
    </div>
    <div class="card-footer">
        <button class="btn btn-primary">تصدير</button>
    </div>
</div>

<!-- Enhanced card -->
<div class="card-enhanced card-primary">
    <div class="card-header-enhanced">
        <h3 class="card-title-enhanced">بيانات الموظف</h3>
        <div class="card-actions">
            <button class="btn btn-sm btn-ghost">
                <i class="fas fa-pen"></i>
            </button>
        </div>
    </div>
    <div class="card-body-enhanced">
        <!-- content -->
    </div>
</div>
```

### 4.5 Modals (`.modal-enhanced`)

#### 4.5.1 Structure

```html
<div class="modal-enhanced" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title">
                    <i class="fas fa-info-circle"></i>
                    عنوان النافذة
                </h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <!-- content -->
            </div>
            <div class="modal-footer">
                <button class="btn btn-primary">حفظ</button>
                <button class="btn btn-ghost" data-bs-dismiss="modal">إلغاء</button>
            </div>
        </div>
    </div>
</div>
```

#### 4.5.2 Sizes

| Size | Max-width | Class |
|------|-----------|-------|
| Small | 300px | `modal-sm` |
| Default | 600px | (default) |
| Large | 800px | `modal-lg` |
| Extra large | 1140px | `modal-xl` |

#### 4.5.3 Features

- `.modal-dialog-centered` — vertically center
- `.modal-dialog-scrollable` — scrollable body
- Static backdrop via `data-bs-backdrop="static"`

### 4.6 Alerts

#### 4.6.1 Basic Alerts (`.alert`)

```html
<div class="alert alert-primary">نص التنبيه</div>
<div class="alert alert-success">تم بنجاح</div>
<div class="alert alert-warning">انتباه</div>
<div class="alert alert-error">حدث خطأ</div>
<div class="alert alert-info">معلومات</div>
```

#### 4.6.2 Enhanced Alerts (`.alert-enhanced`)

```html
<div class="alert-enhanced alert-success">
    <i class="alert-icon fas fa-check-circle"></i>
    <div class="alert-content">
        <h4 class="alert-title">تم بنجاح!</h4>
        <p class="alert-message">تم حفظ البيانات بنجاح</p>
    </div>
</div>
```

#### 4.6.3 Dismissible Alert

```html
<div class="alert alert-primary alert-dismissible">
    <button class="alert-dismiss" data-bs-dismiss="alert">&times;</button>
    هذا تنبيه قابل للإغلاق
</div>
```

### 4.7 Badges (`.badge`)

```css
.badge {
    display: inline-flex;
    align-items: center;
    padding: var(--spacing-1) var(--spacing-2);
    font-size: var(--font-size-xs);
    font-weight: var(--font-weight-medium);
    border-radius: var(--radius-full);
    text-transform: uppercase;
    letter-spacing: 0.025em;
}
```

| Class | Background | Text Color |
|-------|-----------|------------|
| `.badge-primary` | `--primary-100` | `--primary-800` |
| `.badge-secondary` | `--gray-100` | `--gray-800` |
| `.badge-success` | `--success-100` | `--success-800` |
| `.badge-warning` | `--warning-100` | `--warning-800` |
| `.badge-error` | `--error-100` | `--error-800` |
| `.badge-info` | `--info-100` | `--info-800` |

```html
<span class="badge badge-success">
    <i class="fas fa-check-circle"></i>
    نشط
</span>
```

### 4.8 Pagination (`.pagination`)

| Size | Class |
|------|-------|
| Small | `.pagination-sm` |
| Default | `.pagination` |
| Large | `.pagination-lg` |

```html
<nav aria-label="Page navigation">
    <ul class="pagination">
        <li class="page-item disabled">
            <a class="page-link" href="#">السابق</a>
        </li>
        <li class="page-item active"><a class="page-link" href="#">1</a></li>
        <li class="page-item"><a class="page-link" href="#">2</a></li>
        <li class="page-item"><a class="page-link" href="#">3</a></li>
        <li class="page-item">
            <a class="page-link" href="#">التالي</a>
        </li>
    </ul>
</nav>
```

### 4.9 Tabs (`.nav-tabs`)

```html
<ul class="nav-tabs" role="tablist">
    <li class="nav-item" role="presentation">
        <button class="nav-link active" data-bs-toggle="tab" type="button">
            <i class="fas fa-info-circle"></i>
            الأساسية
        </button>
    </li>
    <li class="nav-item" role="presentation">
        <button class="nav-link" data-bs-toggle="tab" type="button">
            <i class="fas fa-address-card"></i>
            التفاصيل
        </button>
    </li>
</ul>
<div class="tab-content">
    <div class="tab-pane active" id="tab1">...</div>
    <div class="tab-pane" id="tab2">...</div>
</div>
```

**Responsive:** On screens <768px, tabs stack vertically.

### 4.10 Accordion (`.accordion`)

```html
<div class="accordion">
    <div class="accordion-item">
        <h2 class="accordion-header">
            <button class="accordion-button" type="button">
                <i class="fas fa-chevron-down"></i>
                القسم الأول
            </button>
        </h2>
        <div class="accordion-collapse show">
            <div class="accordion-body">المحتوى...</div>
        </div>
    </div>
</div>
```

### 4.11 Progress Bars (`.progress`)

```css
.progress {
    height: var(--spacing-2);
    background-color: var(--gray-200);
    border-radius: var(--radius-full);
    overflow: hidden;
}
.progress-bar {
    height: 100%;
    background-color: var(--primary-600);
    border-radius: var(--radius-full);
    transition: width var(--transition-normal);
}
```

| Color | Class |
|-------|-------|
| Primary | `.progress-bar` (default) |
| Success | `.progress-bar-success` |
| Warning | `.progress-bar-warning` |
| Error | `.progress-bar-error` |

```html
<div class="progress">
    <div class="progress-bar" style="width: 65%">65%</div>
</div>
```

### 4.12 Tooltips & Popovers

Uses Bootstrap 5 tooltip/popover components with design system theming:
- Background: `--gray-900`
- Text: white
- Border radius: `--radius-md`

### 4.13 Toast Notifications (`.toast`)

#### 4.13.1 Positioning

| Position | Class |
|----------|-------|
| Top left | `.toast-container.top-left` |
| Top right | `.toast-container.top-right` |
| Bottom left | `.toast-container.bottom-left` |
| Bottom right | `.toast-container.bottom-right` |

#### 4.13.2 Types

| Type | Border Color | Icon Color |
|------|-------------|------------|
| `.toast-success` | `--success-200` | `--success-600` |
| `.toast-warning` | `--warning-200` | `--warning-600` |
| `.toast-error` | `--error-200` | `--error-600` |
| `.toast-info` | `--info-200` | `--info-600` |

#### 4.13.3 Usage

```html
<div class="toast-container top-right">
    <div class="toast toast-success show">
        <div class="toast-content">
            <div class="toast-icon"><i class="fas fa-check-circle"></i></div>
            <div class="toast-body">
                <div class="toast-title">تم الحفظ</div>
                <div class="toast-message">تم حفظ البيانات بنجاح</div>
            </div>
            <button class="toast-close"><i class="fas fa-times"></i></button>
        </div>
        <div class="toast-progress"></div>
    </div>
</div>
```

Features: auto-dismiss progress bar, animation slide-in from right.

### 4.14 Loading States

#### 4.14.1 Spinner (`.spinner`)

| Size | Width/Height | Border Width | Class |
|------|-------------|--------------|-------|
| Small | 12px | 1px | `.spinner.spinner-sm` |
| Default | 16px | 2px | `.spinner` |
| Large | 24px | 3px | `.spinner.spinner-lg` |

#### 4.14.2 Skeleton Loaders (`.skeleton`)

```html
<div class="skeleton skeleton-title"></div>
<div class="skeleton skeleton-text"></div>
<div class="skeleton skeleton-paragraph"></div>
<div class="skeleton skeleton-avatar"></div>
<div class="skeleton skeleton-card"></div>
```

#### 4.14.3 Loading Overlay (`.loading-overlay-enhanced`)

```html
<div class="loading-overlay-enhanced show">
    <div class="loading-content">
        <div class="loading-spinner-enhanced"></div>
        <p class="loading-message">جاري التحميل...</p>
    </div>
</div>
```

Features: backdrop blur, fade transition, centered spinner + message.

---

## 5. Navigation

### 5.1 Sidebar (`.sidebar-nav-enhanced`)

#### 5.1.1 Structure

```css
.sidebar-nav-enhanced {
    width: 280px;
    height: 100vh;
    position: fixed;
    top: 0;
    right: 0;
    background: linear-gradient(135deg, var(--gray-900), var(--gray-800));
    z-index: var(--z-fixed);
    transition: var(--transition-all);
    box-shadow: var(--shadow-xl);
    display: flex;
    flex-direction: column;
    overflow: hidden;
}
```

#### 5.1.2 States

| State | Width | Class |
|-------|-------|-------|
| Expanded | 280px | `.sidebar-nav-enhanced` |
| Collapsed | 80px | `.sidebar-nav-enhanced.collapsed` |
| Mobile hidden | — | (translate off-screen) |
| Mobile visible | — | `.sidebar-nav-enhanced.show` |

#### 5.1.3 Elements

```html
<nav class="sidebar-nav-enhanced">
    <!-- Header / Logo -->
    <div class="sidebar-header-enhanced">
        <a href="#" class="sidebar-logo">
            <i class="sidebar-logo-icon fas fa-building"></i>
            <span>الدولية</span>
        </a>
    </div>

    <!-- Menu Sections -->
    <div class="sidebar-nav-menu">
        <div class="nav-section-enhanced">
            <div class="nav-section-title-enhanced">الرئيسية</div>

            <div class="nav-item-enhanced">
                <a href="#" class="nav-link-enhanced active">
                    <i class="nav-icon-enhanced fas fa-home"></i>
                    <span>لوحة التحكم</span>
                </a>
            </div>

            <div class="nav-item-enhanced">
                <a href="#" class="nav-link-enhanced">
                    <i class="nav-icon-enhanced fas fa-users"></i>
                    <span>الموظفين</span>
                    <span class="nav-badge-enhanced">12</span>
                </a>
            </div>
        </div>
    </div>
</nav>
```

#### 5.1.4 Active State

```css
.nav-link-enhanced.active {
    background: linear-gradient(135deg, var(--primary-600), var(--primary-700));
    color: white;
    box-shadow: var(--shadow-lg);
}
.nav-link-enhanced.active::before {
    content: '';
    position: absolute;
    right: -12px;
    top: 50%;
    transform: translateY(-50%);
    width: 4px;
    height: 20px;
    background: white;
    border-radius: var(--radius-full);
}
```

### 5.2 Top Navbar

White/brand color scheme with:

```html
<nav class="navbar navbar-expand-lg bg-white shadow-sm">
    <div class="container-fluid">
        <!-- Toggle button for mobile -->
        <button class="navbar-toggler" type="button">
            <i class="fas fa-bars"></i>
        </button>

        <!-- Search -->
        <form class="d-flex" role="search">
            <div class="input-group-enhanced">
                <input type="search" class="form-control-enhanced" placeholder="بحث...">
                <span class="input-group-text"><i class="fas fa-search"></i></span>
            </div>
        </form>

        <!-- Right section -->
        <div class="d-flex align-items-center gap-3">
            <!-- Notifications -->
            <button class="btn btn-ghost btn-icon position-relative">
                <i class="fas fa-bell"></i>
                <span class="nav-badge-enhanced position-absolute">3</span>
            </button>
            <!-- User menu -->
            <div class="dropdown">
                <button class="btn btn-ghost d-flex align-items-center gap-2">
                    <div class="avatar avatar-sm">أ</div>
                    <span>أحمد محمد</span>
                </button>
            </div>
        </div>
    </div>
</nav>
```

### 5.3 Breadcrumbs (`.breadcrumb-enhanced`)

```html
<nav aria-label="Breadcrumb">
    <ol class="breadcrumb-enhanced">
        <li class="breadcrumb-item-enhanced">
            <a href="#"><i class="fas fa-home"></i> الرئيسية</a>
        </li>
        <li class="breadcrumb-separator">
            <i class="fas fa-chevron-left"></i>
        </li>
        <li class="breadcrumb-item-enhanced">
            <a href="#">الموظفين</a>
        </li>
        <li class="breadcrumb-separator">
            <i class="fas fa-chevron-left"></i>
        </li>
        <li class="breadcrumb-item-enhanced active">
            تعديل الموظف
        </li>
    </ol>
</nav>
```

---

## 6. Responsive Breakpoints

| Name | Min Width | CSS Media Query | Variable |
|------|-----------|-----------------|----------|
| **XS** | `<576px` | `@media (max-width: 575.98px)` | — |
| **SM** | `≥576px` | `@media (min-width: 576px)` | `--breakpoint-sm: 576px` |
| **MD** | `≥768px` | `@media (min-width: 768px)` | `--breakpoint-md: 768px` |
| **LG** | `≥992px` | `@media (min-width: 992px)` | `--breakpoint-lg: 992px` |
| **XL** | `≥1200px` | `@media (min-width: 1200px)` | `--breakpoint-xl: 1200px` |
| **2XL** | `≥1400px` | `@media (min-width: 1400px)` | `--breakpoint-2xl: 1400px` |

### Responsive Grid Classes

```css
/* Extra small (default) */
.grid-cols-1 ... grid-cols-12

/* Small (≥576px) */
.grid-cols-sm-1 .grid-cols-sm-2

/* Medium (≥768px) */
.grid-cols-md-1 .grid-cols-md-2 .grid-cols-md-3 .grid-cols-md-4

/* Large (≥992px) */
.grid-cols-lg-1 .grid-cols-lg-2 .grid-cols-lg-3 .grid-cols-lg-4 .grid-cols-lg-6

/* Extra large (≥1200px) */
.grid-cols-xl-1 .grid-cols-xl-2 .grid-cols-xl-3 .grid-cols-xl-4 .grid-cols-xl-6
```

### Responsive Display Classes

| Class | Description |
|-------|-------------|
| `.hidden-sm`, `.block-sm` | Visible/invisible at ≥576px |
| `.hidden-md`, `.block-md` | Visible/invisible at ≥768px |
| `.hidden-lg`, `.block-lg` | Visible/invisible at ≥992px |
| `.hidden-xl`, `.block-xl` | Visible/invisible at ≥1200px |
| `.text-sm-center` | Center text at ≥576px |

### Component-Specific Responsive Behavior

| Component | <768px Behavior |
|-----------|-----------------|
| Sidebar | Transform off-screen, `.show` to toggle |
| Stats card | Padding reduced to 1rem, value to `--font-size-2xl` |
| Table | Font reduced to `--font-size-xs`, padding halved |
| Modal | Margin reduced to 1rem, max-width removed |
| FAB | Reduced to 48px, bottom/left 1rem |
| Tabs | Stack vertically, border-radius changes |
| Toast | Min-width 280px, max-width calc(100vw - 2rem) |
| Pagination | Centered with wrap |

---

## 7. Shadows

| Token | Value | Class |
|-------|-------|-------|
| `--shadow-xs` | `0 1px 2px 0 rgb(0 0 0 / 0.05)` | `.shadow-xs` |
| `--shadow-sm` | `0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1)` | `.shadow-sm` |
| `--shadow-base` | `0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1)` | `.shadow` |
| `--shadow-md` | `0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1)` | `.shadow-md` |
| `--shadow-lg` | `0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1)` | `.shadow-lg` |
| `--shadow-xl` | `0 25px 50px -12px rgb(0 0 0 / 0.25)` | `.shadow-xl` |
| `--shadow-2xl` | `0 25px 50px -12px rgb(0 0 0 / 0.25)` | `.shadow-2xl` |
| `--shadow-inner` | `inset 0 2px 4px 0 rgb(0 0 0 / 0.05)` | `.shadow-inner` |

### Shadow Usage Guidelines

| Component | Shadow |
|-----------|--------|
| Cards (resting) | `--shadow-sm` |
| Cards (hover) | `--shadow-md` → `--shadow-lg` |
| Dropdowns | `--shadow-lg` |
| Modals | `--shadow-2xl` |
| Sidebar | `--shadow-xl` |
| FAB | `--shadow-lg` (hover: `--shadow-xl`) |
| Buttons | None (flat design) |

---

## 8. Border Radius

| Token | Value | Class | Usage |
|-------|-------|-------|-------|
| `--radius-none` | `0` | `.rounded-none` | No rounding |
| `--radius-sm` | `0.125rem` (2px) | `.rounded-sm` | Minimal |
| `--radius-base` | `0.25rem` (4px) | `.rounded` | Buttons |
| `--radius-md` | `0.375rem` (6px) | `.rounded-md` | Form controls |
| `--radius-lg` | `0.5rem` (8px) | `.rounded-lg` | Cards, modals |
| `--radius-xl` | `0.75rem` (12px) | `.rounded-xl` | Enhanced cards |
| `--radius-2xl` | `1rem` (16px) | `.rounded-2xl` | Special |
| `--radius-3xl` | `1.5rem` (24px) | `.rounded-3xl` | Hero sections |
| `--radius-full` | `9999px` | `.rounded-full` | Pills, avatars |

### Border Radius by Component

| Component | Radius Token |
|-----------|-------------|
| `.btn` | `--radius-md` |
| `.btn-icon` | `--radius-lg` |
| `.card` | `--radius-lg` |
| `.card-enhanced` | `--radius-xl` |
| `.form-control` | `--radius-md` |
| `.form-control-enhanced` | `--radius-lg` |
| `.modal-content` | `--radius-xl` |
| `.alert` | `--radius-md` |
| `.alert-enhanced` | `--radius-lg` |
| `.badge` | `--radius-full` |
| `.progress` | `--radius-full` |
| `.input-group-*` | `--radius-lg` |
| `.table-enhanced` | `--radius-lg` (overflow hidden) |

---

## Transitions & Motion

| Token | Value | Class/Usage |
|-------|-------|-------------|
| `--transition-fast` | `150ms ease-in-out` | Color, opacity toggles |
| `--transition-normal` | `300ms ease-in-out` | Default for all transitions |
| `--transition-slow` | `500ms ease-in-out` | Panel slide, overlay fade |
| `--transition-all` | `all var(--transition-normal)` | Convenience shorthand |
| `--transition-colors` | `color, background-color, border-color 150ms` | Hover/focus effects |
| `--transition-opacity` | `opacity 150ms` | Fade effects |
| `--transition-shadow` | `box-shadow 150ms` | Card hover shadow |
| `--transition-transform` | `transform 150ms` | Scale/translate |

### Accessibility — Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}
```

---

## Z-Index Scale

| Token | Value | Component |
|-------|-------|-----------|
| `--z-dropdown` | `1000` | Dropdown menus |
| `--z-sticky` | `1020` | Sticky elements |
| `--z-fixed` | `1030` | Sidebar, navbar |
| `--z-modal-backdrop` | `1040` | Modal backdrop |
| `--z-modal` | `1050` | Modal dialogs |
| `--z-popover` | `1060` | Popovers |
| `--z-tooltip` | `1070` | Tooltips |
| `--z-toast` | `1080` | Toast notifications |

---

## Print Styles

```css
@media print {
    .no-print { display: none !important; }
    .card {
        box-shadow: none;
        border: 1px solid var(--gray-300);
    }
    .btn {
        border: 1px solid var(--gray-400);
    }
}
```

---

## Accessibility Guidelines

- **Focus indicators:** All interactive elements use `:focus-visible` with a 2px `--primary-600` outline offset by 2px
- **Contrast:** High contrast mode media query increases border widths to 2px
- **ARIA:** Buttons should include `aria-label` when icon-only; icons should use `aria-hidden="true"`
- **Keyboard navigation:** All interactive elements are keyboard-accessible
- **Text spacing:** Supports user-defined text spacing without breaking layouts

```html
<!-- Accessible icon button -->
<button class="btn-icon btn-primary" aria-label="إضافة مستخدم جديد">
    <i class="fas fa-user-plus" aria-hidden="true"></i>
</button>
```

---

## Quick Reference: CSS File Structure

```
frontend/static/css/
├── design-system.css      # Design tokens, base styles, layout, core components
│                          # (buttons, cards, forms, tables, badges, alerts,
│                          #  progress, spinners, utilities, responsive, print)
├── components.css         # Enhanced components
│                          # (sidebar, breadcrumbs, enhanced cards, stats cards,
│                          #  enhanced forms, button groups, icon buttons, FAB,
│                          #  enhanced tables, modals, alerts, skeleton loaders,
│                          #  toasts, dropdowns, pagination, search, tabs,
│                          #  accordion, list group, dark theme overrides)
└── theme-system.css       # Theme switching system
```

---

*ElDawliya Design System v3.0 — Last updated June 2026*
