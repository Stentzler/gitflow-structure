# Git Flow + CI/CD Pipeline

This repository documents a Git branching strategy and a CI/CD pipeline designed for modern backend development workflows.

---

## 📊 Visual Diagram

![Git Flow CI/CD](git-flow-ci-cd-diagram.png)

---

## 📂 Branching Strategy

| Branch       | Purpose                          |
|--------------|----------------------------------|
| `main`       | Production-ready code            |
| `dev`        | Integration of features and fixes |
| `release/*`  | Release preparation branch        |
| `feat/*`     | Feature development               |
| `fix/*`      | Bugfixes in development           |
| `hotfix/*`   | Critical fixes in production      |

---

## 🔁 Pull Request Flow

- `feat/*` and `fix/*` are branched from `dev`
- When work is done:
  - Create PR into `dev`
  - CI runs (tests + SonarQube)
  - If successful → merge into `dev`

- `release/*` is manually created from `dev` when preparing a release
  - PR from `release/*` into `main`
  - CI runs (tests + SonarQube)
  - If successful → merge into `main`

---

## ✅ CI Pipeline

CI is triggered on PRs to `dev` and `main`:

- ✅ Run tests (unit/integration)
- ✅ Run SonarQube analysis
- 🔒 Block PR merge if:
  - Tests fail
  - SonarQube detects critical issues (future enhancement)

---

## 🚀 CD Pipeline

CD is triggered on **merge** to `dev` and `main`:

### On merge to `dev`:
1. Build Docker image (`dev-<short_sha>`)
2. Push to container registry
3. Deploy to EC2 (Development Environment)

### On merge to `main`:
1. Build Docker image (`prod-<version>`)
2. Push to container registry
3. **Tag manually** using Git (`vX.Y.Z`)
4. Deploy to EC2 (Production Environment)

---

## 🏷️ Release & Tagging

- `release/*` is created manually from `dev` once ready
- PR from `release/*` into `main`
- After merge to `main`:
  - A version tag is manually created (`v1.2.0`, etc.)

---

## 🧼 Other Rules

- 🔐 Branch protection on `main` and `dev`
- 🧹 Auto-delete `feat/*` and `fix/*` branches after merge (optional)
- 💬 Commit messages follow [Conventional Commits](https://www.conventionalcommits.org)

---
