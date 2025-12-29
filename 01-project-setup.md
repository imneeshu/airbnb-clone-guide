# 1. Project Setup with Vite & React

In this section, you'll learn how to set up a modern React project using Vite, a fast build tool for modern web projects.

## What is Vite?

**Vite** (pronounced "veet") is a build tool that provides:
- ⚡ Lightning-fast development server
- 🔥 Instant Hot Module Replacement (HMR)
- 📦 Optimized production builds
- 🛠️ Great developer experience

## Step-by-Step Setup

### Step 1: Create a New Project

Open your terminal and run:

```bash
npm create vite@latest airbnb-clone -- --template react
```

**Explanation:**
- `npm create vite@latest` - Creates a new Vite project
- `airbnb-clone` - Your project name
- `--template react` - Uses the React template

### Step 2: Navigate to Project Directory

```bash
cd airbnb-clone
```

### Step 3: Install Dependencies

```bash
npm install
```

This installs all the required packages including React, ReactDOM, and Vite.

### Step 4: Start Development Server

```bash
npm run dev
```

You should see something like:
```
  VITE v5.x.x  ready in xxx ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

Open `http://localhost:5173/` in your browser to see your app running!

## Project Structure Overview

After setup, your project will look like this:

```
airbnb-clone/
├── node_modules/          # Dependencies (don't edit)
├── public/                # Static assets
├── src/                   # Your source code
│   ├── assets/           # Images, fonts, etc.
│   ├── App.jsx           # Main app component
│   ├── App.css           # App styles
│   ├── index.css         # Global styles
│   └── main.jsx          # Entry point
├── index.html            # HTML template
├── package.json          # Project dependencies
├── vite.config.js        # Vite configuration
└── README.md             # Project documentation
```

## Key Files Explained

### `package.json`
Contains project metadata and scripts:
- `dev` - Starts development server
- `build` - Creates production build
- `preview` - Previews production build

### `vite.config.js`
Vite configuration file. For now, default settings work fine.

### `src/main.jsx`
Entry point of your React app. This is where React is mounted to the DOM.

### `src/App.jsx`
Root component. This is where you'll start building your application.

## What Happens When You Run `npm run dev`?

1. **Vite starts a development server** on port 5173
2. **Your code is served** through the dev server
3. **Changes are detected** automatically (HMR)
4. **Browser updates** instantly without page reload

## Your First Change

Let's make a simple change to verify everything works:

1. Open `src/App.jsx`
2. Replace the content with:

```jsx
function App() {
  return (
    <div className="App">
      <h1>Welcome to Airbnb Clone Tutorial</h1>
      <p>Your project is set up and ready!</p>
    </div>
  )
}

export default App
```

3. Save the file
4. Check your browser - it should update automatically!

## Additional Setup (Optional)

### Install Additional Dependencies

For our Airbnb clone, we'll need some additional packages:

```bash
npm install react-router-dom axios
```

- `react-router-dom` - For navigation between pages
- `axios` - For making HTTP requests

We'll learn more about these in later sections.

## ✅ Checkpoint

You should now have:
- ✅ A working React + Vite project
- ✅ Development server running
- ✅ Basic understanding of the project structure

## Next Steps

Once your project is set up, move to the next section:

**[Folder & File Structure →](./02-folder-structure.md)**

---

**File to create/update:** `src/App.jsx`
**Code to use:** (shown above)

