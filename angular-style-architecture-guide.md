styles & Layout setup
For more details, checkout this commit:
https://github.com/inoubli/ngrx-signal-store-sample/commit/b85553ba30d21c60dbfe26429176f5b91ed64e2f#diff-23fad3928bf3e71585eb4a6e3b2ff72dae02c3ed5a44d2f0deb1d49f0cb3b1a3


Day 1
* Setup routing + layouts
* Lazy loading working
* Basic header/sidebar shell
Day 2
* SCSS architecture
* Tokens + utilities
* Layout system
Day 3
* Material theme
* Integrate components (toolbar, sidenav, etc.)






Day 2
Steps to do once you have prepared your needed first level  (layout routing) & second level (features routing with lazy loading) 

—————————————————
Target Architecture
—————————————————
src/
 ├── app/
 │   ├── core/
 │   │   ├── guards/
 │   │   ├── services/
 │   │
 │   ├── layouts/
 │   │   ├── auth-layout/
 │   │   ├── main-layout/
 │   │
 │   ├── shared/
 │   │   ├── components/
 │   │   ├── ui/
 │   │
 │   ├── features/
 │   │   ├── auth/
 │   │   │   └── auth.routes.ts
 │   │   ├── todos/
 │   │       ├── todos.routes.ts
 │   │       ├── pages/
 │   │       └── store/
 │   │
 │   └── app.routes.ts
 │
 ├── styles/
 │   ├── abstracts/   (tokens, mixins)
 │   ├── base/        (reset, typography)
 │   ├── utilities/   (spacing, flex, grid)
 │   ├── layout/      (layout helpers)
 │   ├── themes/      (material + custom theme)
 │   └── main.scss


—————————————————
main-layout.html
—————————————————
<div class="layout">
  <header class="layout__header">
    <main-header />
  </header>

  <div class="layout__body">
    <aside class="layout__sidebar">
      <main-sidebar />
    </aside>

    <main class="layout__content">
      <router-outlet />
    </main>
  </div>
</div>


—————————————————
app.routes.ts
—————————————————
export const routes: Routes = [
  {
    path: 'auth',
    loadComponent: () => import('./layouts/auth-layout/auth-layout').then((m) => m.AuthLayout),
    children: [
      {
        path: '',
        loadChildren: () => import('./features/auth/auth.routes').then((m) => m.AUTH_ROUTES),
      },
    ],
  },
  {
    path: '',
    loadComponent: () => import('./layouts/main-layout/main-layout').then((m) => m.MainLayout),
    children: [
      {
        path: 'todos',
        loadChildren: () => import('./features/todos/todos.routes').then((m) => m.TODOS_ROUTES),
      },
      {
        path: '',
        redirectTo: 'todos',
        pathMatch: 'full',
      },
    ],
  },
  {
    path: '**',
    redirectTo: '',
  },
];


—————————————————
Style folders creation
—————————————————
mkdir -p src/styles/{abstracts,base,utilities,layout,themes}   // This will create all needed style files
touch src/styles/main.scss


—————————————————
Angular.json
—————————————————
"styles": [
  "src/styles/main.scss"  // Replaced the default entry "src/styles.scss",
]


—————————————————
Style files creation
—————————————————

 /******************************************/
	src/styles/abstracts/_tokens.scss
 /******************************************/
:root {
  /* ========== SPACING (based on 4px system) ========== */
  --space-0: 0;
  --space-xs: 0.25rem; // 4px
  --space-sm: 0.5rem; // 8px
  --space-md: 1rem; // 16px
  --space-lg: 1.5rem; // 24px
  --space-xl: 2rem; // 32px;

  /* ========== LAYOUT ========== */
  --header-height: 4rem; // 64px
  --sidebar-width: 15rem; // 240px

  /* ========== COLORS ========== */
  --color-bg: #f9fafb;
  --color-surface: #ffffff;
  --color-border: #e5e7eb;

  --color-text: #111827;
  --color-text-muted: #6b7280;

  --color-primary: #3f51b5;

  /* ========== RADIUS ========== */
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;

  /* ========== SHADOWS ========== */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
}



 /******************************************/
	src/styles/base/_reset.scss
 /******************************************/
// Browsers ship with their own default styles (margins, paddings, box model quirks). These differ slightly across engines.
// This file resets all of those styles to a consistent baseline, so that we can build our own design system on top of it.
*,
*::before,
*::after {
  box-sizing: border-box;
  // Default is `content-box`: width/height apply to content only,
  // so padding and border are added on top of the declared size.
  // This makes layout calculations harder and can cause overflow.
  // `border-box` includes padding and border in the element’s size,
  // leading to more predictable and stable layouts.
}

body {
  margin: 0;
}



 /******************************************/
	src/styles/base/_typography.scss
 /******************************************/
body {
  font-family: Roboto, sans-serif;
  font-size: 1rem; // 16px default
  color: var(--color-text);
  background-color: var(--color-bg);
}


 /******************************************/
	src/styles/utilities/_utilities.scss
 /******************************************/
/* margin */
.u-mt-sm {
  margin-top: var(--space-sm);
}
.u-mt-md {
  margin-top: var(--space-md);
}
.u-mb-md {
  margin-bottom: var(--space-md);
}

/* padding */
.u-p-sm {
  padding: var(--space-sm);
}
.u-p-md {
  padding: var(--space-md);
}
.u-p-lg {
  padding: var(--space-lg);
}


 /******************************************/
	src/styles/utilities/_flex.scss
 /******************************************/
.u-flex {
  display: flex;
}

.u-flex-col {
  display: flex;
  flex-direction: column;
}

.u-flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

.u-flex-between {
  display: flex;
  align-items: center;
  justify-content: space-between;
}



 /******************************************/
	src/styles/utilities/_layout.scss
 /******************************************/
.u-full-height {
  height: 100%;
}

.u-scroll-y {
  overflow-y: auto;
}

.u-border {
  border: 1px solid var(--color-border);
}

.u-surface {
  background: var(--color-surface);
}


 /******************************************/
	src/styles/main.scss
 /******************************************/
@use './abstracts/tokens';

@use './base/reset';
@use './base/typography';

@use './utilities/spacing';
@use './utilities/flex';
@use './utilities/layout';

@use './layout/layout';
@use './layout/auth-layout';




 /******************************************/
	src/styles/_main-layout.scss
 /******************************************/
.layout {
  height: 100vh;
  display: flex;
  flex-direction: column;

  &__header {
    height: var(--header-height);
    flex-shrink: 0;

    background: var(--color-surface);
    border-bottom: 1px solid var(--color-border);
  }

  &__body {
    flex: 1;
    display: flex;
    overflow: hidden;
  }

  &__sidebar {
    width: var(--sidebar-width);
    flex-shrink: 0;

    background: var(--color-surface);
    border-right: 1px solid var(--color-border);
  }

  &__content {
    flex: 1;
    overflow-y: auto;
    padding: var(--space-lg);
  }
}


 /******************************************/
	src/styles/_auth-layout.scss
 /******************************************/
.auth-layout {
  height: 100vh;

  display: flex;
  align-items: center;
  justify-content: center;

  padding: var(--space-lg);
}




