# NTU CSM vs Monash Placement Management System
## Functional & Technical Fit Assessment

**Source Documents**

- NTU Career Services Management (CSM) Functional Specifications :contentReference[oaicite:0]{index=0}
- Monash University Part D2 Functional Specifications :contentReference[oaicite:1]{index=1}

---

# Executive Summary

Overall, the existing **NTU Career Services Management (CSM)** platform provides a strong foundation for the proposed **Monash Placement Management System (PMS)**.

However, Monash's requirements extend beyond a traditional career services platform. The Monash solution is designed as an enterprise-wide **Placement Management Platform** with significantly stronger emphasis on:

- compliance governance
- configurable workflow orchestration
- enterprise integrations
- eligibility decisioning
- auditability
- planning and forecasting

Rather than replacing the existing platform, the recommended approach is to evolve NTU CSM into a broader placement management platform.

| Criteria | Score | Assessment |
|-----------|------:|------------|
| Functional Fit | **72%** | Strong overlap across core placement capabilities. Major gaps exist around compliance, eligibility engine, planning, and governance. |
| Technical Fit | **85%** | Existing architecture is modern and reusable. Main work involves replacing university-specific integrations. |
| Reusability | **82%** | Majority of platform services can be reused with extension rather than replacement. |
| Delivery Effort | **Medium-High** | Approximately half of the solution can leverage existing implementation. |
| Delivery Risk | **Medium** | Primary risks are architectural and integration-related rather than technology-related. |

---

# 1. Functional Fit (72%)

## Excellent Fit (90–100%)

These capabilities already exist within NTU CSM and map closely to Monash requirements.

| Monash Capability | NTU CSM Equivalent | Fit |
|-------------------|-------------------|-----|
| Partner Management | Employer Management | High |
| Student Portal | Student Portal | High |
| Placement Opportunities | Jobs & Internships | High |
| Application Management | Internship Applications | High |
| Interview Scheduling | Coaching & Appointment Module | High |
| Notifications | Communications Module | High |
| Forms | Dynamic Forms | High |
| Reporting | Dashboards & Reports | High |
| Role-based Access | Workgroups & RBAC | High |
| Audit Logging | Audit Trails | High |

### Supporting Evidence

NTU CSM already supports:

- employer registration and approval
- placement workflows
- internship lifecycle
- application management
- communications
- reporting
- role-based access
- audit trails

These align closely with Monash's Partner Management, Placement Management, Reporting and Workflow capabilities. :contentReference[oaicite:2]{index=2} :contentReference[oaicite:3]{index=3}

---

# Good Fit (70–90%)

## Partner Management

### Existing NTU Capability

- Employer onboarding
- Company profile management
- Approval workflow
- Contact management
- Blacklist / Watchlist
- Employer dashboards

### Monash Requirement

Monash extends this to include:

- Hierarchical organisations
- Relationship ownership
- Agreement lifecycle
- Partner assessments
- Partner scoring
- Relationship Manager assignment

### Assessment

**Fit: ~70%**

The Employer module can be extended into a broader Partner Management module.

---

## Placement Lifecycle

### Existing NTU Capability

- Internship lifecycle
- Placement approvals
- Offer generation
- Faculty supervision
- Assessments
- Journals

### Monash Requirement

Monash expects:

- Generic placement lifecycle
- Faculty-configurable workflows
- Reusable workflow engine
- Planning capability
- Financial tracking

### Assessment

**Fit: ~75%**

Workflow concepts already exist but require abstraction into a reusable platform service.

---

## Communications

NTU CSM already provides:

- email templates
- scheduling
- campaigns
- segmentation
- reminders
- communication preferences

Monash formalises this into an enterprise-wide notification framework.

### Assessment

**Fit: ~90%**

Minimal enhancement required.

---

# Low Fit Areas (30–60%)

These represent the largest functional gaps.

---

## 1. Compliance Evaluation Service (Largest Gap)

### Monash Requirement

Monash introduces a dedicated Compliance Evaluation & Storage Service (CESS) responsible for:

- compliance document repository
- compliance verification
- expiry calculation
- rule evaluation
- version history
- auditability
- reproducible compliance decisions

Examples include:

- Working With Children Check
- Police Check
- Immunisations

Compliance status becomes a shared service consumed across every placement process. :contentReference[oaicite:4]{index=4}

### NTU Capability

NTU currently stores forms and uploaded documents but does **not** provide a centralised compliance evaluation service. :contentReference[oaicite:5]{index=5}

### Assessment

**Fit: ~20%**

This will require a completely new platform module.

---

## 2. Eligibility & Rules Engine

### Monash Requirement

A single rules engine drives:

- applications
- interviews
- matching
- offers
- allocations
- reporting

Rules evaluate:

- academic eligibility
- compliance
- capacity
- travel constraints
- exceptions
- exclusions

### NTU Capability

Eligibility checks exist within individual workflows but are not implemented as a reusable rules engine.

### Assessment

**Fit: ~35%**

Requires architectural redesign.

---

## 3. Configurable Workflow Engine

### NTU

Workflow logic is embedded within modules.

Example:

- Internship Approval
- Employer Approval
- Coaching Approval

### Monash

Requires a generic workflow engine capable of driving:

- Partner Approval
- Compliance Approval
- Placement Approval
- Incident Management
- Agreement Approval

