# Enterprise Software Requirements Writing Style Guide

## Document Information
- **Version**: 1.0
- **Purpose**: Standardize requirements writing across all enterprise software development projects
- **Audience**: Business Analysts, Product Managers, Software Architects, Project Teams
- **Scope**: All functional, non-functional, and technical requirements documentation

---

## 1. Fundamental Writing Principles

### 1.1 Clarity and Precision
Requirements must be written with absolute clarity, leaving no room for misinterpretation or assumption.

**Do:**
- Use specific, measurable terms
- Define exact criteria and thresholds
- Provide concrete examples when helpful

**Don't:**
- Use vague qualifiers like "fast," "user-friendly," "robust," or "intuitive"
- Leave room for multiple interpretations
- Assume readers will understand implicit meanings

**Examples:**

❌ **Poor**: The system should be fast and responsive.

✅ **Good**: The system shall respond to user search queries within 2 seconds under normal load conditions (up to 500 concurrent users).

❌ **Poor**: The application must have good security.

✅ **Good**: The application shall implement AES-256 encryption for data at rest and TLS 1.3 for data in transit.

### 1.2 Consistency and Standards
Maintain consistent terminology, structure, and style throughout all requirements documentation.

**Mandatory Standards:**
- Use the same term for the same concept throughout the document
- Maintain consistent sentence structure and formatting
- Apply standardized requirement identifiers
- Follow established templates for each requirement type

---

## 2. Language Conventions

### 2.1 Modal Verbs and Priority Levels
Use specific modal verbs to indicate requirement priority and obligation levels.

**SHALL** - Mandatory requirements that must be implemented
- "The system **shall** validate user credentials before granting access."

**SHOULD** - Recommended requirements that are highly desirable but not mandatory
- "The system **should** provide password strength indicators during registration."

**MAY** - Optional requirements that provide additional value but are not essential
- "The system **may** offer biometric authentication as an alternative login method."

**MUST** - Use only for absolute constraints or regulatory compliance
- "The system **must** comply with GDPR data protection requirements."

**WILL** - Reserved for statements of fact or future conditions
- "The integration **will** be implemented using REST API protocols."

### 2.2 Voice and Tense
**Active Voice**: Use active voice to clearly identify who performs what action.

❌ **Passive**: User data will be encrypted by the system.

✅ **Active**: The system shall encrypt user data.

**Present Tense**: Write requirements in present tense to indicate current system capabilities.

❌ **Future**: The system will authenticate users.

✅ **Present**: The system shall authenticate users.

### 2.3 Prohibited Terms and Phrases
Avoid ambiguous terms that can lead to misinterpretation:

**Vague Qualifiers:**
- ❌ "appropriate," "adequate," "reasonable," "sufficient"
- ✅ Use specific measurements and criteria

**Subjective Terms:**
- ❌ "user-friendly," "intuitive," "simple," "easy"
- ✅ Define specific usability criteria and success metrics

**Imprecise Quantities:**
- ❌ "many," "few," "several," "various"
- ✅ Use exact numbers or defined ranges

**Conditional Language:**
- ❌ "if possible," "as appropriate," "when necessary"
- ✅ Define specific conditions and triggers

---

## 3. Requirement Structure Templates

### 3.1 Standard Requirement Format
**Pattern**: [Actor] shall [Action] [Object] [Conditions] [Constraints]

**Components:**
- **Actor**: Who or what performs the action (user, system, administrator)
- **Action**: What must be done (validate, process, display, store)
- **Object**: What is acted upon (data, interface, report, user account)
- **Conditions**: Under what circumstances (when user logs in, during peak hours)
- **Constraints**: Limitations or specifications (within 3 seconds, using HTTPS)

**Example**: The system shall authenticate user credentials using LDAP protocol within 5 seconds during normal operating conditions.

### 3.2 Functional Requirements Template

```
REQ-[CATEGORY]-[NUMBER]: [Requirement Title]

Description: [Actor] shall [action] [object] [conditions] [constraints].

Acceptance Criteria:
- [Specific testable condition 1]
- [Specific testable condition 2]
- [Specific testable condition 3]

Business Justification: [Why this requirement is needed]

Priority: [SHALL/SHOULD/MAY]

Dependencies: [Related requirements or external dependencies]
```

**Example:**
```
REQ-AUTH-001: User Authentication

Description: The system shall authenticate registered users using username and password credentials within 3 seconds of submission.

Acceptance Criteria:
- Valid credentials grant access to user dashboard
- Invalid credentials display error message within 2 seconds
- Account locks after 3 consecutive failed attempts
- Locked accounts display appropriate error message

Business Justification: Secure access control prevents unauthorized system access and protects user data.

Priority: SHALL

Dependencies: REQ-SEC-001 (Password Policy), REQ-DB-003 (User Data Storage)
```

