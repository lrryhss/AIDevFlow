# CatTravelPlanning: Development Guide

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini

## 1. Overview

This document provides guidelines for developers working on the Cat Travel Planning component.

## 2. Development Environment Setup

- **Prerequisites**: Docker, Docker Compose, Python 3.10+, Node.js 18+
- **Backend Setup**:
    1.  Follow the same backend setup steps as for the other components.
    2.  Ensure the `cat_planning` schema migrations are applied: `alembic upgrade head`.
- **Frontend Setup**:
    1.  Follow the same frontend setup steps as for the other components.

## 3. Coding Standards

- **General**: All code must adhere to the [Enterprise Coding Style Guide](../../../docs/enterprise_coding_style_guide.md).
- **Python**: Follow PEP 8, use Black for formatting and Flake8 for linting.
- **Requirement Traceability**: All new features must be traceable to a requirement using `@requirement REQ-ID`.

## 4. Development Workflow

- **Branching**: Follow the GitFlow model. Feature branches should be named `feature/CTP-XXX-description` (where CTP-XXX is the Jira ticket number).
- **Commits**: Use the Conventional Commits specification.
- **Pull Requests (PRs)**:
    - A PR must be created for every change.
    - All CI checks must pass.
    - A PR must be reviewed and approved by at least one other developer.

## 5. Testing

- **Unit Tests**: The Planning Service must have unit tests with a minimum of 80% code coverage.
- **Integration Tests**: Write integration tests for the interactions with the database, cache, and mocked external accommodation APIs.
- **E2E Tests**: E2E tests will be written to simulate the user flow of searching for accommodations and managing a checklist.
