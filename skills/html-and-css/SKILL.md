---
name: html-and-css
description: HTML and CSS coding rules and conventions. Use when writing or reviewing any HTML markup or CSS styling, including Svelte component templates and style blocks.
---
# HTML and CSS Rules

## CSS General Rules

- CSS variables must be used for
  - colors
  - common standard units, e.g. `--box-padding: 0.5em 0.8em;`
- CSS colors should have light and dark themes using `light-dark()`
- Classes should be used for common flexbox styling, e.g. `.flex`, `.gap`, `.wrap`
- CSS must be structured using nested CSS
- Avoid creating CSS classes, using element selectors instead
- **NEVER duplicate CSS** - anything that could be shared belongs in `src/style.css`
- **ALWAYS use nested CSS** in component `<style>` blocks - no flat CSS allowed
- Every project MUST have a global stylesheet that defines colors, varient
  classes, styling for common objects including buttons, links, form
  elements and notice divs
- Color varients must use the global color varient classes and MUST not be
  included in component styling
- Use native HTML elements over custom styled divs — use `<details>`,
  `<summary>`, `<fieldset>`, etc. instead of creating custom styled containers
- Always check existing patterns in the codebase first — look at how similar UI
  is already styled before adding new CSS
- Always reuse existing CSS classes from the main stylesheet — don't recreate
  styles that already exist
- Don't duplicate selector names in nested CSS — since CSS is nested under the
  component class, use short names like `.title`, `.count` instead of
  `.component-title`, `.component-count`

### Required Status Color Structure
Each status color/color varient MUST define three variables:
- `--color-x-color`: The main color (used for borders, text)
- `--color-x-contrast`: Text color with sufficient contrast for use on --color-x-color
- `--color-x-light`: Light transparent variant (hsla with ~0.1 alpha) for backgrounds
The specific hue/saturation/lightness values can be customized to fit the app's design.

### Global Stylesheet (Required)

- Must include CSS colors and common properties such as spacing, padding and
  borders as CSS variables
- Must have a global set of color variables that are overridden by the color
  varient classes that can be used on any element of component

