# Monash University — Part D2 Functional Specifications

**Source:** `Monash - Part D2 - Technical and Business Specifications Response Schedules - Final.xlsx`
**Domain:** Student Placement Management Platform
**Priority:** CORE = critical operational fit; PREFERRED = enhances efficiency / governance / UX / integration

## Instructions (Part D2)

1. Complete Vendor Response (Complies / Partially Complies / Does Not Comply), Function Availability, and Elaboration for each item.
2. Blank fields score zero.
3. Anonymous evaluation — avoid logos, product names, and personnel names.
4. Specs express capabilities supporting both faculty-configurable workflows and standardised high-volume compliance-driven placement operations on one platform.

**Total requirements:** 92 (CORE: 78, PREFERRED: 13, UNSPECIFIED: 1)

## Partner Management

- **PM-01** [CORE] — *Partner*: The system supports partner authentication using Monash-supported SSO mechanisms.
  - *Rules:* Guest access permitted for registration only.

- **PM-02** [CORE] — *Partner*: The system allows partners to create and maintain a Partner Profile that captures required organisational details, including support for hierarchical organisational structures.
  - *Rules:* Supports hierarchical partner structures with attribute inheritance across organisational levels. Changes to critical Partner Profile attributes must support configurable review and approval workflows. Critical attributes may include (but are not limited to): Primary contact details Organisation address Accreditation status Insurance or compliance-related information Approval workflows must be con…

- **PM-03** [PREFERRED] — *Monash Admin*: The system supports ABN lookup and pre-population of business details via external services.
  - *Rules:* Subject to API availability.

- **PM-04** [CORE] — *Monash Admin*: The system supports upload and storage of supporting documents with versioning and auditability.
  - *Rules:* Versioned and auditable. Aligned with audit (RWA-05) and retention (NFR-06) frameworks

- **PM-05** [CORE] — *Monash Admin
Monash Faculty
Partner*: The system supports configurable routing of partner assessment activities based on defined criteria, executed via the workflow engine (RWA-03).
  - *Rules:* Routing rules are configurable and executed via the workflow engine (RWA-03).

- **PM-06** [CORE] — *Monash Admin*: The system supports tracking of partner application status through a defined lifecycle.
  - *Rules:* Partner status must support configurable lifecycle states, including but not limited to: Submitted Under Review Approved Approved with Conditions Rejected Inactive Restricted / High Risk Banned / Prohibited Status definitions and usage must be configurable at faculty and/or university level. Status must influence system behaviour, including: Visibility of partner records Eligibility for placement …

- **PM-07** [PREFERRED] — *Monash Admin*: The system supports configurable assessment criteria and scoring models.
  - *Rules:* Faculty-specific where required.

- **PM-08** [CORE] — *Monash Admin
Partner*: The system triggers partner assessment outcome events consumable by the Communications & Notifications Framework (CN-01).
  - *Rules:* Notification delivery is managed via CN-01 and is configurable by role, trigger, and channel.

- **PM-09** [CORE] — *Monash Admin
Monash Faculty*: The system supports generation, execution, and lifecycle management of placement agreements.
  - *Rules:* Agreement artefacts are stored and managed via the document management capability (PM-04).

- **PM-10** [CORE] — *All Users*: The system supports version control and configurable expiry alerts for agreements.
  - *Rules:* 30/60/90-day alerts are configurable.

- **PM-11** [PREFERRED] — *Monash Admin
Monash Faculty*: The system should support flexible placement agreement structures, including the ability to manage multiple students under a single partner agreement where appropriate.
  - *Rules:* Must support both: Individual agreements per placement (student-level), and Master agreement with associated schedules or addenda (multi-student model)

- **PM-12** [CORE] — *Monash Admin
Monash Faculty*: The system allows a Monash University staff member to be assigned as the Primary Relationship Manager for each partner organisation.
  - *Rules:* One active Primary Relationship Manager per partner (additional secondary contacts permitted). Assignment must be auditable and historically tracked

