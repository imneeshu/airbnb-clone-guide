# Setup Instructions

## Quick Start Guide

Follow these steps to get your tutorial website up and running:

### 1. Install Dependencies

```bash
npm install
```

### 2. Update Repository Name (Important!)

Before deploying, update the repository name in the config file:

**File: `docs/.vitepress/config.js`**

Change this line:
```javascript
base: '/fron-proj/',
```

To match your GitHub repository name:
```javascript
base: '/your-repo-name/',
```

Also update the GitHub link in the same file:
```javascript
socialLinks: [
  { icon: 'github', link: 'https://github.com/yourusername/your-repo-name' }
]
```

### 3. Run Development Server

```bash
npm run dev
```

Visit `http://localhost:5173` to see your site!

### 4. Build for Production

```bash
npm run build
```

### 5. Deploy to GitHub Pages

#### Option A: Automatic Deployment (Recommended)

1. Push your code to GitHub
2. Go to your repository Settings → Pages
3. Under "Source", select "GitHub Actions"
4. The workflow will automatically deploy when you push to `main` branch

#### Option B: Manual Deployment

1. Build the site: `npm run build`
2. The built files are in `docs/.vitepress/dist`
3. Push the `dist` folder contents to `gh-pages` branch

### 6. Customize Content

All tutorial content is in the `/docs` directory:
- Edit markdown files to update content
- Add new guides in `/docs/guide/`
- Update navigation in `docs/.vitepress/config.js`

## Project Structure

```
fron-proj/
├── docs/                    # All documentation content
│   ├── .vitepress/         # VitePress configuration
│   │   └── config.js       # Site config (update base path here!)
│   ├── guide/              # Tutorial guides
│   └── index.md            # Homepage
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Actions deployment
├── package.json
└── README.md
```

## Tips

- The `base` path in config.js must match your GitHub repository name
- Use relative links for images: `./image.png`
- VitePress uses Markdown with enhanced features
- Hot reload works during development

## Need Help?

Check the [VitePress Documentation](https://vitepress.dev/) for more details.

