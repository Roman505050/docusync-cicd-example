# Docusync CI/CD Example

An example of using [Docusync](https://github.com/Roman505050/docusync) to automatically sync documentation from multiple repositories and deploy to GitHub Pages using GitHub Actions.

## 📋 Description

This project demonstrates how to set up a complete CI/CD pipeline for:
- Automatically syncing documentation from other GitHub repositories using Docusync
- Automatically building a Docusaurus site
- Automatically deploying to GitHub Pages

## ✨ Features

- 🔄 **Automatic Sync**: Documentation is synced from source repositories on a schedule or when changes occur
- 🏗️ **Automatic Build**: Docusaurus site is automatically built after synchronization
- 🚀 **Automatic Deploy**: Site is automatically deployed to GitHub Pages
- 📝 **Automatic Commits**: Documentation changes are automatically committed back to the repository
- ⚙️ **SSH-free Setup**: Uses GitHub Token instead of SSH keys

## 🛠️ Technologies

- [Docusaurus](https://docusaurus.io/) - Static site generator for documentation
- [Docusync](https://github.com/Roman505050/docusync) - CLI tool for syncing documentation
- [GitHub Actions](https://github.com/features/actions) - CI/CD pipeline
- [pnpm](https://pnpm.io/) - Package manager
- [GitHub Pages](https://pages.github.com/) - Static site hosting

## 📦 Setup

### 1. Clone the repository

```bash
git clone https://github.com/Roman505050/docusync-cicd-example.git
cd docusync-cicd-example
```

### 2. Install dependencies

```bash
pnpm install
```

### 3. Configure docusync.json

Edit `docusync.json` to add your repositories:

```json
{
  "repositories": [
    {
      "github_path": "username/repo-name",
      "docs_path": "docs",
      "display_name": "Repository Name",
      "position": 1,
      "description": "Description of the repository"
    }
  ],
  "paths": {
    "temp_dir": ".temp-repos",
    "docs_dir": "docs"
  }
}
```

### 4. Configure GitHub Pages

1. Go to **Settings** → **Pages** in your repository
2. Under **Source**, select **GitHub Actions**
3. The workflow will automatically start deploying the site

### 5. Configure GitHub Actions

The workflow uses the built-in `GITHUB_TOKEN` with `contents: write` and `pages: write` permissions. No additional secrets are required for basic setup.

If you need access to private repositories, add a Personal Access Token as a secret:
- Create a PAT with `repo` permissions (for private repositories)
- Add the secret in the format: `GH_TOKEN_<USERNAME>` (e.g., `GH_TOKEN_DUCOSYNC_CI_CD_EXAMPLE`)
- Uncomment and configure the `env` section in `sync-docs.yml`

## 🔄 CI/CD Workflows

### Sync Documentation (`sync-docs.yml`)

Automatically syncs documentation from source repositories:

- **Triggers**:
  - On schedule (monthly, on the 15th at 02:00)
  - On push to `main` with changes to `sync-docs.yml` or `docusync.json`
  - Manually via `workflow_dispatch`

- **Steps**:
  1. Checkout repository
  2. Install dependencies (pnpm, Node.js, Python)
  3. Install Docusync
  4. Sync documentation
  5. Check for changes
  6. Build site
  7. Commit and push changes (if there are changes)

### Deploy to GitHub Pages (`deploy-pages.yml`)

Automatically deploys the site to GitHub Pages:

- **Triggers**:
  - On push to `main` with changes to relevant files
  - After completion of "Sync Documentation" workflow
  - Manually via `workflow_dispatch`

- **Steps**:
  1. Checkout repository
  2. Install dependencies
  3. Build Docusaurus site
  4. Deploy to GitHub Pages

## 🚀 Local Development

### Run dev server

```bash
pnpm start
```

The site will be available at `http://localhost:3000`

### Build for production

```bash
pnpm build
```

### Preview production build

```bash
pnpm serve
```

## 📝 Project Structure

```
.
├── .github/
│   └── workflows/
│       ├── sync-docs.yml      # Workflow for syncing documentation
│       └── deploy-pages.yml   # Workflow for deploying to GitHub Pages
├── docs/                       # Synced documentation
├── src/                        # Docusaurus components and styles
├── static/                     # Static files (images, etc.)
├── docusync.json              # Docusync configuration
├── docusaurus.config.ts       # Docusaurus configuration
└── package.json               # Project dependencies
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please open an issue or pull request to discuss changes.

## 🔗 Links

- [Docusync](https://github.com/Roman505050/docusync) - CLI tool for syncing documentation
- [Docusaurus](https://docusaurus.io/) - Docusaurus documentation
- [GitHub Actions](https://docs.github.com/en/actions) - GitHub Actions documentation
- [GitHub Pages](https://docs.github.com/en/pages) - GitHub Pages documentation
