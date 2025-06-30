# 📘 GitHub Actions Keywords – Full Examples

This guide includes **complete YAML examples** for all major GitHub Actions keywords. Copy and paste directly into `.github/workflows/your-workflow.yml`.



DAY 1
---

## 🟢 1. `name`

```yaml
name: CI Pipeline

on: [push]

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

---

Let me know if you want this as a downloadable `.md` file or expanded into a cheat sheet PDF!
