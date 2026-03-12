# Architecture

## Overview

Resume v2 is a personal resume website built with Astro. It showcases Brent Timmermans' professional profile, current role, and social links with a clean, responsive design and dark mode support.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Framework** | Astro 5.16.6 |
| **Language** | TypeScript |
| **Styling** | CSS Custom Properties (CSS Variables) |
| **Linting/Formatting** | Biome 2.3.14 |
| **SEO** | Astro Sitemap Integration |
| **Package Manager** | npm |

## Directory Structure

```
resume-v2/
├── src/
│   ├── pages/              # Astro page routes
│   │   └── index.astro     # Home page (/)
│   ├── layouts/            # Layout components
│   │   └── Layout.astro    # Main HTML layout with meta tags
│   ├── components/         # Reusable Astro components
│   │   ├── Resume/         # Resume section components
│   │   │   ├── index.astro # Main resume container
│   │   │   ├── Hi.astro    # Greeting heading
│   │   │   ├── IAm.astro   # Description text
│   │   │   ├── CurrentRole.astro # Job title
│   │   │   ├── Avatar.astro # Profile image
│   │   │   ├── Friends.astro # Social connections
│   │   │   ├── Coffee.astro # Call-to-action
│   │   │   └── Socials.astro # Social media links
│   │   ├── Theme/          # Theme management
│   │   │   ├── Toggle.astro # Dark/light mode button
│   │   │   └── Init.astro  # Theme initialization script
│   │   └── common/         # Shared utilities
│   │       └── Stack.astro # Flexbox layout component
│   ├── styles/             # Global CSS
│   │   ├── reset.css       # CSS reset
│   │   ├── fonts.css       # Font imports
│   │   ├── variables.css   # CSS custom properties
│   │   └── global.css      # Global styles
│   └── assets/             # Static assets
│       ├── avatar.png      # Profile image
│       └── icons/          # SVG icons (moon, sun)
├── public/                 # Static files (favicon, etc.)
├── dist/                   # Build output
├── astro.config.mjs        # Astro configuration
├── tsconfig.json           # TypeScript configuration
├── biome.json              # Biome linter/formatter config
├── package.json            # Dependencies and scripts
└── README.md               # Project documentation
```

## Core Components

### Pages
- **`src/pages/index.astro`** - Single-page entry point that renders the Layout and Resume components

### Layouts
- **`src/layouts/Layout.astro`** - Main HTML wrapper with:
  - Meta tags (SEO, Open Graph, JSON-LD structured data)
  - CSS imports (reset, fonts, variables, global)
  - Theme toggle button
  - Theme initialization script
  - Responsive main container (max-width: 700px)

### Resume Section (`src/components/Resume/`)
- **`index.astro`** - Container component that orchestrates all resume sub-components using Stack layout
- **`Hi.astro`** - Greeting heading ("Hi")
- **`IAm.astro`** - Professional description
- **`CurrentRole.astro`** - Current job title and company
- **`Avatar.astro`** - Profile image with border styling
- **`Friends.astro`** - Social connections/recommendations section
- **`Coffee.astro`** - Call-to-action section
- **`Socials.astro`** - Social media links (GitHub, LinkedIn)

### Theme System (`src/components/Theme/`)
- **`Toggle.astro`** - Button component with moon/sun icons, handles theme switching
- **`Init.astro`** - Script that initializes theme from localStorage or system preference

### Utilities
- **`Stack.astro`** - Flexbox layout component with props for direction, spacing, alignment, justification

## Data Flow

1. **Page Load** → `src/pages/index.astro`
2. **Layout Render** → `Layout.astro` loads CSS and initializes theme
3. **Theme Init** → `Theme/Init.astro` script runs, sets `data-theme` attribute
4. **Resume Render** → `Resume/index.astro` composes all resume sub-components
5. **User Interaction** → Theme toggle button updates `data-theme` and localStorage
6. **CSS Variables** → CSS custom properties respond to `data-theme` attribute for light/dark mode

## Styling Architecture

### CSS Custom Properties (Variables)
- **Colors**: Light/dark mode variants (`--color-background-light`, `--color-text-primary-dark`, etc.)
- **Typography**: Font sizes (`--font-size-sm` through `--font-size-xl`)
- **Spacing**: Base unit `--spacing: 8px` (multiplied in components)
- **Transitions**: `--transition-duration: 0.3s`
- **Breakpoints**: `--breakpoint-large: 760px`, `--breakpoint-medium: 500px`, `--breakpoint-small: 450px`

### Theme Switching
- Light mode: Default CSS variables
- Dark mode: Overridden by `[data-theme="dark"]` selector
- Persistence: localStorage key `"theme"`
- System preference fallback: Uses `prefers-color-scheme` media query

### Responsive Design
- Mobile-first approach
- Breakpoints at 500px, 450px, and 760px
- Flexbox-based layout with Stack component
- Avatar and text reorder on smaller screens

## External Integrations

- **Astro Sitemap** - Generates `sitemap-index.xml` for SEO
- **GitHub** - Social link and preconnect optimization
- **LinkedIn** - Social link and preconnect optimization

## Configuration

### `astro.config.mjs`
- Site URL: `https://www.brenttimmermans.dev`
- Integrations: Sitemap plugin

### `biome.json`
- Formatter: Tab indentation, 80-char line width
- Linter: Recommended rules enabled
- JavaScript: Double quotes
- Astro overrides: Disabled unused variable/import rules

### `tsconfig.json`
- Extends: `astro/tsconfigs/strict`
- Includes: `.astro/types.d.ts` and all files
- Excludes: `dist` directory

## Build & Deploy

### Development
```bash
npm install
npm run dev  # Runs on http://localhost:4321
```

### Production
```bash
npm run build      # Creates dist/ directory
npm run preview    # Preview production build locally
```

### Code Quality
```bash
npm run lint       # Check with Biome
npm run lint:fix   # Auto-fix issues
npm run format     # Format code
npm run check      # Full Biome check
```

## Performance Optimizations

- Preconnect to GitHub and LinkedIn domains
- DNS prefetch for external social links
- CSS transitions for smooth theme switching
- Semantic HTML with JSON-LD structured data
- Static site generation (no JavaScript runtime overhead)
