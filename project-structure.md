# Project Structure
This document explains how the project is organized, what lives where, and why the separation makes sense.

## Table of Contents



## 📁 Structure Overview 
Here’s a simplified view of the file tree to give you a feel for how everything is organized. 
```text
📁 app/
📁 pages/ (optional)
📁 features/
├── 📁 actions/
│   └── 📁 create-order/
│       ├── 📁 utils/
│       │   └── 📁 use-create-order/
│       │       ├── 📄 use-create-order.ts
│       │       └── 📄 index.ts
│       ├── 📁 model/
│       │   └── 📄 types.ts
│       ├── 📄 create-order.tsx
│       └── 📄 index.ts
├── 📁 widgets/
│   ├── 📁 order-form/
│   │   ├── 📄 order-form.tsx
│   │   └── 📄 index.ts
│   ├── 📁 dashboard-filters/
│   └── 📁 navbar/
📁 fetchers/
└── 📁 user-select/
    ├── 📁 model/
    │   └── 📄 types.ts
    ├── 📄 user-select.tsx
    └── 📄 index.ts
📁 ui/
├── 📁 components/
│   └── 📁 select/
│       ├── 📁 model/
│       │   ├── 📄 styles.ts
│       │   └── 📄 types.ts
│       ├── 📄 select.tsx
│       └── 📄 index.ts
└── 📁 elements/
    └── 📁 field-label/
        ├── 📁 model/
        │   ├── 📄 styles.ts
        │   └── 📄 types.ts
        ├── 📄 field-label.tsx
        └── 📄 index.ts
📁 shared/
├── 📁 hooks/
│   └── 📁 use-user-list/
│       ├── 📄 use-user-list.ts
│       └── 📄 index.ts
├── 📁 functions/
└── 📁 types/
```
## 📌 Folder Responsibilities

### src/

| Folder | Purpose | Example Files |
| --- | --- | --- |
| **`app/`** | Application entry point. Also contains crucial app-wide elements like providers, themes, and global styles | `dashboard/page.tsx`, `layout.tsx`, `theme-provider.tsx` |
| **`pages/`**  | Optional. Used only with React Router or Next.js Pages Router. In App Router projects, it can still hold page-specific utilities or components | `index.tsx`, `about.tsx`, `dashboard.tsx` |
| **`features/`** | Logic-heavy components that handle API interactions and business workflows. Composed of UI components. | `actions/create-order.tsx`, `fetchers/user-select.tsx`, `widgets/dashboard-filters.tsx` |
| **`ui/`** | UI Components divided into two levels. Contains small UI elements and more complex components. | `elements/text.tsx`, `components/input.tsx` |
| **`shared/`** | Custom hooks, utility functions, and API-related logic. | `hooks/use-fetch.ts`, `functions/format-date.ts` |

### src/ui/

| Folder | Description | Example Files |
| --- | --- | --- |
| **`elements/`** | Small, non-interactive, presentational UI parts. | `field-label.tsx`, `text.tsx`, `table-row.tsx` |
| **`components/`** | Reusable UI components that don’t manage business logic. | `button.tsx`, `input.tsx`, `select.tsx`, `table.tsx`, `modal.tsx` |

### src/features

| Folder | Description | Example Files |
| --- | --- | --- |
| **`fetchers/`** | The most basic of features—UI components that handle data fetching, either from an API or a global state. Built from `ui/` blocks. | `user-select.tsx`, `order-table.tsx` |
| **`widgets/`** | Larger UI structures that may manage state, interactions, and compose multiple components and/or fetchers. | `order-form.tsx`, `navbar.tsx`, `dashboard-filters.tsx` |
| **`actions/`** | The largest UI structures that allow users to perform an action (state updates, server requests) | `edit-post.tsx` , `add-comment.tsx`, `create-order.tsx` |

### src/pages

In component-focused setups, `pages/` is usually minimal or empty, unless you're using routing libraries that rely on file-based routing. You can still use it for page-specific wrappers or layouts when needed.

## 📐 Hierarchy Rules

This architecture follows a top-down organizational pattern, where each layer can only import from the layers below it—never from the same layer or above. 

![cfa-hierarchy](https://github.com/user-attachments/assets/c1846cab-5de3-45ed-ad27-d2fcf56dc2ba)

No sideways or upward imports—if you ever feel like you need to break this rule, it’s probably a sign to refactor.

## 🧩 Example: From Elements to Actions

Here’s a real-world example of how a simple user creation flow can span across the layers. 

![example](https://github.com/user-attachments/assets/971042e0-f68d-4389-963f-4737bea78cae)

## 🗂 Localized Folder Pattern

Each component or utility lives in its own folder. This keeps logic, styles, and types scoped and maintainable.

For example:

- `select.tsx` or `select.ts` - the main component or function
- `index.ts` - default export for clean imports
- `model/` - internal logic:
    - `types.ts` - TypeScript types
    - `styles.ts`, `styles.css`, or `styles.scss` - scoped styles
    - optional: `context.ts`, `schema.ts`, etc.

This setup supports scalability, encourages clean code separation, and works well across both UI and utility layers.

## 📄 File naming

All files and folders should follow the **kebab-case** naming convention for consistency and readability. This helps keep everything predictable, especially in larger projects. 

| **Type** | **File Name Example** |
| --- | --- |
| **Styles (styled components, CSS-modules, SCSS, etc)** | `styles.ts` / `styles.scss` / `styles.css` |
| **TypeScript types and interfaces** | `types.ts` |
| **Validation schema (Zod, Yup, etc)** | `schema.ts` |
| **Utility functions** | `format-date.ts`, `parse-query.ts` |
| **Custom React hooks** | `use-fetch.ts`, `use-user-list.ts` |

### **Component & Folder Naming**

- **Component files** should be named after the component: `button.tsx`, `modal.tsx`.
- **Each component should have an `index.ts`** for easier imports:
    ✅ `import { Button } from "@/ui/components/button"` (instead of `button/button.tsx`)

### Importing Components

This structure does not use barrel files (index.ts that re-export multiple modules). Instead, index.ts is only used inside component folders for cleaner imports, e.g.:
✅ `import { Button } from "@/ui/components/button"`
❌ `import { Button } from "@/ui/components"` (no barrel file grouping components together)