- **PM-13** [PREFERRED] — *All Users*: The system provides visibility of the assigned Primary Relationship Manager across partner records and interactions.
  - *Rules:* Visibility governed by RBAC.

- **PM-14** [PREFERRED] — *Monash Admin
Monash Faculty*: The system should log and associate communications, activities, and placements with the assigned Primary Relationship Manager.
  - *Rules:* Integrated with the communications framework and CRM where applicable.

- **PM-15** [CORE] — *Monash Admin
Monash Faculty*: The system supports configurable visibility and access to partner records, including faculty-scoped access, relationship ownership controls, and attribute-level data restrictions.
  - *Rules:* Access to partner records is governed by role-based access control (RWA-01) and aligns to least-privilege access principles (NFR-03). Partner records exist as shared institutional entities; however, visibility is configurable based on: Faculty ownership or association Assigned Primary Relationship Manager (PM-12) User role and permissions The system supports tiered visibility, including: Discovery…

## Student Management

- **SM-01** [CORE] — *Monash Admin
Monash Student*: The system sources student data from the Student Management System.
  - *Rules:* Read-only core attributes. The system must support ingestion and synchronisation of additional student attributes required for placement operations, including but not limited to: Residential address Preferred name Pronouns Course-specific attributes (e.g. teaching methods, specialisations) Attribute sets must be extensible to support faculty-specific requirements.

- **SM-02** [CORE] — *Monash Admin
Monash Faculty*: The system supports the creation and management of pre-enrolled student records prior to official enrolment, with reconciliation to authoritative student data sources when enrolment is confirmed.
  - *Rules:* The system must allow authorised users to create provisional student records where official enrolment data is not yet available. Pre-enrolled records must be clearly identified as provisional. The system must support reconciliation of pre-enrolled records with authoritative student data from the Student Management System (e.g. Callista) once enrolment occurs. Reconciliation must: Match records usi…

- **SM-03** [CORE] — *Monash Student*: The system enforces completion of a standardised student profile prior to participation.
  - *Rules:* Profile completeness enforced. The system must support configurable extension of student profile attributes to accommodate faculty-specific requirements. This includes the ability to: Define new attributes without vendor customisation Support different attribute types (e.g. text, numeric, date, Boolean, controlled lists) Apply attributes selectively by faculty, course, or placement type Attribute …

- **SM-04** [CORE] — *Monash Student*: The system captures and maintains student-submitted compliance artefacts and work history.
  - *Rules:* Compliance artefacts are stored as structured records and are available for downstream evaluation.

