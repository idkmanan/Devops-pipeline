# DevOps CI/CD Pipeline

A simple Node.js project demonstrating a CI/CD pipeline using GitHub Actions. The pipeline automatically runs tests on every push to the `main` branch.

## Pipeline Overview

The GitHub Actions workflow (`pipeline.yaml`) runs the following steps on each push:

| Step | Action | Description |
|------|--------|-------------|
| Checkout | `actions/checkout@v4` | Pulls the latest code |
| Setup Node.js | `actions/setup-node@v3` | Configures Node.js 18 |
| Install Dependencies | `npm install` | Installs project dependencies |
| Run Tests | `npm test` | Executes the test suite |

## Project Structure

```
.
├── .github/workflows/
│   └── pipeline.yaml      # GitHub Actions workflow definition
├── index.js                # Application entry point
├── package.json            # Project configuration & scripts
└── test.js                 # Test script
```

## Step-by-Step Setup Guide

### 1. Initialize the Node.js Project

Create the application and test files:

```bash
# package.json
echo '{
  "name": "devops-pipeline",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "run": "node index.js",
    "test": "node test.js"
  }
}' > package.json

# index.js
cat > index.js << 'EOF'
console.log("Hello Devops!")
console.log("I'm learning CI/CD pipelines using Github Actions")
EOF

# test.js
cat > test.js << 'EOF'
console.log("Starting Tests .....")
console.log("Running Tests")
setTimeout(()=>console.log("Test complete!!!"),3000);
EOF
```

### 2. Create the GitHub Actions Workflow

Create the directory structure and workflow file:

```bash
mkdir -p .github/workflows
```

Add `.github/workflows/pipeline.yaml`:

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: checkout code
        uses: actions/checkout@v4

      - name: setup node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test
```

### 3. Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit: Add CI Pipeline"
git branch -M main
git remote add origin https://github.com/idkmanan/Devops-pipeline.git
git push -u origin main
```

### 4. Verify the Pipeline

1. Go to your repository on GitHub
2. Click the **Actions** tab
3. You should see the workflow running (or already completed)
4. Click on the workflow run to see the logs for each step

### 5. Monitor Future Runs

Every subsequent push to `main` will automatically trigger the pipeline. Check the **Actions** tab for:

- Green checkmark — pipeline passed
- Red X — pipeline failed (click into the run to see which step failed)

## Running Locally

```bash
# Run the application
npm run

# Run tests
npm test
```

## Workflow Breakdown

- **Trigger:** `on.push.branches: [main]` — runs only on pushes to `main`
- **Runner:** `ubuntu-latest` — GitHub-hosted Ubuntu environment
- **Checkout:** Downloads the repository contents
- **Setup Node:** Installs Node.js 18.x
- **Install:** Runs `npm install` (currently no external dependencies)
- **Test:** Runs `node test.js`, a simulated async test with a 3-second delay
