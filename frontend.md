# Frontend projects

In this section, we will describe our best practices when dealing with frontend code.

## Framework

We use both [Next.js](https://nextjs.org/) and [Vite](https://vite.dev/) in our projects.

Next.js remains a valid choice for existing projects or when its specific features are required. However, for new frontend projects, we recommend starting with Vite unless there is a clear reason not to. Vite gives us a simpler frontend stack, a smaller runtime surface, faster development server feedback, and avoids depending on Next.js-specific server features when we only need a client application.

## Project layout

We favor a simple feature-oriented structure. A typical frontend project can follow this layout:

```text
src/
├── components/
├── features/
├── hooks/
├── services/
├── stores/
├── types/
└── utils/
```

Use `features/` for business domains and user-facing flows. Use `components/`, `hooks/`, `types/`, and `utils/` for shared code that is not owned by a single feature.

## Naming

We use the following naming conventions:

* React components are written in `PascalCase`, _e.g._ `UserMenu.tsx`;
* hooks start with `use`, _e.g._ `useCurrentUser.ts`;
* folders use `kebab-case`, _e.g._ `user-menu/`.

💩 Bad

```text
src/components/user_menu.tsx
src/hooks/currentUser.ts
src/components/UserMenu.tsx
```

❤️ Good

```text
src/components/user-menu/UserMenu.tsx
src/hooks/useCurrentUser.ts
```

## Barrel exports

When a folder exposes several public modules, use a small `index.ts` file as a barrel export. This keeps imports stable and avoids leaking internal file paths.

```ts
export * from './UserMenu';
export * from './UserMenuButton';
```

Do not use barrel exports to hide too many unrelated modules behind a single import path. If the file becomes hard to read, split the folder or import directly.

## State and data fetching

We usually separate server state from local UI state:

* use [TanStack Query](https://tanstack.com/query) for data fetched from APIs;
* use local React state when the state belongs to one component or one small component tree;
* consider a small store such as [Zustand](https://zustand-demo.pmnd.rs/) only when React state is not enough.

Before adding a store, check whether regular React state, component composition, or TanStack Query already solve the problem. A store should make state easier to understand, not just easier to access from everywhere.
