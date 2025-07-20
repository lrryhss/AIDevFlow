# AI Prompts for Operations Team

This document provides AI prompts tailored for the Operations Team to assist with system deployment, monitoring, maintenance, and incident response, adhering to the project's SOPs and best practices.

## 1. Deployment & Release Management

### 1.1 Executing Deployment Procedures

**Prompt**: "We are deploying `[Application Name]` version `[Version Number]` to the `[Environment, e.g., production, staging]` environment. Based on the deployment runbook (`[path/to/runbook.md]`), outline the step-by-step procedure for this deployment, including pre-deployment checks, execution steps, and post-deployment verification. Reference the [Phase 4: Documentation and Knowledge Transfer SOP](../../docs/SOPs/phase_4_documentation_knowledge_transfer_sop.md)."

### 1.2 Rollback Procedures

**Prompt**: "If the deployment of `[Application Name]` version `[Version Number]` to `[Environment]` fails, what is the rollback procedure? Detail the steps to revert to the previous stable version, minimize downtime, and communicate the rollback status to stakeholders."

## 2. Monitoring & Alerting

### 2.1 Setting Up Monitoring Dashboards

**Prompt**: "For `[Application Name]`, design a monitoring dashboard using `[Monitoring Tool, e.g., Grafana, Datadog]`. Specify key metrics to visualize (e.g., CPU utilization, memory usage, request latency, error rates), and suggest appropriate alert thresholds for critical issues."

### 2.2 Responding to Alerts

**Prompt**: "We received an alert: `[Alert Message/Description]`. Based on our operational runbooks and knowledge base, what is the likely cause of this alert, and what are the immediate steps to investigate and resolve the issue?"

## 3. System Maintenance & Optimization

### 3.1 Performing Routine Maintenance

**Prompt**: "Outline a checklist for routine maintenance tasks for `[System/Database Name]` (e.g., database backups, log rotation, security patching). Specify the frequency for each task and any prerequisites or post-maintenance checks."

### 3.2 Optimizing System Performance

**Prompt**: "Our `[Application Name]` is experiencing `[Performance Issue, e.g., high CPU usage, slow response times]`. What are the common causes for this type of issue, and what diagnostic steps and optimization strategies should we consider?"

## 4. Incident Response

### 4.1 Initial Incident Triage

**Prompt**: "An incident has been reported: `[Incident Description]`. What are the immediate steps for initial triage, including verifying the incident, assessing its impact, and escalating to the appropriate teams?"

### 4.2 Post-Incident Analysis

**Prompt**: "After resolving `[Incident ID/Description]`, conduct a post-incident analysis. Identify the root cause, contributing factors, and recommend preventative measures to avoid similar incidents in the future. Focus on lessons learned and process improvements."
