# AI Prompts for DevOps Engineers

This document provides AI prompts tailored for DevOps Engineers to assist with infrastructure management, CI/CD pipeline development, deployment automation, and operational support, adhering to the project's SOPs and best practices.

## 1. Infrastructure Management

### 1.1 Designing Infrastructure as Code (IaC)

**Prompt**: "For the `[Application Name]` application, propose an Infrastructure as Code (IaC) design using `[Tool, e.g., Terraform, CloudFormation]` to deploy a `[Service Type, e.g., microservice, web application]` on `[Cloud Provider, e.g., AWS, Azure]`. Include considerations for scalability, high availability, and cost optimization."

### 1.2 Environment Provisioning

**Prompt**: "We need to provision a new `[Environment Type, e.g., staging, QA]` environment for `[Project Name]`. Outline the steps and configurations required, including network setup, compute resources, database instances, and necessary access controls. Reference the [Phase 2: Architecture and Design SOP](../../docs/SOPs/phase_2_architecture_design_sop.md)."

## 2. CI/CD Pipeline Development

### 2.1 Designing CI/CD Pipelines

**Prompt**: "Design a CI/CD pipeline for a `[Language/Framework, e.g., Node.js Express API, Python Django App]` using `[CI/CD Tool, e.g., GitHub Actions, GitLab CI/CD]`. Include stages for code linting, unit testing, integration testing, security scanning, and artifact building. Reference the [Phase 3: Implementation and Testing SOP](../../docs/SOPs/phase_3_implementation_testing_sop.md) and [Git Workflow and Best Practices SOP](../git_sop.md)."

### 2.2 Troubleshooting Pipeline Failures

**Prompt**: "Our CI/CD pipeline for `[Project Name]` is failing at the `[Stage Name, e.g., 'build', 'test', 'deploy']` stage with the following logs: 
```
[CI/CD logs]
```
What are the most common causes for this type of failure at this stage, and what steps should I take to diagnose and resolve the issue?"

## 3. Deployment Automation

### 3.1 Automating Application Deployment

**Prompt**: "Outline a strategy for automating the deployment of `[Application Name]` to `[Environment, e.g., Kubernetes cluster, AWS EC2 instances]` using `[Tool, e.g., Argo CD, Ansible]`. Include steps for blue/green deployment, rollback procedures, and post-deployment verification."

### 3.2 Creating Operational Runbooks

**Prompt**: "Draft an operational runbook for `[Application Name]` covering `[Scenario, e.g., 'application restart', 'database backup', 'log analysis']`. Include step-by-step instructions, expected outcomes, and troubleshooting tips, referencing the [Phase 4: Documentation and Knowledge Transfer SOP](../../docs/SOPs/phase_4_documentation_knowledge_transfer_sop.md)."

## 4. Monitoring & Logging

### 4.1 Setting Up Application Monitoring

**Prompt**: "Propose a monitoring strategy for `[Application Name]` using `[Monitoring Tool, e.g., Prometheus, Datadog]`. Specify key metrics to track (e.g., CPU, memory, request latency, error rates), alert thresholds, and dashboard visualizations."

### 4.2 Centralized Logging

**Prompt**: "We need to implement centralized logging for our microservices. Recommend a solution (e.g., ELK Stack, Splunk) and outline the steps for integrating it with our `[Language/Framework]` applications and `[Cloud Provider]` infrastructure."
