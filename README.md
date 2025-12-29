# Airbnb Clone Tutorial

A comprehensive step-by-step tutorial website for learning to build an Airbnb clone using React and Vite.

## 📚 About

This is a tutorial documentation site built with VitePress that guides you through building an Airbnb clone while learning React concepts step-by-step.

## 🚀 Getting Started

### Prerequisites

- Node.js (v16 or higher)
- npm or yarn

### Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/fron-proj.git
cd fron-proj
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

4. Open your browser and visit `http://localhost:5173`

## 📖 Tutorial Structure

The tutorial is organized into clear sections:

1. **Project Setup** - Initialize your Vite + React project
2. **Folder Structure** - Organize your codebase properly
3. **Components Explained** - Understand component architecture (Smart, Dumb, Reusable)
4. **Building Screens** - Create individual screens step-by-step
5. **Mock Data** - Use mock data for development
6. **Fetch & Axios** - Handle API calls
7. **Hooks & Lifecycle** - Master React hooks

## 🛠️ Development

- **Dev server**: `npm run dev`
- **Build**: `npm run build`
- **Preview**: `npm run preview`

## 📝 Content

All tutorial content is in the `/docs` directory:
- `/docs/index.md` - Homepage
- `/docs/guide/` - Tutorial sections

## 🚀 Deployment

### Deploy to GitHub Pages

1. Update `.vitepress/config.js` with your repository name:
```javascript
base: '/your-repo-name/'
```

2. Create `.github/workflows/deploy.yml` (already included)

3. Push to GitHub and GitHub Actions will deploy automatically

### Manual Deployment

```bash
npm run build
# The built files will be in docs/.vitepress/dist
```

## 📄 License

MIT

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
