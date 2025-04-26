# Customization and FAQ

This document covers how you can customize the project structure to fit your team, and answers common questions about best practices.

## Table of Contents

- [Customization & Team Preferences](#-customization--team-preferences)
- [FAQ](#-faq)
  - [What’s the difference between `widgets/` and `actions/`?](#whats-the-difference-between-widgets-and-actions)
  - [Why are there no barrel files?](#why-are-there-no-barrel-files)
  - [What do you mean by “Each component should have an `index.ts`,” but no barrel files?](#what-do-you-mean-by-each-component-should-have-an-indexts-but-no-barrel-files)
  - [Why use a `model/` folder instead of placing `types.ts` and `styles.ts` directly?](#why-use-a-model-folder-instead-of-placing-typests-and-stylests-directly)
  - [Where do API calls go?](#where-do-api-calls-go)
  - [When would I use the `pages/` folder instead of `app/`?](#when-would-i-use-the-pages-folder-instead-of-app)
  - [What’s the difference between `features/` and `ui/`?](#whats-the-difference-between-features-and-ui)
  - [Can I group `features/` by domain instead of type?](#can-i-group-features-by-domain-instead-of-type)
  - [Where should I put images?](#where-should-i-put-images)
  - [Where should I store SVG icons (as React components)?](#where-should-i-store-svg-icons-as-react-components)
  - [Do I need all the layers?](#do-i-need-all-the-layers)

## ✏️ Customization & Team Preferences

This structure is designed to be flexible, so feel free to adapt it to what works best for your project.

- If you prefer a different **file naming convention**, go for it.
- **Barrel files (`index.ts`)** are intentionally avoided, but if your team finds them useful, you can include them.
- You can organize your features and utilities by domain if that fits your workflow better.

## ⁉ FAQ

### What’s the difference between `widgets/` and `actions/`?
Both deal with larger UI structures, but `actions/` typically involve **user-triggered behavior** or workflows that affect the app’s state or perform an async task (like submitting a form). `widgets/` are more about UI composition and layout—they focus on **displaying data** or combining smaller components without necessarily handling the logic themselves.
    
### Why are there no barrel files?
To avoid problems like circular dependencies, slow build times, and unclear imports. Each `index.ts` is local to a folder and only used to simplify imports from that specific component. No re-exporting everything in bulk. [More on that here](https://tkdodo.eu/blog/please-stop-using-barrel-files).
    
### What do you mean by “Each component should have an `index.ts`,” but no barrel files?
It means that inside a **single component folder**, an `index.ts` is used for **default export only**—just to simplify imports like this:
    
```tsx
import { Button } from "@/ui/components/button";
```

However, we **don’t create barrel files** that re-export multiple components from one place like `components/index.ts`. That approach is avoided to prevent circular dependencies and keep imports clean and explicit.
    
### Why use a `model/` folder instead of placing `types.ts` and `styles.ts` directly?
The `model/` folder helps group everything related to the internal “model” of a component—like its types, styles, validation schemas, and context. It keeps the main file focused while making the structure more scalable and organized.
    
### Where do API calls go?
API calls and related logic should be placed in the `shared` layer to keep them reusable across different parts of the application. This ensures a single source of truth for data-fetching logic.
    
### When would I use the `pages/` folder instead of `app/`?  
Use `pages/` if you’re working with **Next.js’s Pages Router** or **React projects** using React Router.

Even in **Next.js projects with the App Router**, the `pages/` folder can still be helpful if you need to apply **page-specific styles, layouts, or configurations**. Some projects also use both during migration or for legacy reasons.
    
### What’s the difference between `features/` and `ui/`?
- `features/` contains logic-heavy components that manage workflows or interact with APIs.
- `ui/` is for reusable presentation components.

Think of `features/` as *what the app does*, and `ui/` as *how it looks*.
    
### Can I group `features/` by domain instead of type?  
Yep! If your project is growing, you can group `features/` by domain instead of keeping all `widgets/`, `actions/`, and `fetchers/` flat. For example:

```
features/
├── user/
│   ├── widgets/
│   ├── actions/
│   └── fetchers/
└── order/
    ├── widgets/
    └── actions/
```

This helps keep related components close together. Use this structure when it makes sense for your app’s complexity.
    
### Where should I put images?    
**If an image is used by a certain component**, place it inside that component’s folder, for example:

`/components/avatar/media` or `/widgets/profile-card/images`.

**If the image doesn’t belong to a specific layer**, store it in a neutral location like:

`/shared/media` or `/shared/images`.

Whether you name it `images` or `media` or something else is entirely up to you—feel free to use whatever makes the most sense in your case.
    
### Where should I store SVG icons (as React components)?
If the icon is a standalone components that returns SVG code, it belongs in the elements layer. You can store it directly in `ui/elements` or group them together in `ui/elements/icons`. 
    
### Do I need all the layers?
Not all folder layers (like `fetchers/` or `widgets/`) are required—use only what makes sense for your project.
