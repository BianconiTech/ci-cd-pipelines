# GitHub Actions – Workflow Types Overview

GitHub Actions provides a flexible and powerful automation framework. This document explains the **different types of workflows** you can create, when to use them, and how they behave.

---

## 🔁 1. Reusable Workflows (`workflow_call`)

**Definition:** A full workflow that can be called by other workflows in the same or different repositories.

**Use Case:** Centralize pipeline logic (e.g., Terraform plan, security scan).

```yaml
on:
  workflow_call:
    inputs:
      env:
        required: true
        type: string
```

**Called by:**

```yaml
jobs:
  example:
    uses: org/repo/.github/workflows/reusable.yml@main
    with:
      env: production
```

---

## 🧩 2. Composite Actions

**Definition:** Reusable blocks of steps packaged as an action.

**Use Case:** Modularize logic like `terraform fmt`, `helm lint`, `docker login`.

**Directory:** `.github/actions/my-action/action.yml`

```yaml
runs:
  using: "composite"
  steps:
    - run: echo "Running something reusable"
```

**Used in workflow:**

```yaml
uses: ./.github/actions/my-action
```

---

## 🚀 3. Standard Workflows

**Definition:** Regular workflows triggered by events like `push`, `pull_request`, `schedule`, etc.

```yaml
on:
  push:
    branches: [main]
  pull_request:
  schedule:
    - cron: "0 0 * * *"
```

**Use Case:** Continuous integration/deployment, PR checks, scheduled jobs.

---

## 👇 4. Manual Trigger (`workflow_dispatch`)

**Definition:** Manually triggered by user via GitHub UI.

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment'
        required: true
        type: choice
        options:
          - dev
          - staging
          - prod
```

---

## 📦 When to Use Each

| Type             | Best For                               | Reusable | Executed directly? |
|------------------|-----------------------------------------|----------|---------------------|
| `workflow_call`  | Cross-repo orchestration                | ✅ Yes   | ❌ No              |
| Composite Action | Step reuse (within same repo or others) | ✅ Yes   | ❌ No              |
| Standard Workflow| Regular CI/CD                           | ❌ No    | ✅ Yes             |
| `workflow_dispatch` | Manual approvals, on-demand jobs    | ❌ No    | ✅ Yes             |

---

## 🧠 Recommendation

- Use **`workflow_call`** for building a reusable CI/CD toolkit across repos.
- Use **Composite Actions** to break logic into clean, testable blocks.
- Use **Standard Workflows** for main automation flows.
- Use **`workflow_dispatch`** for controlled/manual executions.

---

Maintained by [BianconiTech](https://github.com/BianconiTech)