```css
  :root {
    color-scheme: light dark;

    /* Spacing */
    --box-padding: 0.5rem 1rem;
    --box-spacing: 1rem;

    /* Border */
    --border-radius: 4px;
    --border: 1px solid var(--color-border);

    /* Colors */
    --color-background: light-dark(hsl(0, 0%, 96%), hsl(0, 0%, 13%));
    --color-background-hover: light-dark(hsl(0, 0%, 100%), hsl(0, 0%, 0%));
    --color-card: light-dark(hsl(0, 0%, 100%), hsl(0, 0%, 18%));
    --color-contrast: light-dark(hsl(0, 0%, 20%), hsl(0, 0%, 88%));
    --color-contrast-muted: light-dark(hsl(0, 0%, 40%), hsl(0, 0%, 67%));
    --color-border: light-dark(hsl(0, 0%, 86%), hsl(0, 0%, 27%));

    /* Status colors */
    --color-primary-color: light-dark(hsl(204, 70%, 53%), hsl(204, 70%, 41%));
    --color-primary-contrast: light-dark(hsl(0, 0%, 100%), hsl(0, 0%, 100%));
    --color-primary-light: light-dark(
      hsla(204, 70%, 53%, 0.1),
      hsla(204, 70%, 41%, 0.1)
    );

    --color-success-color: light-dark(hsl(145, 37%, 40%), hsl(145, 69%, 24%));
    --color-success-contrast: light-dark(hsl(0, 0%, 100%), hsl(0, 0%, 100%));
    --color-success-light: light-dark(
      hsla(145, 37%, 40%, 0.1),
      hsla(145, 69%, 24%, 0.1)
    );

    --color-warning-color: light-dark(hsl(38, 89%, 50%), hsl(38, 73%, 32%));
    --color-warning-contrast: light-dark(hsl(0, 0%, 100%), hsl(0, 0%, 100%));
    --color-warning-light: light-dark(
      hsla(38, 89%, 50%, 0.1),
      hsla(38, 73%, 32%, 0.1)
    );

    --color-error-color: light-dark(hsl(6, 82%, 59%), hsl(6, 70%, 37%));
    --color-error-contrast: light-dark(hsl(0, 0%, 100%), hsl(0, 0%, 100%));
    --color-error-light: light-dark(
      hsla(6, 82%, 59%, 0.1),
      hsla(6, 70%, 37%, 0.1)
    );

    /* Color variants - defaults */
    --color-default-color: light-dark(hsl(0, 0%, 13%), hsl(0, 0%, 87%));
    --color-default-contrast: light-dark(hsl(0, 0%, 87%), hsl(0, 0%, 13%));
    --color-default-light: light-dark(hsl(0, 0%, 40%, 0.1), hsla(0, 0%, 67%, 0.1));

    --color--light: var(--color-default-light);
    --color--contrast: var(--color-default-contrast);
    --color--color: var(--color-default-color);
  }

  /* Color variant classes (doubled to increase specificity */
  .primary.primary {
    --color--color: var(--color-primary-color);
    --color--contrast: var(--color-primary-contrast);
    --color--light: var(--color-primary-light);
  }

  .error.error {
    --color--color: var(--color-error-color);
    --color--contrast: var(--color-error-contrast);
    --color--light: var(--color-error-light);
  }

  .warning.warning {
    --color--color: var(--color-warning-color);
    --color--contrast: var(--color-warning-contrast);
    --color--light: var(--color-warning-light);
  }

  .success.success {
    --color--color: var(--color-success-color);
    --color--contrast: var(--color-success-contrast);
    --color--light: var(--color-success-light);
  }

  /* Base elements */
  * {
    box-sizing: border-box;
    color: var(--color--color);
  }

  body {
    margin: 0;
    font-family: sans-serif;
    background: var(--color-background);
    color: var(--color--color);
  }

  main > *{
    max-width: 100rch;
    margin: 0 auto;
    padding: var(--box-padding);
  }

  input,
  textarea {
    font-family: inherit;
    width: 100%;
    padding: var(--box-padding);
    border: var(--border);
    border-radius: var(--border-radius);
    font-size: 1rem;
    background: var(--color-card);
    color: var(--color-contrast);
  }

  a,
  button.link {
    text-decoration: none;
    color: var(--color-primary-color);

    &:hover {
      text-decoration: underline;
    }
  }

  .button-list details,
  .button-list li > a,
  a.button,
  button {
    cursor: pointer;
    border: var(--border);
    border-color: var(--color--color);
    border-radius: var(--border-radius);
    background: var(--color--color);
    color: var(--color--contrast);
    padding: var(--box-padding);

    &:hover {
      background: var(--color--light);
      color: var(--color--color);
      text-decoration: none;
    }

    &.outline {
      background: var(--color--light);
      color: var(--color--color);

      &:hover {
        background: var(--color--color);
        color: var(--color--contrast);
      }
    }

    &.link {
      padding: 0;
      margin: 0;
      background: none;
    }
  }

  .button-list {
    margin: 0 auto;
    list-style: none;

    li {
      margin-top: var(--box-spacing);
      margin-bottom: var(--box-spacing);

      & > a {
        display: flex;
        align-items: center;
        gap: 1rem;
        background: var(--color--light);
        color: var(--color--color);

        &:hover {
          background: var(--color--color);
          color: var(--color--contrast);
        }
      }
    }

    details {
      background: var(--color--light);
      color: var(--color--color);
      padding: 0;

      & > *:not(summary) {
        --color--light: var(--color-default-light);
        --color--contrast: var(--color-default-contrast);
        --color--color: var(--color-default-color);
        color: var(--color--color);
      }

      & > * {
        padding: var(--box-padding);
      }

      summary {
        padding: var(--box-padding);

        &:hover {
          background: var(--color--color);
          color: var(--color--contrast);
        }
      }
    }
  }



  /* Form labels */
  label {
    display: block;
    margin-bottom: 1rem;

    span {
      display: block;
      font-weight: 500;
      margin-bottom: 0.15rem;
    }

    input,
    textarea {
      width: 100%;
    }
  }

  /* Common classes */
  .center {
    text-align: center;
  }

  .flex {
    display: flex;

    &.middle {
      align-items: center;
    }

    &.wrap {
      flex-wrap: wrap;
    }
  }

  .gap {
    gap: var(--box-spacing);
  }

  .grow {
    flex: 1;
  }

  .notice {
    padding: var(--box-padding);
    border-radius: var(--border-radius);
    margin-top: var(--box-spacing);
    margin-bottom: var(--box-spacing);
    border: var(--border);
    border-color: var(--color-color);
    background: var(--color--light);
    color: var(--color--color);
  }
```

## HTML-Specific Rules

### Semantic HTML

- Use semantic HTML as it should be used
- Use native HTML elements over custom styled divs — use `<details>`,
  `<summary>`, `<fieldset>`, etc. instead of creating custom styled containers
- Avoid using `div`s and `span`s unless absolutely necessary
- Use in-built components where they exist, e.g. `<input type="date">`
- Do not use aria tags unless needed
- Use links for page changes

### Form Labels

Wrap input in label with a span for the label text:

**Good:**

```html
<label>
  <span>Date</span>
  <input type="date" />
</label>
```

**Bad:**

```html
<div class="field">
  <label for="date">Date</label>
  <input id="date" type="date" />
</div>
```

### Button Groups

Use a simple div with buttons:

**Good:**

```html
<div class="buttons">
  <button type="button">Pass</button>
  <button type="button">NA</button>
  <button type="button">Fail</button>
</div>
```

### Avoid Unnecessary Wrappers

Don't wrap elements unnecessarily. Prefer direct children:

**Good:**

```html
<li>
  <a href="...">
    <span>Icon</span>
    Title
  </a>
</li>
```

**Bad:**

```html
<li>
  <a href="...">
    <span class="icon-wrapper">
      <span class="icon">Icon</span>
    </span>
    <span class="title">Title</span>
  </a>
</li>
```
### Overuse of Classes and Elements