without vendor customisation.

### Assessment

**Fit: ~50%**

Existing workflows provide a good foundation but require platform abstraction.

---

## 4. Incident Management

Monash requires:

- incident recording
- safety escalation
- configurable workflows
- severity classification
- investigation history

NTU CSM currently has no equivalent module.

### Assessment

**Fit: ~10%**

Entirely new capability.

---

## 5. Placement Planning

Monash introduces strategic planning capability:

- placement demand forecasting
- partner capacity planning
- allocation planning

NTU is primarily operational.

### Assessment

**Fit: ~15%**

New capability required.

---

## 6. Financial Tracking

Monash requires:

- placement costs
- invoices
- budgets
- forecasts

NTU currently contains no financial management capability.

### Assessment

**Fit: ~5%**

New capability required.

---

# Functional Gap Summary

| Capability | Estimated Fit | Recommendation |
|------------|--------------:|----------------|
| Partner Management | 70% | Extend existing Employer module |
| Student Management | 85% | Extend profile model |
| Placement Opportunities | 90% | Reuse |
| Applications | 90% | Reuse |
| Interview Management | 85% | Extend |
| Communications | 90% | Reuse |
| Reporting | 85% | Reuse |
| Workflow Engine | 50% | Refactor |
| Compliance Service | 20% | New module |
| Eligibility Engine | 35% | New platform service |
| Incident Management | 10% | New module |
| Planning | 15% | New module |
| Financial Tracking | 5% | New module |

---

# 2. Technical Fit (85%)

## Existing Architecture

NTU CSM already uses a modern technology stack:

- React
- NestJS
- PostgreSQL
- AWS
- Cognito
- S3
- SES
- CloudWatch

:contentReference[oaicite:6]{index=6}

This architecture is well suited to support the Monash platform.

---

## Integration Changes

The primary technical work involves replacing NTU-specific integrations.

### NTU Integrations

- NTU SSO
- NTU SIS
- Outlook
- Teams
- VMock
- ACRA

### Monash Integrations

- Okta IAM
- Callista / Banner
- uniCRM
- MuleSoft
- DocuSign
- Enterprise Finance
- Compliance Services

:contentReference[oaicite:7]{index=7} :contentReference[oaicite:8]{index=8}

### Assessment

The architecture is reusable.

The integrations change.

---

# 3. Reusability (82%)

## Highly Reusable (~100%)

- Authentication abstraction
- RBAC
- Forms engine
- Communications
- Notifications
- Email templates
- Reporting framework
- Dashboard framework
- Audit logging
- File management
- Calendar integration

---

## Moderately Reusable (70–80%)

- Employer Management
- Student Portal
- Internship Management
- Assessment Framework
- Placement Applications

These require enhancement rather than replacement.

---

## Limited Reuse (30–50%)

- Approval workflows
- Placement lifecycle
- Eligibility logic

These require architectural refactoring.

---

## New Development Required (0–20%)

- Compliance Evaluation Service
- Eligibility Rules Engine
- Incident Management
- Planning Module
- Financial Tracking

---

# 4. Delivery Effort

Estimated implementation effort:

| Work Category | Estimated Contribution |
|--------------|-----------------------:|
| Reuse Existing Modules | 45% |
| Extend Existing Modules | 30% |
| New Platform Capabilities | 25% |

Overall effort is **Medium to High**, with significant value gained from reuse of existing platform components.

---

# 5. Delivery Risk

## Low Risk

- Student Portal
- Employer Portal
- Communications
- Forms
- Reporting
- Authentication
- RBAC

These capabilities are already mature within NTU CSM.

---

## Medium Risk

- Monash integrations
- Workflow abstraction
- Identity migration
- Data migration

---

## High Risk

### Compliance Evaluation Service

This is the single largest architectural gap.

It impacts:

- eligibility
- placement approvals
- reporting
- audit
- notifications

---

### Eligibility Engine

Every major workflow depends on a centralised eligibility engine.

Design decisions made here affect:

- matching
- offers
- placements
- reporting

---

### Enterprise Integration

Monash requires all integrations to flow through enterprise middleware (e.g. MuleSoft), introducing governance and architectural constraints that differ from NTU's current implementation.

---

# Overall Recommendation

The recommended bid position is to present the solution as an **evolution of an existing, proven platform** rather than a completely new implementation.

### Overall Assessment

| Category | Score |
|----------|------:|
| Functional Fit | **72%** |
| Technical Fit | **85%** |
| Reusability | **82%** |
| Delivery Effort | **Medium–High** |
| Delivery Risk | **Medium** |

### Key Strengths

- Mature internship and placement workflows
- Proven employer management capability
- Strong communications framework
- Flexible forms engine
- Comprehensive reporting
- Modern cloud-native architecture
- Mature RBAC and audit capabilities

### Primary Gaps

1. Compliance Evaluation & Storage Service (CESS)
2. Central Eligibility & Rules Engine
3. Configurable Workflow Engine
4. Incident Management
5. Placement Planning
6. Financial Tracking
7. Enterprise Integration (Monash ecosystem)

### Recommendation

A **70–75% functional fit** is a realistic and defensible position for the tender. It accurately reflects the significant reuse available from the NTU CSM platform while acknowledging the strategic platform enhancements required to meet Monash's enterprise placement management objectives.