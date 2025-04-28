# GitHub Actions Workflow Templates

This repository provides a suite of **reusable GitHub Actions workflow templates** designed to accelerate CI/CD setup for common frontend, backend, and release tasks. Drop these YAML files into your own repository’s `.github/workflows/` directory and customize branch names or parameters as needed or simply copy the steps that you need into your existing workflows.

## Available Templates

### 1. `frontend_utils.yaml`

A complete CI/CD pipeline for single‑page applications built with Node.js and Vite (Vue, React, etc.):
   - **build-and-test**: Checks out code, sets up Node.js, installs dependencies, runs unit tests (`npm run test:unit`), builds production assets (`npm run build:prod`), then commits and pushes the `dist/` output back to the `deploy-branch`.  
  - **deploy-production**: Merges `deploy-branch` into `prod`, cleans up the workspace, moves built assets from `dist/` to root, commits & pushes to `prod`.  
  - **rollback-all**: On any failure, restores `prod` from a temporary backup branch and resets `deploy-branch`.  
  - **cleanup-workspace**: Always runs last to delete temp branches and clear the workspace.


### 2. `backend_utils.yaml`

A flexible workflow for backend deployments (PHP Laravel, Java, etc.):
  - **replace-env-file**: Replaces `.env` with `.env.production` (forces add, commits & pushes), removes `.env.development`.  
  - **rollback-all**: On failure, reverts `.env` from `.env.development` backup and force-pushes to preserve local dev state.

### 3. `versioning_utils.yaml`

Automates version management and GitHub releases:

  - **check-deploy-version**: Reads `version` from `package.json`, bumps patch version if a matching Git tag already exists, commits & pushes the new version.  
  - **create-release**: After successful CI and deployment, reads `CHANGELOG.txt` (or uses a default message), creates a GitHub Release with tag `PRODUCTION_V_<version>` and uses `actions/create-release@v1`. Empties `CHANGELOG.txt` on success.  
  - **rollback-all**: If release creation fails, deletes the created GitHub Release and tag using the GitHub REST API.

## Usage

1. **Integration** 
  - Copy the desired template file(s) into your project under:
    .github/workflows/frontend_utils.yaml 
    .github/workflows/backend_utils.yaml 
    .github/workflows/versioning_utils.yaml

1. **Customize**:
  - Change branch names (`deploy-branch`, `prod` or `prod-branch`, etc.) to match your workflow.  
  - Adjust step commands (`npm run build:prod`, Laravel commands, etc.) as necessary.  
  
2. **Commit & push** to your repository. The workflows will run automatically on the configured events.

---

> For detailed examples and parameter customization, refer to the comments inside each YAML template.