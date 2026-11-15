# Automated CI Pipeline & Playwright E2E Test Suite (GitHub Actions, Node.js, Jest & MongoDB)

**Author:** Egor Zvyagin | [github.com/egor-zvyagin](https://github.com/egor-zvyagin)  
**Stack:** Node.js 18, Express, MongoDB, GitHub Actions, Jest, Supertest, Playwright, ESLint  
**Architecture:** Continuous Integration Pipeline with Containerized Database Services & Automated Multi-Stage Testing  

---

## 1. Executive Summary & Purpose

This repository implements a production-grade Continuous Integration (CI) pipeline for a full-stack Node.js and Express RESTful application backed by MongoDB. Engineered to prevent regression, enforce strict code formatting, and automate quality assurance across pull requests and branch merges, the CI workflow incorporates:

1. **Static Analysis & Linting:** ESLint verification enforcing coding conventions and error prevention.
2. **Unit Testing & Code Coverage:** Isolated unit tests via Jest with high statement/branch coverage metrics.
3. **Integration Testing with Service Containers:** Automated provisioning of a live MongoDB service container in GitHub Actions to test data persistence and schema validations.
4. **End-to-End (E2E) Browser Automation:** Headless browser automation using Playwright testing real user interactions, CRUD workflows, and DOM mutations across Chromium/WebKit/Firefox engines.
5. **Artifact Packaging & Diagnostics:** Automated artifact collection, preserving test execution reports and Playwright trace bundles upon failure.

---

## 2. Pipeline Architecture & Workflow Stages

```text
+-----------------------------------------------------------------------------------+
|                        GitHub Actions Workflow: ci-pipeline.yml                   |
+-----------------------------------------------------------------------------------+
                                          |
                                          v
                         +----------------------------------+
                         |  Checkout Code & Node.js 18 Setup|
                         +----------------------------------+
                                          |
                                          v
                         +----------------------------------+
                         |  npm ci (Deterministic Dependency|
                         |         Installation)            |
                         +----------------------------------+
                                          |
                                          v
                         +----------------------------------+
                         |  ESLint Static Code Analysis     |
                         |  (npm run test:lint)             |
                         +----------------------------------+
                                          |
                                          v
                         +----------------------------------+
                         |  Unit Testing (Jest)             |
                         |  (npm run test:unit)             |
                         +----------------------------------+
                                          |
                                          v
                         +----------------------------------+
                         |  MongoDB Service Container       |
                         |  Integration Tests (Supertest)   |
                         |  (npm run test:integration)      |
                         +----------------------------------+
                                          |
                                          v
                         +----------------------------------+
                         |  Playwright E2E Browser Suite    |
                         |  (npm run test:e2e)              |
                         +----------------------------------+
                                          |
                                          v
                         +----------------------------------+
                         |  Production Package Archival     |
                         |  (npm run package)               |
                         +----------------------------------+
```

---

## 3. Test Suite Breakdown

### Static Analysis & Linting
* **Linter:** ESLint with standard rules configured in `.eslintrc.yml`.
* **Execution:** Validates syntax, enforces ES6+ conventions, catches unhandled promises, and prevents dead code.

### Unit Testing
* **Framework:** Jest.
* **Scope:** Business logic models, data sanitization, date formatting, and route handlers.

### Integration Testing (Database Tier)
* **Framework:** Jest + Supertest.
* **Service:** GitHub Actions containerized MongoDB service (`mongo:latest`) mapped to localhost `27017`.
* **Scope:** CRUD endpoints (`POST /api/notes`, `GET /api/notes`, `PUT /api/notes/:id`, `DELETE /api/notes/:id`), validating database write persistence and connection lifecycle.

### End-to-End (E2E) Testing
* **Engine:** Playwright.
* **Execution:** Spawns application server, launches headless Chromium, fills forms, clicks interaction controls, and asserts UI updates dynamically.

---

## 4. Local Development & Testing Instructions

### Prerequisites
* Node.js 18 LTS
* Local MongoDB instance or Docker daemon

### Installation
```bash
cd src
npm install
```

### Running Test Suites Locally
```bash
# Run linting
npm run test:lint

# Run unit tests
npm run test:unit

# Run integration tests (requires local MongoDB on 27017)
npm run test:integration

# Install Playwright browser binaries
npx playwright install --with-deps

# Run end-to-end browser tests
npm run test:e2e
```

### Production Build / Package
```bash
npm run package
```

---

## 5. Security & Best Practices

* **Zero Hardcoded Secrets:** Connection strings and environment configurations are injected dynamically through environment variables (`SERVER=mongodb://...`).
* **Deterministic Dependencies:** GitHub Actions pipeline uses `npm ci` with `package-lock.json` caching to eliminate version drift.
* **Failure Forensics:** Trace files, screenshots, and Playwright video captures are automatically saved as pipeline artifacts on test failure.
