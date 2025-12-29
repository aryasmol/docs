# Deployment Instructions

To publish this documentation to the internet, we recommend using **Mintlify**.

## Prerequisites
1.  A GitHub repository containing this `atoms_docs` folder.
2.  A Mintlify account (https://mintlify.com).

## Steps

### 1. Push to GitHub
If you haven't already, push your code to a GitHub repository.

```bash
git init
git add atoms_docs/
git commit -m "Add Atoms SDK documentation"
# git remote add origin <your-repo-url>
git push origin main
```

### 2. Connect to Mintlify
1.  Log in to [dashboard.mintlify.com](https://dashboard.mintlify.com).
2.  Click **New Project**.
3.  Select your GitHub repository.
4.  When asked for the directory, specify: `atoms_docs`.
5.  Click **Deploy**.

Mintlify will automatically detect the `mint.json` configuration and build your documentation site.

### 3. Verification
Once deployed, Mintlify will provide a URL (e.g., `https://your-project.mintlify.app`). Visit this URL to ensure your documentation is live and formatted correctly.
