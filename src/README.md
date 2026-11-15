# Notes Application — Node.js, Express & MongoDB

A lightweight, production-ready web application demonstrating test-driven architecture, containerized integration testing, and automated multi-browser End-to-End (E2E) testing.

## Tech Stack & Core Libraries

- **Runtime:** Node.js (>= 18.x) & Express.js 4.x
- **Persistence:** MongoDB 6.x via Mongoose 6.x ORM
- **Template Engine:** EJS (Embedded JavaScript Templates)
- **Code Quality & Linting:** ESLint 8.x (Standard config)
- **Unit & Integration Testing:** Jest 27.x, Supertest 6.x, Mockingoose 2.x
- **End-to-End (E2E) Testing:** Playwright 1.45+ (Chromium, Firefox, WebKit)
- **CI/CD:** GitHub Actions with containerized MongoDB services

---

## Application Features

- **Note Management:** Create, list, view, and delete rich text notes.
- **Data Validation:** Strict schema enforcement on note title, body, and timestamp attributes.
- **RESTful Routing:** Express HTTP handlers with REST conventions and method-override support.
- **Resilient Connectivity:** Configurable database connection strings via environment variables (`.env`).

---

## Local Development & Setup

### 1. Prerequisites
Ensure you have the following installed:
- Node.js (v18.x or v20.x recommended)
- MongoDB running locally on `localhost:27017` or via Docker:
  ```bash
  docker run -d -p 27017:27017 --name mongodb mongo:6.0
  ```

### 2. Dependency Installation
Install project dependencies from the repository root:
```bash
npm install --prefix src
```

### 3. Environment Configuration
An example `.env` file is located in `src/.env`:
```ini
PORT=3000
SERVER=mongodb://127.0.0.1:27017
```

### 4. Running the Application
Start the Node.js server:
```bash
npm run start --prefix src
```
Access the application in your browser at `http://localhost:3000`.

---

## Automated Test Suites

The repository implements a comprehensive test pyramid to guarantee software reliability across layers:

### 1. Static Code Analysis (ESLint)
Enforces syntax consistency and detects unused dependencies or unsafe operations:
```bash
npm run test:lint --prefix src
```

### 2. Unit Testing & Model Coverage (Jest)
Validates Mongoose schema constraints, model validation rules, and business logic:
```bash
npm run test:unit --prefix src
```

### 3. Integration Testing (Supertest + MongoDB)
Tests Express route handlers, database queries, and HTTP status codes against a live database:
```bash
npm run test:integration --prefix src
```

### 4. End-to-End (E2E) Browser Testing (Playwright)
Simulates realistic user journeys (creating and deleting notes) across all major browser engines (Chromium, Firefox, and WebKit):
```bash
# First-time setup: install browser binaries
npx playwright install --with-deps

# Run end-to-end test suite
npm run test:e2e --prefix src
```

---

## Continuous Integration Pipeline

The automated CI workflow (`.github/workflows/ci-pipeline.yml`) executes on every push and pull request:
1. Provisions an Ubuntu 22.04 LTS virtual runner with a containerized MongoDB 6.0 service.
2. Runs ESLint static analysis.
3. Executes Jest unit and integration tests with coverage assertion.
4. Boots the local server and runs Playwright across Chromium, Firefox, and WebKit.
5. Packages a deployable tarball (`deployable-artifact.tar.gz`) on green builds merged to `main`.
