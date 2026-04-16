---
name: sveltekit
description: SvelteKit and Svelte coding rules and conventions. Use when initialising, writing, or reviewing any SvelteKit or Svelte code, including components, routes, and libraries.
---

# SvelteKit

Use the latest version of Svelte and SvelteKit.

## Initialisation

New SvelteKit projects must be initialised with the following command:

```shell
npx sv create --template minimal --types jsdoc --add eslint prettier paraglide="languageTags:en,en-nz+demo:no" mcp="ide:vscode,other+setup:remote" --install yarn <folder_for_project>
```

Unless it is a library, in which case:

```shell
npx sv create --template library --types jsdoc --add eslint prettier paraglide="languageTags:en,en-nz+demo:no" mcp="ide:vscode,other+setup:remote" --install yarn <folder_for_project>
```