### 3.3 Non-Functional Requirements Template

```
REQ-[CATEGORY]-[NUMBER]: [Performance/Quality Attribute]

Description: The system shall [perform function] [performance criteria] [measurement conditions].

Metrics:
- [Specific measurable criterion 1]
- [Specific measurable criterion 2]

Test Conditions: [How the requirement will be verified]

Priority: [SHALL/SHOULD/MAY]
```

**Example:**
```
REQ-PERF-001: Response Time Performance

Description: The system shall respond to user requests with average response times under 2 seconds during normal load conditions.

Metrics:
- 95% of requests complete within 2 seconds
- 99% of requests complete within 5 seconds
- Maximum concurrent users: 1,000
- Normal load defined as up to 500 concurrent users

Test Conditions: Performance testing using simulated user load with representative data volumes.

Priority: SHALL
```

### 3.4 Interface Requirements Template

```
REQ-INT-[NUMBER]: [Interface Description]

Description: The system shall [interface action] with [external system] using [protocol/format] [frequency/conditions].

Interface Specifications:
- Protocol: [Technical protocol details]
- Data Format: [Format specifications]
- Frequency: [How often interactions occur]
- Error Handling: [How errors are managed]

Priority: [SHALL/SHOULD/MAY]
```

---

## 4. Specific Writing Guidelines

### 4.1 Numbers and Measurements
**Use Arabic Numerals**: Always use numbers (1, 2, 3) rather than words (one, two, three).

**Include Units**: Always specify units of measurement.
- ✅ "within 500 milliseconds"
- ❌ "within 500"

**Provide Ranges When Appropriate**: Use specific ranges rather than vague quantities.
- ✅ "between 10 and 50 records"
- ❌ "several records"

### 4.2 Technical Specifications
**Be Specific About Technologies**: Name specific protocols, standards, and technologies.
- ✅ "using TLS 1.3 encryption"
- ❌ "using secure encryption"

**Include Version Numbers**: Specify exact versions when relevant.
- ✅ "compatible with Internet Explorer 11 and later"
- ❌ "compatible with modern browsers"

### 4.3 User Interface Requirements
**Describe Observable Behavior**: Focus on what users can see and do.
- ✅ "The login button shall change from blue to green when clicked"
- ❌ "The login button shall provide visual feedback"

**Specify Interaction Patterns**: Define exact user interactions.
- ✅ "Users shall complete checkout within 5 clicks from cart review"
- ❌ "Checkout process shall be streamlined"

---

## 5. Quality Assurance Guidelines

### 5.1 Testability Requirements
Every requirement must be verifiable through testing or inspection.

**Include Acceptance Criteria**: Define how success will be measured.

**Specify Test Conditions**: Describe the conditions under which testing will occur.

**Make Requirements Atomic**: Each requirement should test one specific capability.

### 5.2 Completeness Checklist
Before finalizing requirements, verify:

- [ ] All actors are clearly identified
- [ ] All actions are precisely defined
- [ ] All conditions and constraints are specified
- [ ] Acceptance criteria are included
- [ ] Dependencies are documented
- [ ] Priority levels are assigned
- [ ] Requirements are uniquely identified
- [ ] Terminology is consistent throughout

### 5.3 Review and Validation
**Peer Review Process**: All requirements must undergo peer review by at least one other analyst.

**Stakeholder Validation**: Business stakeholders must approve requirements that affect their processes.

**Technical Review**: Technical leads must validate feasibility of technical requirements.

---

## 6. Documentation Standards

### 6.1 Requirement Identifiers
Use consistent naming conventions for requirement IDs:

**Format**: REQ-[CATEGORY]-[NUMBER]

**Categories:**
- AUTH (Authentication/Authorization)
- UI (User Interface)
- PERF (Performance)
- SEC (Security)
- INT (Integration)
- DATA (Data Management)
- RPT (Reporting)
- API (Application Programming Interface)
- COMP (Compliance)

**Example**: REQ-AUTH-001, REQ-PERF-015, REQ-SEC-003

### 6.2 Traceability Matrix
Maintain bidirectional traceability linking:
- Business objectives to requirements
- Requirements to design specifications
- Requirements to test cases
- Requirements to implementation components

### 6.3 Version Control
**Document Versioning**: Use semantic versioning (Major.Minor.Patch) for requirement documents.

**Change Tracking**: Document all changes with:
- Date of change
- Author of change
- Reason for change
- Impact assessment
- Approval authority

