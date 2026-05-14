# SvelteKit

Use the latest version of Svelte and SvelteKit

## Initialisation
New SvelteKit projects must be initialised with the following command:
```shell
npx sv create --template minimal --types jsdoc --add eslint prettier paraglide="languageTags:en,en-nz+demo:no" mcp="ide:vscode,other+setup:remote" --install yarn <folder_for_project>
```

Unless it is a library, in which case the following command must be used:
```shell
npx sv create --template library --types jsdoc --add eslint prettier paraglide="languageTags:en,en-nz+demo:no" mcp="ide:vscode,other+setup:remote" --install yarn <folder_for_project>
```
