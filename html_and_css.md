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
a,
button.link {
  text-decoration: none;
  color: var(--interactive-color);

  &:hover {
    text-decoration: underline;
  }
}

button {
  --color-1: var(--interactive-color);
  --color-2: var(--background-color);

  border: solid 2px var(--color-1);
  background: var(--color-1);
  color: var(--color-2);

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

  &.primary {
    --color-1: var(--primary-color);
  }

  &.warn {
    --color-1: var(--warn-color);
  }

  &.link {
    padding: 0;
    margin: 0;
    background: none:
  }
}
````

