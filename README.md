# Ibexa Flex Recipes

Symfony Flex recipes for the comwrap Ibexa bundles.

This repository is the source of truth. Recipes are edited on `main`; the
"Update Flex endpoint" GitHub Action then regenerates the endpoint on the
`flex/main` branch, which is what Composer reads.

## Available recipes

| Package | Repository |
| --- | --- |
| `comwrap/ibexa-code-style` | [ibexa-code-style-bundle](https://github.com/comwrap/ibexa-code-style-bundle) |
| `comwrap/ibexa-core-components` | [ibexa-core-components](https://github.com/comwrap/ibexa-core-components) |
| `comwrap/ibexa-dictionary` | [ibexa-dictionary](https://github.com/comwrap/ibexa-dictionary) |

## Usage in a project

Register the endpoint in the project's `composer.json`:

```json
"extra": {
    "symfony": {
        "allow-contrib": true,
        "endpoint": {
            "flex-comwrap": "https://api.github.com/repos/comwrap/Ibexa-Flex-Recipes/contents/index.json?ref=flex/main",
            "flex-ibexa": "https://api.github.com/repos/ibexa/recipes/contents/index.json?ref=flex/main",
            "flex-symfony": "flex://defaults"
        }
    }
}
```

Recipes copying into `%MIGRATION_DIR%` or `%TEMPLATES_DIR%` rely on custom
`extra` keys. Projects using them must define:

```json
"extra": {
    "migration-dir": "src/Migrations/Ibexa/migrations/",
    "templates-dir": "templates/themes/"
}
```

Without these keys Flex leaves the placeholder unresolved and creates a
directory literally named `%MIGRATION_DIR%`.

## Adding or changing a recipe

1. Add `comwrap/<package>/<version>/manifest.json`, where `<package>` is the
   Composer package name (not the repository name) and `<version>` is the
   lowest package version the recipe applies to. Flex picks the highest recipe
   version that is less than or equal to the installed package version.
2. Push to `main`. The workflow publishes `index.json` and the per-recipe JSON
   files to `flex/main`, and keeps previous revisions under `archived/`.