---

## 7. Common Mistakes to Avoid

### 7.1 Implementation Details in Requirements
❌ **Wrong**: The system shall use Oracle database to store user information.

✅ **Right**: The system shall store user information in a relational database with ACID compliance.

### 7.2 Multiple Requirements in One Statement
❌ **Wrong**: The system shall authenticate users and log all access attempts and send email notifications.

✅ **Right**: 
- REQ-AUTH-001: The system shall authenticate users using valid credentials.
- REQ-LOG-001: The system shall log all authentication attempts.
- REQ-NOT-001: The system shall send email notifications for successful logins.

### 7.3 Mixing Functional and Non-Functional Requirements
❌ **Wrong**: The system shall process payments securely and quickly.

✅ **Right**:
- REQ-PAY-001: The system shall process credit card payments using PCI-compliant methods.
- REQ-PERF-001: The system shall complete payment processing within 10 seconds.

---

## 8. Examples by Requirement Type

### 8.1 User Story Format (When Appropriate)
**Template**: As a [user type], I want [functionality] so that [business value].

**Followed by**: Detailed acceptance criteria using the standard requirement format.

**Example**:
```
User Story: As a customer service representative, I want to search customer records by phone number so that I can quickly access customer information during support calls.

REQ-SEARCH-001: The system shall allow customer service representatives to search customer records using phone number as search criteria.

Acceptance Criteria:
- Search returns results within 3 seconds
- Partial phone number matches are supported (minimum 7 digits)
- Search results display customer name, account status, and last contact date
- No customer data displays for invalid phone number formats
```

### 8.2 Data Requirements
```
REQ-DATA-001: Customer Information Storage

Description: The system shall store customer information including contact details, account preferences, and transaction history.

Data Elements:
- First Name (required, 50 characters maximum)
- Last Name (required, 50 characters maximum)
- Email Address (required, valid email format)
- Phone Number (optional, 10-15 digits)
- Account Status (required, enumerated values: Active, Inactive, Suspended)
- Registration Date (required, ISO 8601 format)

Data Integrity Rules:
- Email addresses must be unique across all customer records
- Phone numbers must follow international format standards
- Account status changes must be logged with timestamp and user ID

Priority: SHALL
```

### 8.3 Integration Requirements
```
REQ-INT-001: Payment Gateway Integration

Description: The system shall integrate with PayPal payment gateway using REST API to process customer payments.

Interface Specifications:
- Protocol: HTTPS REST API
- Authentication: OAuth 2.0
- Data Format: JSON
- Timeout: 30 seconds maximum
- Retry Logic: Maximum 3 attempts with exponential backoff

Error Handling:
- Network timeouts shall display user-friendly error message
- Payment failures shall log detailed error information
- System shall gracefully handle gateway maintenance windows

Priority: SHALL
```

---

## 9. Compliance and Regulatory Requirements

### 9.1 Regulatory Language
When writing compliance-related requirements, use precise regulatory language and cite specific regulations.

**Example**:
```
REQ-COMP-001: GDPR Data Subject Rights

Description: The system shall implement GDPR Article 17 "Right to Erasure" allowing EU residents to request deletion of personal data.

Compliance Specifications:
- Data deletion requests must be processed within 30 days
- Confirmation of deletion must be provided to the data subject
- Audit trail of deletion actions must be maintained
- Third-party data processors must be notified of deletion requests

Regulatory Citation: EU General Data Protection Regulation (GDPR) Article 17

Priority: MUST
```

---

## 10. Review and Approval Process

### 10.1 Review Checklist
Before submitting requirements for approval, verify:

**Content Quality:**
- [ ] Requirements follow standard templates
- [ ] Language is clear and unambiguous
- [ ] All requirements are testable
- [ ] Acceptance criteria are defined
- [ ] Dependencies are documented

**Technical Accuracy:**
- [ ] Technical specifications are feasible
- [ ] Integration requirements are realistic
- [ ] Performance criteria are achievable
- [ ] Security requirements are appropriate

**Business Alignment:**
- [ ] Requirements support business objectives
- [ ] Priority levels are appropriate
- [ ] Scope is clearly defined
- [ ] Business value is articulated

### 10.2 Approval Authority
**Functional Requirements**: Business stakeholder approval required
**Technical Requirements**: Technical lead approval required
**Security Requirements**: Security architect approval required
**Compliance Requirements**: Legal/compliance team approval required

---

*This writing style guide ensures consistent, clear, and comprehensive requirements documentation across all enterprise software development projects. Adherence to these standards reduces ambiguity, improves communication, and increases project success rates.*