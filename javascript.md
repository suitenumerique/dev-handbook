# La Suite numérique with JavaScript

In this section, we will describe our best practices when dealing with JavaScript code.

## Runtime

JavaScript projects should use the active [Node.js LTS](https://nodejs.org/en/about/previous-releases) version. This keeps our tooling predictable and avoids depending on features that are not yet broadly supported.

When possible, declare the expected Node.js version in the project configuration, _e.g._ in `package.json`:

```json
{
  "engines": {
    "node": "<current LTS version>"
  }
}
```

## Linting and formatting

JavaScript projects should enforce code quality and formatting with the following tools:

* [ESLint](https://eslint.org/) for JavaScript and TypeScript code,
* [Prettier](https://prettier.io/) for formatting,
* [stylelint](https://stylelint.io/) for stylesheets when the project contains styles.

Some projects may use [Oxc](https://oxc.rs/) for part of this toolchain. This is acceptable when it fits the project, but each project should still provide clear linting and formatting commands so contributors know how to validate their changes.

Typical scripts in `package.json` should look like:

```json
{
  "scripts": {
    "lint": "eslint .",
    "format": "prettier --write .",
    "test": "vitest run"
  }
}
```

## Tests

We use [Vitest](https://vitest.dev/) for JavaScript unit tests. Test files should be named after the file, component, hook, or utility they cover:

```text
UserMenu.test.tsx
useCurrentUser.test.ts
formatDate.test.ts
```

Tests can live next to the tested file or in a `__tests__` directory when this makes the feature easier to read. Keep the convention consistent inside the same project.

## Frontend

* [Frontend projects](frontend.md)
