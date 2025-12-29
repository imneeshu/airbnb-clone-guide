# Deployment Guide

## Publishing to GitHub Pages

Follow these steps to publish your tutorial website:

### Step 1: Create GitHub Repository

1. Go to GitHub and create a new repository
2. Name it (e.g., `fron-proj` or `airbnb-clone-tutorial`)
3. **Don't** initialize with README (we already have files)

### Step 2: Update Configuration

Before pushing, update the repository name in the config:

**File: `docs/.vitepress/config.js`**

```javascript
base: '/your-repo-name/',  // Change this to match your GitHub repo name
```

Also update the GitHub link:
```javascript
socialLinks: [
  { icon: 'github', link: 'https://github.com/yourusername/your-repo-name' }
]
```

### Step 3: Initialize Git and Push

```bash
# Initialize git (if not already done)
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit: Airbnb Clone Tutorial website"

# Add remote (replace with your GitHub URL)
git remote add origin https://github.com/yourusername/your-repo-name.git

# Push to main branch
git branch -M main
git push -u origin main
```

### Step 4: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** → **Pages**
3. Under **Source**, select **GitHub Actions**
4. The GitHub Actions workflow will automatically deploy your site

### Step 5: Access Your Site

After the workflow runs (usually takes 1-2 minutes), your site will be available at:
```
https://yourusername.github.io/your-repo-name/
```

## Automatic Deployment

The `.github/workflows/deploy.yml` file is already configured to:
- Automatically build your site when you push to `main` branch
- Deploy to GitHub Pages
- No manual steps needed after initial setup!

## Manual Deployment (Alternative)

If you prefer manual deployment:

```bash
# Build the site
npm run build

# The built files are in docs/.vitepress/dist
# You can then deploy these files manually
```

## Troubleshooting

### Site not loading?

- Check that `base` path in config matches your repo name
- Ensure GitHub Pages is enabled in repository settings
- Check GitHub Actions workflow for errors

### 404 errors?

- Verify the `base` path in `docs/.vitepress/config.js` matches your repository name exactly
- Make sure there are no typos

### Build fails?

- Check Node.js version (needs v16+)
- Run `npm install` to ensure dependencies are installed
- Check the Actions tab in GitHub for error details

## Updating Content

To update your site:
1. Edit the markdown files in `/docs`
2. Commit and push changes
3. GitHub Actions will automatically rebuild and deploy

## Custom Domain (Optional)

To use a custom domain:
1. Add a `CNAME` file in `/docs/public/` with your domain
2. Configure DNS settings with your domain provider
3. Update GitHub Pages settings with your custom domain