- **SM-05** [CORE] — *Monash Admin*: The system supports authorised Monash University administrators to upload, verify, approve, reject, and manage student compliance certifications (e.g., Working with Children Check, Police Check, immunisations).
  - *Rules:* Compliance artefacts may be uploaded and managed by students and authorised administrators. Compliance artefacts are stored as structured records with defined attributes (type, document, status, expiry date, verification outcome). Administrative capabilities support bulk operations (e.g. upload, update, controlled removal) in accordance with governance rules. Compliance verification actions (e.g. …

- **SM-06** [CORE] — *Monash Admin
Monash Faculty*: The system supports configurable grouping of students into cohorts for the purpose of managing placement-specific rules, workflows, and allocations.
  - *Rules:* The system supports configurable cohort/group definitions based on enrolment, pre-enrolment, student attributes, and manual assignment. Groups support both dynamic (rule-based) and static (manual) membership models. Groups are reusable across placement operations, including eligibility (ER), allocation, workflows, and communications. Group definitions and membership changes are auditable and versi…

## Placement Opportunity Management

- **PL-01** [CORE] — *Monash Admin
Monash Faculty
Partner*: The system supports the creation and management of placement opportunities as structured records.
  - *Rules:* Draft > Review > Approved > Published.

- **PL-02** [CORE] — *Monash Admin
Monash Student*: The system applies eligibility outcomes derived from the Eligibility & Rules Engine (ER-01).
  - *Rules:* Academic, compliance, and capacity rules.

- **PL-03** [CORE] — *Monash Admin
Monash Faculty
Partner*: The system supports reusing information contained in historical placement records.
  - *Rules:* Can copy user-entered data only System data (dates, calendars, allocation groups) cannot be copied

## Matching Applications & Selection

- **MA-01** [CORE] — *Monash Admin*: The system invokes eligibility decisions from the Eligibility & Rules Engine (ER-01).
  - *Rules:* Transparent, auditable rules.

- **MA-02** [CORE] — *Monash Admin
Monash Faculty*: The system supports governed manual overrides of eligibility decisions.
  - *Rules:* Overrides logged and governed.

- **MA-03** [CORE] — *Monash Student*: The system prevents ineligible students from submitting placement applications through real-time validation.
  - *Rules:* Real-time validation.

- **MA-04** [CORE] — *All Users*: The system supports configurable application forms, including multimedia responses.
  - *Rules:* Accessibility-compliant.

## Interviews & Assessment

- **IN-01** [CORE] — *All Users*: The system supports configurable interview scheduling with calendar integration.
  - *Rules:* Time zone differences are automatically detected and normalised across all participants (students, partners, staff). Interview availability is displayed in the user’s local time zone while maintaining a consistent system reference time. Calendar integrations (e.g. Outlook, Google) respect participant time zones when creating and updating events. Daylight saving changes are handled automatically to…

- **IN-02** [CORE] — *All Users*: The system supports structured interview feedback with audit trails.
  - *Rules:* Feedback is captured using configurable structured criteria (scores + comments). Supports panel-based assessments with independently attributable inputs. Assessment templates are configurable, reusable, and version-controlled. The system supports aggregation of feedback (e.g. scoring, ranking, comparison). All feedback is timestamped, attributable, and immutable, with changes recorded as new entri…

## Offers, Allocation & On-placement

- **OA-01** [CORE] — *All Users*: The system must manage placement offers, acceptances, and reallocations, informed by eligibility and matching outcomes derived from the Eligibility & Rules Engine (ER).
  - *Rules:* Capacity updated in real time.

- **OA-02** [CORE] — *All Users*: The system prevents conflicting or duplicate allocations.
  - *Rules:* Teaching period enforced.

- **OA-03** [CORE] — *All Users*: The system on-placement monitoring and incident reporting are aligned to Monash University's Incident Management Framework.
  - *Rules:* Includes safety escalation. The system supports on-placement monitoring and incident reporting aligned to the Incident Management capability (IM-01).

## Post-Placement & Feedback

- **PP-01** [CORE] — *Monash Students
Partners*: The system supports post-placement surveys for students and partners.
  - *Rules:* Moderation required before publication.

- **PP-02** [CORE] — *All Users*: The system supports structured recording and management of student placement activity, including logbook-style tracking of hours, tasks, and learning experiences.
  - *Rules:* The system must support configurable logbook functionality for students undertaking placements. Logbook capabilities must include: Recording of placement hours and attendance Entry of tasks, activities, or experiences undertaken Optional reflective entries aligned to learning objectives Logbook structures must be configurable to support different faculty or placement requirements. The system must …

## Planning Capabilities

- **PP-PL-01** [CORE] — *Monash Admin
Monash Faculty*: The system supports placement demand planning and request management.
  - *Rules:* Supports structured definition, aggregation, and tracking of placement demand against partner capacity.

- **PP-FN-01** [CORE] — *Monash Admin
Monash Faculty*: The system tracking and reporting of placement-related financial commitments where applicable.
  - *Rules:* The system must support tracking of placement-related costs, invoices, and payments. Financial approvals must align with delegated authority frameworks. The system must support reporting on placement costs by partner, faculty, discipline, and cohort. The system must support this capability without reliance on external spreadsheets.

## Cross-cutting Capabilities (Consolidated)

- **ER-01** [CORE] — *All Users*: The system provides a centralised eligibility and rules engine used across matching, applications, interviews, offers, and allocation.
  - *Rules:* The rules engine must consume authoritative data from integrated systems (e.g. SMS, compliance records) as inputs to eligibility decisions. Eligibility decisions must be automatically re-evaluated when upstream data changes (e.g. enrolment status, compliance expiry). Eligibility decisions are exposed as consistent, standardised and auditable outputs consumable across all modules. The system must s…

- **ER-02** [CORE] — *All Users*: The system evaluates academic, compliance, capacity, and placement-specific criteria.
  - *Rules:* Criteria configurable by authorised Monash admins. Evaluates academic, compliance (via CES), capacity, and placement-specific rules. Supports location/travel constraints and student-specific restrictions (e.g. conflicts, exclusions). All criteria support hard rules, soft preferences, and governed exceptions. Uses structured partner attributes and integrated geolocation services where required.

- **ER-03** [CORE] — *All Users*: The system ensures all eligibility decisions are auditable and reproducible.
  - *Rules:* Inputs, outputs, and overrides logged.

- **ER-04** [UNSPECIFIED]: The system supports governed manual eligibility overrides within defined thresholds.
  - *Rules:* Governance and approval rules are configurable. Escalation pathways must be configurable by faculty, unit, or placement type, including internal and/or partner approval workflows.

- **CESS-01** [CORE] — *Monash Admin
Monash Faculty*: The system provides a centralised Compliance Evaluation and Storage Service (CESS) that manages the secure storage, verification, evaluation, and lifecycle of student compliance artefacts used in placement processes.
  - *Rules:* Operates as a centralised, independent service providing standardised compliance evaluation outputs to consuming systems. Compliance artefacts are normalised into structured, attribute-based records with system-derived status (e.g. valid, expiring, expired, missing, rejected). Evaluation logic supports date-based validity, conditional requirements, completeness, and verification state. Compliance …

- **CESS-02** [CORE] — *Monash Admin
Monash Faculty*: The system ensures that all compliance evaluation outcomes are auditable, versioned, and reproducible, including the inputs, evaluation logic, and resulting compliance status at any point in time.
  - *Rules:* All compliance evaluation events must generate an immutable audit record capturing inputs, evaluation outputs, timestamps, and the invoking user or system process. Evaluation inputs must be version-referenced, including: compliance artefacts and their attributes applicable rule sets / evaluation logic version upstream data used in evaluation (e.g. course, placement requirements) Compliance evaluat…

- **BO-01** [CORE] — *Monash Admin
Monash Faculty*: The system supports bulk operations across key entities, including partners, students, placements, and compliance records.
  - *Rules:* Bulk create, update, and upload must be supported via UI and/or file import/API mechanisms. Bulk operations must include pre-validation, error handling, and detailed feedback (e.g. success/failure by record). Bulk operations must support partial success, with failed records isolated and actionable without reprocessing the entire batch. Bulk operations must generate comprehensive audit logs, includ…

- **CN-01** [CORE] — *All Users*: The system provides a unified communications and notifications framework.
  - *Rules:* Notifications are event-driven and decoupled from individual functional modules. Eliminates duplicated notification logic. The communications framework must support generation of structured documents based on system data, for distribution to internal and external stakeholders. Generated documents must support multiple output formats, including but not limited to: Formatted documents (e.g. PDF, Wor…

- **CN-02** [CORE] — *Monash Admin*: The system supports configurable notifications by trigger, role, channel, and timing.
  - *Rules:* Email, dashboard, calendar, optional SMS. The system must support notifications triggered by changes in upstream authoritative systems, including student enrolment status changes. This includes events where a student is no longer enrolled in a unit associated with an active or pending placement. Notifications must be configurable by: Recipient role (e.g. faculty admin, placement coordinator) Trigg…

- **CN-03** [CORE] — *Monash Admin*: The system logs and audits all notifications.
  - *Rules:* Delivery failures flagged.

- **CN-04** [CORE] — *All Users*: The system supports user-managed notification preferences within policy constraints.
  - *Rules:* Critical notifications cannot be disabled.

- **RA-01** [CORE] — *Monash Admin
Monash Faculty*: The system provides centralised reporting across the placement lifecycle.
  - *Rules:* Used by Students, Partners, and Monash Admins.

- **RA-02** [CORE] — *Monash Admin
Monash Faculty*: The system ensures reporting uses consistent underlying operational data.
  - *Rules:* Reporting consumes outputs from the Eligibility & Rules Engine (ERE) and the Compliance Evaluation Service (CES) to ensure alignment between operational decisions and reported metrics. Ensures consistency across metrics.

- **RA-03** [CORE] — *Monash Admin
Monash Faculty
Partner*: The system provides standard operational, compliance, and performance reports.
  - *Rules:* Faculty- and university-level views. Reporting must consume compliance status and evaluation outputs from the Compliance Evaluation Service (CES) to ensure consistency between operational decisions and reported metrics. Reports and exported data must support structured formats (e.g. Excel, CSV) suitable for external consumption and downstream processing.

- **RA-04** [PREFERRED] — *All Users*: The system supports configurable dashboards and data export for external analytics tools.
  - *Rules:* Power BI or equivalent.

- **RA-05** [CORE] — *Monash Admin
Monash Faculty*: The system provides reporting on partner ownership, including assigned Primary Relationship Managers across all partner records.
  - *Rules:* Filterable by faculty, partner type, status, and activity level.

- **RA-06** [CORE] — *Monash Admin*: The system provides workload and distribution reporting for Primary Relationship Managers.
  - *Rules:* Metrics include the number of partners, active placements, and recent activity per Relationship Manager.

- **RA-07** [CORE] — *Monash Admin*: The system provides partner engagement and activity reporting linked to the Primary Relationship Manager.
  - *Rules:* Includes placements created, applications, communications, and partner activity over time.

- **RA-08** [CORE] — *Monash Admin*: The system supports the identification and reporting of unassigned or inactive partner relationships.
  - *Rules:* Configurable thresholds for inactivity (e.g. no activity in 6–12 months).

- **RA-09** [CORE] — *Monash Admin*: The system supports export and integration of partner relationship data to external analytics platforms (e.g. Power BI, CRM).
  - *Rules:* Must respect RBAC and privacy constraints.

- **RA-10** [PREFERRED] — *Monash Admin*: The system should support integration with enterprise finance systems to provide visibility of placement-related financial commitments and forecasts.
  - *Rules:* The system should support read-only integration with Monash University finance systems (e.g. SAP) to retrieve financial data relevant to placement activities. This may include: Approved budgets Committed spend Forecast expenditure Payment status (where applicable) Financial data must be: Clearly identified as originating from the authoritative finance system Not editable within the placement syste…

- **RWA-01** [CORE] — *All Users*: The system supports configurable role-based access control (RBAC) with hierarchical roles.
  - *Rules:* Applies to all users: students, partners, faculty, central admins.

- **RWA-02** [CORE] — *Monash Admin*: The system enforces access to data and actions based on role, status, and workflow stage.
  - *Rules:* Prevents inappropriate visibility or action.

- **RWA-03** [CORE] — *Monash Admin
Monash Faculty*: The system provides a configurable workflow engine used consistently across all lifecycle processes.
  - *Rules:* Workflow configuration must be achievable without vendor intervention. Workflow configuration must be available across all modules.

- **RWA-04** [CORE] — *Monash Admin
Monash Faculty*: The system supports conditional workflows, approvals, and exception handling.
  - *Rules:* Faculty- and placement-type specific variants supported.

- **RWA-05** [CORE] — *Monash Admin*: The system maintains comprehensive audit logs of all actions.
  - *Rules:* Includes eligibility decisions, overrides, approvals, and communications.

- **RWA-06** [CORE] — *Monash Admin*: The system ensures audit records are immutable, timestamped, and attributable.
  - *Rules:* Required for compliance and dispute resolution.

- **RWA-07** [CORE] — *Monash Admin*: The system supports synchronisation of user identity lifecycle events and logging of authentication and access events.
  - *Rules:* User identity lifecycle events are synchronised with Monash identity systems (INT-01). Authentication and access events are captured, timestamped, and attributable to a user or system process. Events include login activity, access attempts, session activity, and changes to access or permissions. Identity and access events are centrally logged, immutable, and retained in line with audit requirement…

- **RWA-08** [CORE] — *Monash Admin*: The system supports governed configuration change management with versioning, traceability, and auditability.
  - *Rules:* Configuration changes are versioned, traceable, and auditable. Changes to configuration (e.g. workflows, rules, forms, roles) are recorded with timestamp, user, and change details. Configuration changes do not overwrite historical behaviour and preserve prior versions. Changes are governed by role-based access control and approval where required. Configuration history is accessible for audit, roll…

- **IM-01** [PREFERRED] — *Monash Admin
Monash Faculty
Partner*: The system provides a centralised Incident Management capability used across placement lifecycle activities.
  - *Rules:* Incident records are captured as structured, auditable entities. Incidents support classification, status, severity, and outcome attributes. Incident workflows are configurable and governed via the workflow engine (RWA-03). Incident events are timestamped, attributable, and immutable (RWA-05). Incident-related communications are managed via the Communications Framework (CN-01). Incident data is ac…

- **AI-01** [PREFERRED] — *Monash Admin
Monash Faculty
Partner*: The system supports AI-assisted generation of structured content across key workflows, including placement information and communications.
  - *Rules:* AI capabilities operate as assistive services and do not replace human decision authority. Content must be editable before submission Must align to Monash templates and standards No sensitive data used unless governed Users informed when AI is used- Supports regeneration/refinement

- **AI-02** [PREFERRED] — *Monash Admin
Monash Faculty
Partner*: The system supports AI-assisted summarisation of qualitative inputs such as applications, interview feedback, and evaluations.
  - *Rules:* AI capabilities operate as assistive services and do not replace human decision authority. Source data must remain visible- Human decision-making remains authoritative Outputs must be traceable Users can regenerate summaries

- **AI-03** [PREFERRED] — *Monash Admin
Monash Faculty
Partner*: The system supports AI-assisted drafting of communications across the placement lifecycle.
  - *Rules:* AI capabilities operate as assistive services and do not replace human decision authority. Must align to approved templates and tone Requires user review before sending Must be logged and auditable (CN-03)

- **AI-04** [CORE] — *Monash Admin*: The system provides governance, transparency, and control over all AI-assisted capabilities.
  - *Rules:* AI capabilities operate as assistive services and do not replace human decision authority. AI features configurable (by module/faculty) All AI actions logged and auditable (RWA-05) Vendor must disclose model/data handling System must operate without AI enabled AI outputs clearly distinguishable

## Integrations

- **INT-01** [CORE] — *All Users*: The system integrates with Monash University identity and access management services.
  - *Rules:* All users must authenticate via Monash University Identity and Access Management systems (e.g. Okta SSO). User identity must be managed centrally and not duplicated within the placement system. Role-based access control (RBAC) must be enforced through identity attributes and role assignments that align with Monash University IAM policies. Access provisioning and de-provisioning must reflect change…

- **INT-02** [CORE] — *Monash Admin
Monash Faculty
Monash Student*: The system integrates with Monash student administration systems.
  - *Rules:* The system must not override or persist authoritative student data beyond operational use. Student data (including enrolment, course progression, and academic attributes) must be sourced from Monash Student Management Systems (e.g. Callista, Ellucian Banner) as the authoritative systems of record, with read-only access where applicable. The system must support near real-time or event-driven integr…

- **INT-03** [CORE] — *All Users*: The system integrates with Monash University customer relationship management systems.
  - *Rules:* uniCRM is the authoritative source for partner and engagement data; the PMS owns placement operational data, with duplication minimised via references and events. Integration supports bi-directional data exchange (organisation data inbound; placement engagement events outbound) using consistent identifiers and matching rules to prevent duplication. All integration occurs via Monash middleware (e.g…

- **INT-04** [CORE] — *Monash Admin*: The system integrates with Monash integration middleware.
  - *Rules:* All system integrations must be mediated through Monash-approved integration middleware (e.g. MiX / MuleSoft), avoiding point-to-point integrations. The system must publish and consume events and APIs via the integration layer using secure, standardised interfaces. Integration patterns must support both synchronous (API) and asynchronous (event-driven) communication. Integration flows must include…

- **INT-05** [CORE] — *Monash Admin*: The system integrates with document execution platforms.
  - *Rules:* DocuSign or equivalent.

- **INT-06** [CORE] — *Monash Admin*: The system integrates with communications platforms.
  - *Rules:* Email gateways, calendar systems, optional SMS.

- **INT-07** [PREFERRED] — *Monash Admin*: The system should integrate with external data services, including verification, risk, and geolocation/mapping services.
  - *Rules:* External services may include: Business verification (e.g. ABN lookup) Risk and due diligence services Geolocation and mapping services for distance and travel time calculations Integration must support retrieval of travel distance and time estimates based on configurable parameters (e.g. transport mode, time of day).

- **INT-08** [CORE] — *Monash Admin*: The system provides secure, documented APIs for data exchange.
  - *Rules:* REST-based, authenticated, rate-limited.

## Non-Functional Specifications

- **NFR-01** [CORE] — *All Users*: The system complies with Monash University information security and privacy policies.
  - *Rules:* Including GDPR-aligned principles.

- **NFR-02** [CORE] — *All Users*: The system protects data using encryption in transit and at rest.
  - *Rules:* Applies to all environments.

- **NFR-03** [CORE] — *All Users*: The system enforces least-privilege access aligned to RBAC controls.
  - *Rules:* Applies to all users and integrations. Access controls align with centrally managed authentication policies enforced through IAM. Access controls enforce data isolation boundaries aligned to institutional governance.

- **NFR-04** [CORE] — *Monash Admin*: The system provides architectural transparency, including data flows, integration patterns, and control boundaries.
  - *Rules:* The system provides visibility of architecture, including data flows, integration patterns, and control boundaries. Architectural information covers core components, data movement, and system interactions. Documentation is maintained, current, and accessible to authorised Monash stakeholders. Visibility supports security, audit, and integration governance requirements. Access to architectural info…

- **NFR-05** [CORE] — *All Users*: The system supports peak concurrent usage during semester placement cycles.
  - *Rules:* Load assumptions to be validated with Monash University.

- **NFR-06** [CORE] — *All Users*: Ability to implement effective redundancy, backup, and disaster recovery strategies
  - *Rules:* SLA-defined RTO and RPO. Incident response includes defined notification timelines and communication obligations.

- **NFR-07** [CORE] — *Monash Admin*: The system provides operational service visibility, including health, availability, and diagnostic information.
  - *Rules:* The system provides visibility of service health, availability, and operational status. Monitoring includes system performance, availability, and integration activity. Diagnostic information supports identification of errors, failures, and degraded performance. Monitoring data is accessible to authorised users and aligned with role-based access control. Monitoring and diagnostic information suppor…

- **NFR-08** [CORE] — *All Users*: The system supports data retention, archival, and disposal aligned to Monash University policy.
  - *Rules:* Minimum seven (7) years. Retention policies support automated, policy-driven archival and retrieval of records, including configurable triggers and restoration capability.

- **NFR-09** [CORE] — *All Users*: The system maintains full auditability for regulatory and compliance purposes.
  - *Rules:* Aligns with the audit framework.

- **NFR-10** [CORE] — *Monash Admin*: The system supports data ownership, portability, and controlled extraction and deletion of data in open, non-proprietary formats.
  - *Rules:* Data ownership remains with Monash University. The system supports extraction of data in open, non-proprietary formats. Data extraction supports full and scoped export of core entities and associated relationships. Extracted data preserves structure, identifiers, and audit-relevant metadata. Data extraction and deletion are role-based, access-controlled, and auditable (RWA-01, RWA-05). The system …

- **NFR-11** [CORE] — *All Users*: The system complies with WCAG 2.1 AA accessibility standards.
  - *Rules:* Access to the Voluntary Product Accessibility Tests (VPATs)

- **NFR-12** [PREFERRED] — *Monash Admin*: The system and associated service model support ongoing operational improvement, controlled platform evolution, and alignment with Monash University’s evolving placement management requirements.
  - *Rules:* The Supplier should provide visibility of product roadmap, release management, and service improvement processes. Platform updates and enhancements should minimise operational disruption and preserve configuration integrity where feasible. Continuous improvement activities should support governance, service optimisation, operational efficiency, and evolving institutional requirements.

- **UX-01** [CORE] — *Monash Students; Partner / Host*: The system provides role-based self-service portals for students and industry / partner users to complete placement-related activities relevant to their role and lifecycle stage.
  - *Rules:* Portal access must be governed by role-based access control, workflow stage, and data visibility rules. Student portal capabilities must support, as applicable: profile completion, placement search or preference submission, eligibility visibility, application submission, compliance evidence upload, interview booking, offer acceptance, placement status tracking, logbooks or activity records, issue …

## Integration Details

| ID | Priority | Requirement | Key notes |
|---|---|---|---|
| INT-01 | CORE | The system integrates with Monash University identity and access management services. | All users must authenticate via Monash University Identity and Access Management systems (e.g. Okta SSO). User identity must be managed centrally and not duplicated within the plac… |
| INT-02 | CORE | The system integrates with Monash student administration systems. | The system must not override or persist authoritative student data beyond operational use. Student data (including enrolment, course progression, and academic attributes) must be s… |
| INT-03 | CORE | The system integrates with Monash University customer relationship management systems. | uniCRM is the authoritative source for partner and engagement data; the PMS owns placement operational data, with duplication minimised via references and events. Integration suppo… |
| INT-04 | CORE | The system integrates with Monash integration middleware. | All system integrations must be mediated through Monash-approved integration middleware (e.g. MiX / MuleSoft), avoiding point-to-point integrations. The system must publish and con… |
| INT-05 | CORE | The system integrates with document execution platforms. | DocuSign or equivalent. |
| INT-06 | CORE | The system integrates with communications platforms. | Email gateways, calendar systems, optional SMS. |
| INT-07 | PREFERRED | The system should integrate with external data services, including verification, risk, and geolocation/mapping services. | External services may include: Business verification (e.g. ABN lookup) Risk and due diligence services Geolocation and mapping services for distance and travel time calculations In… |
| INT-08 | CORE | The system provides secure, documented APIs for data exchange. | REST-based, authenticated, rate-limited. |

### Integration map (cross-refs)

| Capability | Spec IDs | Systems / pattern |
|---|---|---|
| Identity / SSO | PM-01, INT-01, RWA-07 | Monash IAM / SSO; identity lifecycle sync; auth event logging |
| Student data | SM-01, SM-02, INT-02 | Student Management / student administration systems; pre-enrolment reconciliation |
| CRM | INT-03, RA-09 | Monash CRM; partner relationship export |
| Middleware | INT-04 | Monash integration middleware |
| e-Signature / docs | INT-05, PM-09, PM-10 | Document execution platforms; agreement lifecycle |
| Communications | INT-06, CN-01–CN-04, PM-08 | Email/notification platforms; trigger/role/channel/timing |
| External data | INT-07 (PREFERRED), PM-03 (PREFERRED) | ABN lookup, verification, risk, geolocation/mapping |
| APIs | INT-08 | Secure documented APIs for data exchange |
| Calendar | IN-01 | Interview scheduling calendar integration |
| Analytics / BI | RA-04 (PREFERRED), RA-09 | Power BI / external analytics |
| Finance | RA-10 (PREFERRED), PP-FN-01 | Enterprise finance systems; placement financial commitments |
| Compliance vault | CESS-01, CESS-02 | Centralised compliance artefact storage & evaluation |

---

*End of Monash Part D2 functional specifications.*