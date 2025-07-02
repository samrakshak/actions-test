# 📘 GitHub Actions Keywords – Full Examples

This guide includes **complete YAML examples** for all major GitHub Actions keywords. Copy and paste directly into `.github/workflows/your-workflow.yml`.



DAY 1
---

## Simple Example

```yaml
name: GitHub Actions Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v4
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```

## 🟢 1. `name`

```yaml
name: CI Pipeline

on: [push]  or pull_request 

jobs:
  say-hello:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Hello world!"
```

---

## 🟢 2. `run-name`

```yaml
name: Demo Workflow

run-name: "Triggered by ${{ github.actor }} on ${{ github.ref }}"

on: [push]

jobs:
  print:
    runs-on: ubuntu-latest
    steps:
      - run: echo "This run has a dynamic name!"
```

---

## 🟢 3. `on`

```yaml
name: Multi-Event Trigger

on:
  push:
    branches: [main]
  pull_request:
    types: [opened, synchronize]
  schedule:
    - cron: '0 0 * * 1' # Every Monday

jobs:
  trigger-example:
    runs-on: ubuntu-latest
    steps:
      - run: echo "This workflow was triggered by ${{ github.event_name }}"
```

---

## 🟢 4. `permissions` (Workflow-Level)

```yaml
name: Restricted Permissions

on: [push]

permissions:
  contents: read
  issues: write

jobs:
  show-permissions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Permissions have been set for this workflow."
```

---

## 🟢 5. `env` (Global Environment Variables)

```yaml
name: Global Environment Variables

on: [push]

env:
  GLOBAL_MESSAGE: "Hello from env!"
  FOO: "Bar"

jobs:
  show-env:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Message: $GLOBAL_MESSAGE and Foo: $FOO"
```

---

## 🟢 6. `defaults`

```yaml
name: Defaults Example

on: [push]

defaults:
  run:
    shell: bash
    working-directory: scripts

jobs:
  show-defaults:
    runs-on: ubuntu-latest
    steps:
      - name: Create directory and file
        run: |
          mkdir -p scripts
          echo "echo 'Hello from default working-directory'" > scripts/hello.sh
      - name: Run script
        run: bash hello.sh
```

---

## 🟢 7. `concurrency`

```yaml
name: Concurrency Example

on: [push]

concurrency:
  group: deploy-${{ github.ref }}
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying... Only one deployment per branch can run."
```

---

# 🟡 JOB-LEVEL KEYWORDS

---

## 🟢 8. `runs-on`

```yaml
name: Runner Example

on: [push]

jobs:
  run-on-ubuntu:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Running on Ubuntu!"
```

---

## 🟢 9. `needs`

```yaml
name: Needs Example

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building..."

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing after build..."
```

---

## 🟢 10. `if`

```yaml
name: Conditional Job

on: [push]

jobs:
  only-on-main:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - run: echo "This only runs on the main branch!"
```

---

## 🟢 11. `strategy` (Matrix Builds)

```yaml
name: Matrix Example

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [14, 16]
        os: [ubuntu-latest, windows-latest]
      fail-fast: false
      max-parallel: 2
    steps:
      - run: echo "Testing on Node ${{ matrix.node }} and OS ${{ matrix.os }}"
```

---

## 🟢 12. `environment`

```yaml
name: Environment Example

on: [push]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://example.com
    steps:
      - run: echo "Deploying to production environment!"
```

---

## 🟢 13. `timeout-minutes`

```yaml
name: Timeout Example

on: [push]

jobs:
  short-job:
    runs-on: ubuntu-latest
    timeout-minutes: 1
    steps:
      - run: sleep 120 # This will fail after 1 minute
```

---

## 🟢 14. `continue-on-error`

```yaml
name: Continue on Error

on: [push]

jobs:
  try-anyway:
    runs-on: ubuntu-latest
    continue-on-error: true
    steps:
      - run: exit 1
      - run: echo "This step still runs despite failure."
```

---

## 🟢 15. `container`

```yaml
name: Container Job

on: [push]

jobs:
  node-inside-container:
    runs-on: ubuntu-latest
    container:
      image: node:20
      env:
        NODE_ENV: development
    steps:
      - run: node -v
```

---

## 🟢 16. `services`

