# CatSafetyMonitoring: Development Guide

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini

## 1. Overview

This document provides guidelines for developers working on the Cat Safety & Monitoring component. It covers setting up the development environment, coding standards, and the development workflow.

## 2. Development Environment Setup

- **Prerequisites**: Docker, Docker Compose, Python 3.10+, Node.js 18+
- **Backend Setup**:
    1.  Clone the repository.
    2.  Navigate to the `backend` directory.
    3.  Run `docker-compose up -d` to start the required services (PostgreSQL, Redis, RabbitMQ).
    4.  Create a virtual environment: `python -m venv venv`
    5.  Activate the virtual environment: `source venv/bin/activate`
    6.  Install dependencies: `pip install -r requirements.txt`
    7.  Run database migrations: `alembic upgrade head`
    8.  Start the services: `uvicorn app.main:app --reload`
- **Frontend Setup**:
    1.  Navigate to the `mobile` directory.
    2.  Install dependencies: `npm install`
    3.  Run the app on a simulator or device: `npx react-native run-ios` or `npx react-native run-android`

## 3. Coding Standards

- **General**: All code must adhere to the [Enterprise Coding Style Guide](../../../docs/enterprise_coding_style_guide.md).
- **Python**: Follow the PEP 8 style guide. Use Black for code formatting and Flake8 for linting.
- **TypeScript/JavaScript**: Follow the Airbnb JavaScript Style Guide. Use Prettier for code formatting and ESLint for linting.
- **Requirement Traceability**: All new features and significant changes must be traceable to a requirement. Include `@requirement REQ-ID` in docstrings or comments.

## 4. Development Workflow

- **Branching**: Follow the GitFlow branching model as defined in the [Git SOP](../../../docs/SOPs/git_sop.md).
    - Feature branches should be named `feature/CSM-XXX-description` (where CSM-XXX is the Jira ticket number).
- **Commits**: Use the Conventional Commits specification for commit messages.
- **Pull Requests (PRs)**:
    - A PR must be created for every feature, bugfix, or chore.
    - The PR description must include a summary of the changes and a link to the relevant Jira ticket.
    - All CI checks (linting, unit tests, code coverage) must pass before a PR can be merged.
    - A PR must be reviewed and approved by at least one other developer.
- **Code Reviews**:
    - Reviewers should focus on code quality, correctness, and adherence to standards.
    - Provide constructive feedback and suggestions.

## 5. Testing

- **Unit Tests**: Each microservice must have a comprehensive suite of unit tests with a minimum of 80% code coverage.
- **Integration Tests**: Write integration tests for the interactions between services, especially for the message queue and database.
- **End-to-End (E2E) Tests**: E2E tests will be written to simulate user flows from the mobile app to the backend.
