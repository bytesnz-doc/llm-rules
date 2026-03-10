# HTML and CSS Rules

## General Rules
- Avoid creating CSS classes, using element selectors instead

## HTML-specific Rules
- Use semantic HTML as it should be used
- Avoid using `div`s and `span`s unless absolutely necessary
- Use in-built components where they exist, e.g. `<input type="date">`
- Do not use aria tags unless needed
- Use links for page changes

## CSS-specific Rules
- CSS variables must be used for
  - colors
  - common standard units, e.g. `--box-padding: 0.5em 0.8em;`
- CSS colors should have light and dark themes using `light-dark()`
- Classes should be used common flexbox styling, e.g. `.flex`, `.gap`, `.wrap`
- CSS must be structured using nested CSS
- Put common CSS in a common file like src/style.css
- Use native HTML elements over custom styled divs - Use `<details>`,
  `<summary>`, `<fieldset>`, etc. instead of creating custom styled containers
- Check existing patterns in the codebase first - Look at how similar UI is
  already styled before adding new CSS
- Reuse existing CSS classes from style.css - Don't recreate styles that
  already exist

## Examples

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
    color: var(--color-text);
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
      color: var(--color-text);
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

## CSS Colors With Light and Dark Themes

```css
:root {
  theme: light dark;
  --background: light-dark(#fff, #000);
}
```

## Structured CSS
```css
:root {
  color-scheme: light dark;
  --color-background: light-dark(#f5f5f5, #202020);
  --color-background-light: light-dark(#c5c5c5, #a0a0a0);
  --color-text: light-dark(#202020, #f5f5f5);
  --color-primary-color: light-dark(#ff4924, #b81f00);
  --color-primary-light: light-dark(#ff4924, #b81f00);
  --color-primary-text: light-dark(#000, #fff);
  --color-error-color: light-dark(#DB2121, #7D1515);
  --color-error-color: light-dark(#DB2121, #7D1515);
  --color-error-text: #fff;
  --color-success-color: light-dark(#4AC720, #173D0A);
  --color-success-light: light-dark(#4AC720, #173D0A);
  --color-success-text: light-dark(#000, #fff);
  --color-warning-color: light-dark(#D1A021, #654D10);
  --color-warning-light: light-dark(#D1A021, #654D10);
  --color-warning-text: light-dark(#000, #fff);
  --border-radius: 0;
  --box-padding: 0,5rem 1rem;

  --color-1: var(--color-text);
  --color-2: var(--color-background);
}

.primary {
  --color-1: var(--color-primary-text);
  --color-2: var(--color-primary-color);
  --color-3: var(--color-primary-light);
}


.error {
  --color-1: var(--color-error-text);
  --color-2: var(--color-error-color);
  --color-3: var(--color-error-light);
}

.warning {
  --color-1: var(--color-warning-text);
  --color-2: var(--color-warning-color);
  --color-3: var(--color-warning-light);
}

.success {
  --color-1: var(--color-success-text);
  --color-2: var(--color-success-color);
  --color-3: var(--color-success-light);
}

a,
button.link {
  text-decoration: none;
  color: var(--color-primary-color);

  &:hover {
    text-decoration: underline;
  }
}

a.button,
button {
  border: solid 2px var(--color-1);
  background: var(--color-1);
  color: var(--color-2);
  border-radius: var(--border-radius);

  &:hover {
    background: var(--color-2);
    color: var(--color-1);
  }

  &.outline {
    background: var(--color-2);
    color: var(--color-1);

    &:hover {
      background: var(--color-1);
      color: var(--color-2);
    }
  }

  &.link {
    padding: 0;
    margin: 0;
    background: none:
  }
}

.notice {
  border: solid 1px var(--color-1);
  padding: var(--box-padding);
}

.nav-list {
  margin: 0 auto;
  list-style: none;

  li a {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 1.5rem;
    background: var(--color-2);
    border: 1px solid var(--color-1);
    border-radius: 8px;
    text-decoration: none;
    color: var(--color-text);
    transition: background 0.2s;
    font-size: 1.25rem;
    font-weight: 500;

    &:hover {
      background: var(--color-3);
      text-decoration: none;
    }

    .icon {
      font-size: 2rem;
    }
  }
}

````