```yaml
name: Services Example

on: [push]

jobs:
  test-with-postgres:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_PASSWORD: postgres
        ports:
          - 5432:5432
    steps:
      - run: |
          sudo apt-get install -y postgresql-client
          psql -h localhost -U postgres -c "SELECT version();"
        env:
          PGPASSWORD: postgres
```

---

## 🟢 17. `outputs`

```yaml
name: Outputs Example

on: [push]

jobs:
  generate:
    runs-on: ubuntu-latest
    outputs:
      custom-message: ${{ steps.set.outputs.message }}
    steps:
      - id: set
        run: echo "message=Hello from the build job!" >> $GITHUB_OUTPUT

  consume:
    needs: generate
    runs-on: ubuntu-latest
    steps:
      - run: echo "The message was: ${{ needs.generate.outputs.custom-message }}"
```

---

## 🟢 18. `permissions` (Job-Level)

```yaml
name: Job-Level Permissions

on: [push]

jobs:
  secure-job:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      issues: write
    steps:
      - run: echo "This job has limited permissions."
```


All Permission Declared

```yaml
permissions:
  actions: write
  checks: write
  contents: write
  deployments: write
  issues: write
  discussions: write
  packages: write
  pages: write
  pull-requests: write
  repository-projects: write
  security-events: write
  statuses: write

```

```yaml
| Use Case                     | Needed Permissions       |
| ---------------------------- | ------------------------ |
| Read repo contents           | `contents: read`         |
| Push commits/tags            | `contents: write`        |
| Create GitHub Releases       | `contents: write`        |
| Create/close issues          | `issues: write`          |
| Manage deployments           | `deployments: write`     |
| Update pull request statuses | `statuses: write`        |
| Access packages (e.g., GHCR) | `packages: read/write`   |
| Write security advisories    | `security-events: write` |

```


Sure! Here’s a **clear, complete GitHub Actions workflow** that:

✅ Logs in to Docker Hub
✅ Builds a Docker image
✅ Pushes it to your Docker Hub repository

Below, I’ll give you:

* a **basic example**,
* a **versioned/tagged example**,
* and an explanation of each step.

---

## 🟢 Simple Example: Docker Build and Push

This workflow runs on every push to `main`:

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      # Checkout your code
      - uses: actions/checkout@v4

      # Log in to Docker Hub
      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      # Build the Docker image
      - name: Build Docker image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/myapp:latest .

      # Push the Docker image
      - name: Push Docker image
        run: docker push ${{ secrets.DOCKER_USERNAME }}/myapp:latest
```

---

✅ **How to set up your secrets:**

In your repository:

1. Go to **Settings > Secrets and variables > Actions**
2. Click **New repository secret**
3. Add:

   * `DOCKER_USERNAME` – your Docker Hub username
   * `DOCKER_PASSWORD` – a Docker Hub [personal access token](https://hub.docker.com/settings/security) (recommended instead of password)

---

## 🟢 Example: Build and Push with Git Tag Versioning

This version uses the Git tag as the Docker tag:

```yaml
name: Docker Build and Push on Tag

