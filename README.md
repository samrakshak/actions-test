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
##DAY 2








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


