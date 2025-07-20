# AI Prompts for Data Architects

This document provides AI prompts tailored for Data Architects to assist with data modeling, database design, data governance, and ensuring data integrity and security, adhering to the project's SOPs and best practices.

## 1. Data Modeling & Database Design

### 1.1 Designing Conceptual Data Models

**Prompt**: "Given the user stories and requirements for `[Feature/Module Name]` (e.g., `[path/to/user_stories.md]`, `[path/to/requirements.xml]`), propose a conceptual data model. Identify key entities, their attributes, and relationships. Focus on business concepts rather than technical implementation details."

### 1.2 Designing Logical Data Models

**Prompt**: "Based on the conceptual data model for `[Feature/Module Name]`, design a logical data model. Include all entities, attributes with data types, primary keys, foreign keys, and normalization levels. Consider the [Design Principles](../../1_principles/1.3_design_principles.md) and [Phase 2: Architecture and Design SOP](../../docs/SOPs/phase_2_architecture_design_sop.md)."

### 1.3 Designing Physical Database Schemas

**Prompt**: "Translate the logical data model for `[Feature/Module Name]` into a physical database schema for `[Database Technology, e.g., PostgreSQL, MongoDB]`. Include DDL (Data Definition Language) scripts for table creation, indexing strategies, and partitioning considerations for performance and scalability. Reference the [Implementation Principles](../../1_principles/1.4_implementation_principles.md)."

## 2. Data Governance & Quality

### 2.1 Defining Data Governance Policies

**Prompt**: "For `[Data Type, e.g., Customer Personal Data, Transactional Data]`, propose data governance policies covering data ownership, data quality standards, data retention, and data access controls. Reference the [Enterprise Security Best Practices](../../docs/enterprise_security_best_practices.md) and relevant compliance standards (e.g., GDPR, HIPAA)."

### 2.2 Ensuring Data Integrity

**Prompt**: "What strategies can we implement to ensure data integrity for `[Data Entity, e.g., User Accounts, Product Inventory]`? Consider database constraints, validation rules, and transaction management. Provide code examples for `[Language/Framework]` if applicable."

## 3. Data Migration & Integration

### 3.1 Planning Data Migrations

**Prompt**: "Outline a data migration strategy from `[Source System/Database]` to `[Target System/Database]` for `[Data Set, e.g., existing user data]`. Include steps for data extraction, transformation, loading (ETL), validation, and rollback procedures. Reference the [Phase 4: Documentation and Knowledge Transfer SOP](../../docs/SOPs/phase_4_documentation_knowledge_transfer_sop.md)."

### 3.2 Designing Data Integration Points

**Prompt**: "We need to integrate `[System A]` with `[System B]` to exchange `[Data Type, e.g., order information]`. Propose a data integration strategy, including data formats, protocols (e.g., REST API, Message Queue), and error handling mechanisms. Reference the [Implementation Principles](../../1_principles/1.4_implementation_principles.md)."

## 4. Performance & Scalability

### 4.1 Optimizing Database Performance

**Prompt**: "Analyze the following SQL query: 
```sql
[SQL Query]
```
Identify potential performance bottlenecks and suggest optimizations (e.g., indexing, query rewriting, denormalization)."

### 4.2 Scaling Database Infrastructure

**Prompt**: "Propose strategies for scaling our `[Database Technology]` database to handle `[Target Load, e.g., 10x current traffic, 1TB data]`. Consider options like sharding, replication, and caching, and discuss their trade-offs."