on:
  push:
    tags:
      - 'v*.*.*'   # e.g., v1.2.3

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker image with tag
        run: |
          TAG=${GITHUB_REF#refs/tags/}
          docker build -t ${{ secrets.DOCKER_USERNAME }}/myapp:${TAG} .

      - name: Push Docker image
        run: |
          TAG=${GITHUB_REF#refs/tags/}
          docker push ${{ secrets.DOCKER_USERNAME }}/myapp:${TAG}
```

✅ This way, if you push a tag `v1.0.0`, your Docker image will be tagged `v1.0.0`.

---

## 🟢 Example: Build and Push Both `latest` and Tag

Many teams push both a semantic version and `latest`:

```yaml
name: Docker Build and Push on Tag

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker images
        run: |
          TAG=${GITHUB_REF#refs/tags/}
          docker build -t ${{ secrets.DOCKER_USERNAME }}/myapp:${TAG} -t ${{ secrets.DOCKER_USERNAME }}/myapp:latest .

      - name: Push Docker images
        run: |
          TAG=${GITHUB_REF#refs/tags/}
          docker push ${{ secrets.DOCKER_USERNAME }}/myapp:${TAG}
          docker push ${{ secrets.DOCKER_USERNAME }}/myapp:latest
```

---

## 🟢 Recap of What Each Step Does

| Step                     | Purpose                                    |
| ------------------------ | ------------------------------------------ |
| `actions/checkout`       | Clones your repository                     |
| `docker/login-action@v2` | Logs in securely to Docker Hub             |
| `docker build`           | Builds your Docker image                   |
| `docker push`            | Pushes your image to Docker Hub repository |

---

Absolutely—let’s **restate the final assignment** in a clean, clear, *implementation-agnostic* way, including your new requirements:

---
## DAY 2

Absolutely—let’s **upgrade this into a professional DevOps training pack**.

Below is **clear, structured content** with **full examples**, **best practices**, and **practical context** for your interns.

---

# 🎓 GitHub Actions Advanced DevOps Training

✅ **Target Audience:** Junior engineers and interns
✅ **Focus:** Real-world workflows and reusable examples
✅ **Skill Level:** Intermediate to Advanced

---

## 🟢 1️⃣ Environment Variables, Secrets & Environments

---

### ✅ 1.1 Default GitHub Environment Variables

GitHub provides built-in variables automatically:

| Variable            | Description            |
| ------------------- | ---------------------- |
| `GITHUB_REPOSITORY` | Owner/repo name        |
| `GITHUB_REF`        | Branch or tag ref      |
| `GITHUB_SHA`        | Commit SHA             |
| `GITHUB_RUN_NUMBER` | Workflow run number    |
| `GITHUB_WORKSPACE`  | Working directory path |

**Example:**

```yaml
jobs:
  show-defaults:
    runs-on: ubuntu-latest
    steps:
      - run: |
          echo "Repository: $GITHUB_REPOSITORY"
          echo "Branch/Tag: $GITHUB_REF"
          echo "Commit SHA: $GITHUB_SHA"
```

---

### ✅ 1.2 Custom Environment Variables

You can define custom variables globally, per job, or per step.

**Workflow-level (global):**

```yaml
env:
  APP_ENV: production
  GLOBAL_VAR: "Available everywhere"

jobs:
  print-vars:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Workflow-level APP_ENV = $APP_ENV"
```

---

**Job-level:**

```yaml
jobs:
  job-vars:
    runs-on: ubuntu-latest
    env:
      JOB_VAR: "Only in this job"
    steps:
      - run: echo "JOB_VAR = $JOB_VAR"
```

---

**Step-level:**

```yaml
jobs:
  step-vars:
    runs-on: ubuntu-latest
    steps:
      - name: Print step variable
        run: echo "STEP_VAR = $STEP_VAR"
        env:
          STEP_VAR: "Visible in this step only"
```

---

### ✅ 1.3 GitHub Secrets

Secrets are **encrypted variables** stored securely.

**How to add:**

1. Go to **Settings > Secrets and Variables > Actions**.
2. Click **New repository secret**.

Example: `DOCKER_PASSWORD`

**Usage:**

```yaml
jobs:
  show-secret:
    runs-on: ubuntu-latest
    steps:
      - run: echo "My secret is ${{ secrets.DOCKER_PASSWORD }}"
```

---

### ✅ 1.4 GitHub Environments

**Environments** provide:

* Scoped secrets per environment.
* Protection rules (approvals).
* Deployment history.

**Example:**

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - run: echo "Deploying to production"
      - run: echo "Secret = ${{ secrets.PRODUCTION_API_KEY }}"
```

✅ **Tip:** Use environments for staging/production separation.

---

## 🟢 2️⃣ Caching Dependencies (Node.js frontend & backend)

---

### ✅ 2.1 Why Caching Matters

* Speeds up builds by reusing `node_modules`.
* Avoids downloading dependencies every time.

---

### ✅ 2.2 Node.js Monorepo Example

Imagine this structure:

```
/
  frontend/
    package.json
    package-lock.json
  backend/
    package.json
    package-lock.json
```

---

**Example: Caching frontend dependencies**

```yaml
# .github/workflows/cache-node-modules.yml
name: Cache Node Modules

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Use Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Cache node_modules
        uses: actions/cache@v4
        with:
          path: |
            **/node_modules
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-

      - name: Install Dependencies
        run: npm ci

      - name: Build
        run: npm run build

```

---

**Example: Caching backend dependencies**

```yaml
jobs:
  build-backend:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: backend
    steps:
      - uses: actions/checkout@v4

      - uses: actions/cache@v4
        with:
          path: backend/node_modules
          key: backend-${{ runner.os }}-${{ hashFiles('backend/package-lock.json') }}
          restore-keys: backend-${{ runner.os }}-

      - uses: actions/setup-node@v4
        with:
          node-version: 18

      - run: npm ci
      - run: npm run build
```

✅ **Note:** This pattern ensures **separate caches for frontend and backend**.

---

## 🟢 3️⃣ Using Marketplace Actions

---

**Marketplace actions save time.**

---

**Example: Setup Node, cache, and upload build artifacts**

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 18

      - uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}

      - run: npm ci
      - run: npm run build

      - uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/
```

✅ **Tip:** Always check the Marketplace rating before using third-party actions.

---

## 🟢 4️⃣ Creating Your Own Reusable Actions

---

### ✅ 4.1 Composite Action Example

**Directory: `.github/actions/hello-world/action.yml`**

```yaml
name: Hello World
description: Greets the user

inputs:
  who:
    description: Who to greet
    required: true
    default: World

runs:
  using: "composite"
  steps:
    - run: echo "Hello, ${{ inputs.who }}!"
```

---

**Usage:**

```yaml
jobs:
  greet:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: ./.github/actions/hello-world
        with:
          who: "Interns"
```


Absolutely — let’s look at **GitHub Actions examples** covering both:

✅ **Caching** (to speed up builds)
✅ **Creating Releases and Tags**

Below, I’ll share clear, ready-to-use examples you can adapt.

---

## 🎯 1️⃣ Example: Caching Dependencies

**Node.js example caching `node_modules`**

```yaml
name: Node.js CI with Cache

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Cache node_modules
        uses: actions/cache@v4
        with:
          path: node_modules
          key: node-modules-${{ runner.os }}-${{ hashFiles('package-lock.json') }}
          restore-keys: |
            node-modules-${{ runner.os }}-

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

👉 **How it works:**

* `actions/cache` saves `node_modules` keyed by `package-lock.json` hash.
* If lock file hasn’t changed, dependencies are restored instantly.

---

## 🎯 2️⃣ Example: Caching Docker Layers

**Docker build cache example**

```yaml
name: Docker Build with Cache

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Cache Docker layers
        uses: actions/cache@v4
        with:
          path: /tmp/.buildx-cache
          key: buildx-${{ github.sha }}
          restore-keys: |
            buildx-

      - name: Build Docker image
        run: |
          docker build \
            --cache-from=type=local,src=/tmp/.buildx-cache \
            --cache-to=type=local,dest=/tmp/.buildx-cache \
            -t my-app:latest .
```

---

## 🎯 3️⃣ Example: Creating Release & Tag Automatically

This example **creates a GitHub Release and Tag** when you push to `main` with a version bump in a file (e.g., `package.json`).

```yaml
name: Create Release and Tag

on:
  push:
    branches:
      - main

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Get version
        id: get_version
        run: |
          VERSION=$(jq -r '.version' package.json)
          echo "version=$VERSION" >> $GITHUB_OUTPUT

      - name: Create Tag
        run: git tag v${{ steps.get_version.outputs.version }}

      - name: Push Tag
        run: git push origin v${{ steps.get_version.outputs.version }}

      - name: Create GitHub Release
        uses: softprops/action-gh-release@v2
        with:
          tag_name: v${{ steps.get_version.outputs.version }}
          name: Release v${{ steps.get_version.outputs.version }}
          draft: false
          prerelease: false
```

👉 **Make sure:**

* Your `package.json` has `"version": "x.y.z"`.
* You have `GITHUB_TOKEN` permissions to create tags/releases (default).

---

## 🎯 4️⃣ Example: Combined — Build, Cache, Release

This shows **caching + creating release and tag together**.

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build-release:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Cache node_modules
        uses: actions/cache@v4
        with:
          path: node_modules
          key: node-modules-${{ runner.os }}-${{ hashFiles('package-lock.json') }}
          restore-keys: |
            node-modules-${{ runner.os }}-

      - name: Install dependencies
        run: npm ci

      - name: Run Tests
        run: npm test

      - name: Get Version
        id: get_version
        run: |
          VERSION=$(jq -r '.version' package.json)
          echo "version=$VERSION" >> $GITHUB_OUTPUT

      - name: Create Tag
        run: git tag v${{ steps.get_version.outputs.version }}

      - name: Push Tag
        run: git push origin v${{ steps.get_version.outputs.version }}

      - name: Create GitHub Release
        uses: softprops/action-gh-release@v2
        with:
          tag_name: v${{ steps.get_version.outputs.version }}
          name: Release v${{ steps.get_version.outputs.version }}
```

---

✅ **Quick Tips**

* `actions/cache` is used for dependencies or Docker layers.
* `softprops/action-gh-release` is a popular action for creating releases.
* Use **semantic versioning** (`v1.2.3`) in your tags.
* You can combine with workflows that lint, test, and deploy.

---

If you’d like, I can help you adapt these examples to:

* Python
* Go
* Java
* Docker publishing
* Semantic Release (fully automated version bumping)

Just share more details!


✅ **Tip:** Use composite actions to DRY up repetitive steps across workflows.

---

## 🟢 5️⃣ Semantic Versioning

---

### ✅ 5.1 Principles

* **MAJOR** – incompatible API changes
* **MINOR** – backward-compatible features
* **PATCH** – bug fixes

---

**Example: Automate version bump and tagging with `semantic-release`**

---

**Workflow:**

```yaml
name: Semantic Release

on:
  push:
    branches:
      - main

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: actions/setup-node@v4
        with:
          node-version: 18

      - run: npm install -g semantic-release @semantic-release/git @semantic-release/github

      - run: semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

**`package.json` (scripts):**

```json
{
  "release": "semantic-release"
}
```

---

✅ **Tip:** Configure `.releaserc` for your changelog format.

---

## 🟢 6️⃣ Creating GitHub Releases and Tags

---

### ✅ 6.1 Create Release on Tag Push

```yaml
name: Release

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: softprops/action-gh-release@v1
        with:
          tag_name: ${{ github.ref_name }}
          name: Release ${{ github.ref_name }}
          body: |
            🎉 Automated release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

**✅ Example Tag:**

```
v1.2.3
```

Creates a GitHub release with that tag.

---

## 🟢 7️⃣ Generating Release Notes Automatically

---

**Example using `release-drafter`:**

**`.github/release-drafter.yml`:**

```yaml
name-template: 'v$NEXT_PATCH_VERSION'
tag-template: 'v$NEXT_PATCH_VERSION'
categories:
  - title: 🚀 Features
    labels: feature
  - title: 🐛 Fixes
    labels: fix
```

**Workflow:**

```yaml
on:
  push:
    branches:
      - main

jobs:
  draft-release:
    runs-on: ubuntu-latest
    steps:
      - uses: release-drafter/release-drafter@v6
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

✅ **Tip:** Release Drafter automatically updates your draft release notes as you merge PRs.

---

# 🎯 Recap of What You Covered

✅ Environment Variables (default, custom, secrets, environments)
✅ Caching Node.js builds (frontend + backend)
✅ Using Marketplace Actions
✅ Creating Your Own Actions
✅ Semantic Versioning Automation
✅ Creating GitHub Releases and Tags
✅ Generating Release Notes

---

If you want, I can:

* Help you design **slide decks**
* Prepare **assignments and solutions**
* Create **grading rubrics**
* Write additional examples (e.g., Python, Java, Docker)

Just say the word! 🌟







## 🎓 **Final Assignment**

You will set up a **monorepo** containing both a frontend and a backend application. Your task is to design and configure **complete CI/CD workflows** with the following requirements:

---

### ✅ **Requirements**

1. **Self-Hosted Runners**

   * Configure **two separate self-hosted runners**:

     * One dedicated to building the backend.
     * One dedicated to building the frontend.
   * Ensure each workflow uses the correct runner.

2. **Selective Workflow Triggers**

   * Workflows must **only trigger** when changes occur in their respective directories (`frontend/` or `backend/`).
   * Changes to non-code files (e.g., `README.md`) must **not trigger any builds**.

3. **Build and Deployment**

   * **Backend:**

     * Build the backend application.
     * Upload the build artifacts to GitHub so they can be downloaded later.
   * **Frontend:**

     * Build the frontend application.
     * Deploy the built frontend site to **GitHub Pages** automatically.

4. **Security Scanning**

   * Integrate a **Trivy security scan** for your application or Docker images.
   * Generate an **HTML report** of the scan results.
   * Publish the report in **GitHub Pages** under a separate path such as `/scan`.


Document should be well written.

5. **Bonus (Optional)**

   * In addition to the regular build artifacts:

     * Produce **Docker image exports** of both the frontend and backend applications.
     * Publish the exported Docker images as downloadable artifacts.
   * Update the deployed frontend site to include a **download button or link** that allows users to download these artifacts easily.

---

---


