# 2. Folder & File Structure

A well-organized folder structure makes your codebase maintainable, scalable, and easy to navigate. In this section, we'll set up the proper structure for our Airbnb clone.

## Why Organization Matters

Good structure helps:
- 🎯 Find files quickly
- 👥 Collaborate easily
- 🔄 Maintain and update code
- 📚 Understand the project at a glance

## Recommended Folder Structure

Create the following structure in your `src` folder:

```
src/
├── assets/              # Static files (images, fonts, etc.)
│   └── images/
│       └── logo.svg
│
├── components/          # Reusable UI components
│   ├── common/         # Common reusable components
│   │   ├── Button/
│   │   │   ├── Button.jsx
│   │   │   ├── Button.css
│   │   │   └── index.js
│   │   ├── Card/
│   │   └── Input/
│   └── layout/         # Layout components
│       ├── Header/
│       ├── Footer/
│       └── Navigation/
│
├── pages/              # Page/screen components
│   ├── Home/
│   │   ├── Home.jsx
│   │   ├── Home.css
│   │   └── index.js
│   ├── Listing/
│   ├── PropertyDetail/
│   └── Search/
│
├── hooks/              # Custom React hooks
│   ├── useProperties.js
│   └── useSearch.js
│
├── services/           # API calls and external services
│   ├── api.js
│   └── propertyService.js
│
├── data/               # Mock data (for development)
│   └── mockProperties.js
│
├── utils/              # Helper functions
│   └── helpers.js
│
├── styles/             # Global styles
│   ├── variables.css
│   └── global.css
│
├── App.jsx             # Main app component
├── App.css
├── main.jsx            # Entry point
└── index.css           # Base styles
```

## Creating the Structure

### Step 1: Create Folders

Run these commands in your terminal (or create folders manually):

```bash
# From your project root (airbnb-clone)
mkdir -p src/components/common
mkdir -p src/components/layout
mkdir -p src/pages
mkdir -p src/hooks
mkdir -p src/services
mkdir -p src/data
mkdir -p src/utils
mkdir -p src/styles
mkdir -p src/assets/images
```

### Step 2: Create Index Files for Components

For easier imports, create `index.js` files in component folders:

**File: `src/components/common/Button/index.js`**
```javascript
export { default } from './Button'
```

**File: `src/components/common/Card/index.js`**
```javascript
export { default } from './Card'
```

This allows you to import like: `import Button from '@/components/common/Button'` instead of `import Button from '@/components/common/Button/Button'`

## Folder Purposes Explained

### `/components`
Reusable UI components that can be used across multiple pages.

### `/components/common`
Components used frequently throughout the app (Button, Card, Input, etc.)

### `/components/layout`
Layout components that structure the app (Header, Footer, Navigation)

### `/pages`
Full page/screen components. Each folder represents one screen.

### `/hooks`
Custom React hooks for reusable logic.

### `/services`
Functions that handle API calls and external integrations.

### `/data`
Mock data files for development and testing.

### `/utils`
Helper functions and utilities (formatters, validators, etc.)

### `/styles`
Global styles, CSS variables, and shared stylesheets.

### `/assets`
Static files like images, icons, fonts.

## Component File Naming Convention

Each component should have:
- **ComponentName.jsx** - Component logic
- **ComponentName.css** - Component styles
- **index.js** - Export file (optional but recommended)

Example:
```
Button/
├── Button.jsx
├── Button.css
└── index.js
```

## Setting Up Path Aliases (Optional but Recommended)

To avoid relative path hell (`../../../../`), configure path aliases in `vite.config.js`:

**File: `vite.config.js`**
```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@pages': path.resolve(__dirname, './src/pages'),
      '@hooks': path.resolve(__dirname, './src/hooks'),
      '@services': path.resolve(__dirname, './src/services'),
      '@utils': path.resolve(__dirname, './src/utils'),
    }
  }
})
```

Now you can import like:
```javascript
import Button from '@components/common/Button'
import { useProperties } from '@hooks/useProperties'
```

## Initial Setup Files

Let's create a few starter files:

### 1. Global Styles

**File: `src/styles/variables.css`**
```css
:root {
  /* Colors */
  --primary-color: #FF385C;
  --secondary-color: #00A699;
  --text-primary: #222222;
  --text-secondary: #717171;
  --border-color: #DDDDDD;
  --bg-white: #FFFFFF;
  --bg-light: #F7F7F7;
  
  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  
  /* Typography */
  --font-family: 'Circular', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  
  /* Border Radius */
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
}
```

**File: `src/styles/global.css`**
```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: var(--font-family);
  color: var(--text-primary);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

a {
  text-decoration: none;
  color: inherit;
}

button {
  font-family: inherit;
  cursor: pointer;
}
```

Update `src/main.jsx` to import global styles:

**File: `src/main.jsx`**
```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'
import './styles/variables.css'
import './styles/global.css'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

### 2. Placeholder Component Structure

**File: `src/components/common/Button/Button.jsx`**
```javascript
import './Button.css'

function Button({ children, onClick, variant = 'primary' }) {
  return (
    <button 
      className={`button button--${variant}`}
      onClick={onClick}
    >
      {children}
    </button>
  )
}

export default Button
```

**File: `src/components/common/Button/Button.css`**
```css
.button {
  padding: 12px 24px;
  border: none;
  border-radius: var(--radius-sm);
  font-size: var(--font-size-base);
  font-weight: 600;
  transition: all 0.2s;
}

.button--primary {
  background-color: var(--primary-color);
  color: white;
}

.button--primary:hover {
  background-color: #E61E4D;
}
```

**File: `src/components/common/Button/index.js`**
```javascript
export { default } from './Button'
```

## ✅ Checkpoint

You should now have:
- ✅ Organized folder structure
- ✅ Understanding of each folder's purpose
- ✅ Path aliases configured (optional)
- ✅ Global styles set up
- ✅ Sample component structure

## Next Steps

Now that your structure is ready, let's understand components:

**[Components Explained →](./03-components.md)**

---

**Files Created:**
- Folder structure in `src/`
- `src/styles/variables.css`
- `src/styles/global.css`
- `src/components/common/Button/` (sample component)