**Bad**
```html
<ul class="nav-list">
  <li class="nav-list-item"><a href="/inspections">
    <span class="nav-list-item-icon">📋</span>
    <span class="nav-list-item-title">Inspections</span>
  </a></li>

  <li class="nav-list-item"><a href="events">
    <span class="nav-list-item-icon">🚒</span>
    <span class="nav-list-item-title">Events</span>
  </a></li>
</ul>

<style>
  ul.nav-list {
    max-width: 400px;
    margin: 0 auto;
    list-style: none;
  }

  li.nav-list-item a {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 1.5rem;
    background: var(--color-surface);
    border: 1px solid var(--color-border);
    border-radius: 8px;
    text-decoration: none;
    color: var(--color-contrast);
    transition: background 0.2s;
    font-size: 1.25rem;
  }

  li.nav-list-item a:hover {
    background: light-dark(#f5f5f5, #3a3a3a);
    text-decoration: none;
  }

  .nav-list-item-icon {
    font-size: 2rem;
  }

  .nav-list-item-title {
    font-weight: 500;
  }
</style>
```

**Good**
```html
<ul class="nav-list">
  <li><a href="/inspections">
    <span>📋</span>
    Inspections
  </a></li>

  <li><a href="/events">
    <span>🚒</span>
    Events
  </a></li>
</ul>

<style>
  .nav-list {
    max-width: 400px;
    margin: 0 auto;
    list-style: none;

    li a {
      display: flex;
      align-items: center;
      gap: 1rem;
      padding: 1.5rem;
      background: var(--color-surface);
      border: 1px solid var(--color-border);
      border-radius: 8px;
      text-decoration: none;
      color: var(--color-contrast);
      transition: background 0.2s;
      font-size: 1.25rem;
      font-weight: 500;

      &:hover {
        background: light-dark(#f5f5f5, #3a3a3a);
        text-decoration: none;
      }

      .icon {
        font-size: 2rem;
      }
    }
  }
</style>
```

## CSS-specific Rules

### Nested CSS (MANDATORY)

All component styles MUST use nested CSS. No flat CSS allowed.

**Bad:**

```css
.step {
}
.step h3 {
}
.step-description {
}
.step input {
}
.step-notes textarea {
}
```

**Good:**

```css
.step {
  h3 {
  }
  .description {
  }
  input {
  }
  textarea {
  }
}
```

### Class Naming - Scoped and Nested

Use BEM-like nesting, not verbose class names.

**Bad:**

```css
.completion-date-label {
}
.completion-date-input {
}
.step-header {
}
.step-description-text {
}
```

**Good:**

```css
.completion-date {
  label {
  }
  input {
  }
}

.step {
  h3 {
  }
  .description {
  }
  input {
  }
  textarea {
  }
}
```

### Pattern Checklist

Before writing ANY CSS in a component, check:

- [ ] Does `style.css` already have this pattern?
- [ ] Can I use a semantic HTML element instead of `<div>`?
- [ ] Can I use a label wrapper for inputs instead of a div?
- [ ] Am I duplicating input/textarea/button styles?
- [ ] Am I using nested CSS?
- [ ] Am I using global color variant classes (`.error`, `.warning`, `.success`) instead of defining custom colors?

## Examples

### Button Link List

**Good:**

```html
<ul class="button-list">
  <li>
    <a href="/inspections">
      <span>📋</span>
      Inspections
    </a>
  </li>

  <li>
    <a href="/events">
      <span>🚒</span>
      Events
    </a>
  </li>
</ul>
```

### Form

The form styling is in style.css. Components just use the label pattern:

```html
<form>
  <label>
    <span>Name</span>
    <input type="text" />
  </label>

  <label>
    <span>Date</span>
    <input type="date" />
  </label>
</form>
```

### Button Group with Variants

**Bad:**

```html
<div class="buttons">
  <button type="button" class="pass">Pass</button>
  <button type="button" class="na">NA</button>
  <button type="button" class="fail">Fail</button>
</div>

<style>
  .buttons {
    display: flex;
    gap: 0.5rem;

    button {
      flex: 1;
      padding: 0.5rem;
      border: 2px solid var(--color-border);
      background: var(--color-card);
      cursor: pointer;

      &.pass {
        background: var(--color-success-light);
        border-color: var(--color-success-color);
        color: var(--color-success-color);
      }

      &.na {
        background: var(--color-warning-light);
        border-color: var(--color-warning-color);
        color: var(--color-warning-color);
      }

      &.fail {
        background: var(--color-error-light);
        border-color: var(--color-error-color);
        color: var(--color-error-color);
      }

      &.selected {
        border-width: 3px;
      }
    }
  }
</style>
```

**Good:**

```html
<div class="buttons">
  <button type="button" class="success">Pass</button>
  <button type="button" class="warning">NA</button>
  <button type="button" class="error">Fail</button>
</div>

<style>
  .buttons {
    display: flex;
    gap: 0.5rem;

    button {
      flex: 1;
      cursor: pointer;

      &.selected {
        border-width: 3px;
      }
    }
  }
</style>
```

