# CSS Rules

- CSS variables must be used for colors
- CSS must be structured using nested CSS
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

