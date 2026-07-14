# Monash Tender Go/No-Go Evaluation (Fable)
## NTU CSM (our product) vs Monash Part D2 (client requirements)

**Date:** 14 Jul 2026  
**Method:** Map all 92 Monash Part D2 requirements to NTU CSM functional capabilities. CORE weighted 1.0; PREFERRED 0.5.  
**Fit bands:** Full ≥85% · High partial 65–84% · Partial 35–64% · Low partial 20–34% · Gap <20%

---

## 1. Executive Summary (for management)

| Metric | Score | Interpretation |
|---|---|---|
| **Functional fit** | **33%** | Large domain gap — CSM is a career-services product; Monash is buying a regulated placement platform |
| **Technical fit** | **41%** | AWS/security baseline transfers; the Monash integration estate (Okta, Callista/Banner, uniCRM, MuleSoft, DocuSign) is almost entirely new |
| **Platform reuse potential** | **~50%** | Comms, forms, portals, RBAC shell, calendar/email, bulk ops, AI-assist are genuine platform assets |
| **Delivery effort** | **High (~50% new-build intensity)** | 42 of 92 requirements sit in Low partial / Gap — near half the schedule has little or no product coverage |
| **Delivery risk** | **High (~75%)** | CORE gaps concentrate in compliance (CESS), eligibility engine, agreements, and mandated Monash integrations |
| **Overall bid readiness** | **~38%** | Not bid-as-is; only viable as a funded placement-product build on CSM platform primitives |

### One-line decision frame

> **Do not treat this as an NTU CSM configuration bid.**  
> Treat it as: *"Should we fund a Placement Management product line, reusing CSM's cross-cutting platform (~half of that layer transfers), with Monash as the anchor customer?"*

### Recommended posture

| Option | When it makes sense |
|---|---|
| **No-bid / decline** | Budget or timeline assumes CSM works "as a placement system"; appetite for risk is low |
| **Conditional bid** | Multi-year SOW; phased MVP that defers most PREFERRED items; explicit funded scope for CESS + rules engine + agreements + Monash integrations |
| **Strategic bid** | Deliberate, board-approved entry into the AU university placement market with Monash as lighthouse |

---

## 2. Scorecard Detail

### 2.1 Functional fit — **33%**

| Monash domain | Fit | What CSM has | What Monash needs beyond CSM |
|---|---|---|---|
| Communications (CN) | ~60% | Campaigns, templates, segments, prefs, digests, trigger governance, letter generation | Fully event-driven decoupled framework; SMS; enrolment-change triggers; structured PDF/Word doc generation |
| Placement opportunities (PL) | ~57% | Job/internship posting lifecycle with approval + duplicate-previous | Placement-record semantics; ER-driven eligibility on publish/apply |
| Bulk operations (BO) | ~55% | CSV bulk for SGCC sessions, jobs, comms recipients | Bulk across partners/students/placements/compliance with partial-success isolation + full audit |
| Self-service portals (UX) | ~55% | Student + employer portals (apply, offers, journals, feedback) | Compliance upload, eligibility visibility, logbooks, incident reporting in one placement portal |
| Interviews (IN) | ~45% | Coaching calendar, Outlook sync, internship interview invitations, assessment templates | Multi-party time-zone-normalised scheduling; panel feedback with immutable attribution |
| AI capabilities (AI) | ~41% | AI email authoring; OpenAI feedback summarisation; AI skill match | AI-04 governance layer: per-module toggles, disclosure, auditability, operate-without-AI |
| Matching / applications (MA) | ~36% | Forms engine; CB eligibility check with on-screen ineligible prompt | Central ERE invocation; governed logged overrides; accessibility-compliant multimedia forms |
| Post-placement & planning (PP) | ~34% | Feedback/evaluation forms; journals; attendance tracking (E); financial support *application* forms | Configurable logbooks; demand planning vs partner capacity; placement cost/invoice/payment tracking |
| RBAC / workflow / audit (RWA) | ~34% | L4/L3/L2 roles, workgroups, scattered audit trails | Configurable workflow engine (RWA-03); config versioning (RWA-08); comprehensive immutable audit |
| Partner management (PM) | ~33% | Employer registration/approval, ACRA UEN, IRC auto-assign, blacklist/watchlist | Hierarchy + inheritance, lifecycle status model, assessment workflows, executed agreements + expiry, PRM governance |
| Offers / on-placement (OA) | ~32% | Offer letters, accept/reject + signed upload, faculty allocation capacity | Real-time capacity + teaching-period conflict prevention; incident-framework-aligned monitoring |
| Student management (SM) | ~26% | SIS sync, profiles, labels, remarks | Pre-enrolment reconciliation; WWCC/police/immunisation compliance records; enforced extensible profiles |
| Reporting (RA) | ~24% | Module reports, CSV exports, analytics-tool integration | Placement lifecycle reporting on ERE/CESS outputs; PRM ownership/workload/engagement reporting |
| Eligibility & rules engine (ER) | ~11% | Scattered per-module eligibility checks | Centralised, re-evaluating, auditable, reproducible rules engine consumed by every module |
| Compliance (CESS) | ~5% | — | Full compliance evaluation & storage service with versioned, reproducible outcomes |
| Incident management (IM) | ~5% | AWO "Placement at Risk" alert only | Centralised structured incident capability on the workflow engine |

### 2.2 Technical fit — **41%**

| Area | Fit | Notes |
|---|---|---|
| Encryption at rest/in transit | Full | RDS encryption, Multi-AZ, private network, Secrets Manager |
| Email + calendar (INT-06) | High-partial | SES + Outlook real-time sync + .ics proven; SMS absent (WhatsApp/Telegram ≠ SMS) |
| Backup / DR (NFR-06) | High-partial | Multi-AZ + 30-day backups; SLA-defined RTO/RPO needs formalising |
| APIs (INT-08) | Partial | NestJS APIs exist; documented, rate-limited external APIs not evidenced |
| Least privilege / concurrency / monitoring | Partial | Sound internal baseline; Monash-grade external visibility (NFR-04/07) needs packaging |
| Student data sync (INT-02) | Partial-low | NTU SIS batch pattern ≠ Callista/Banner event-driven via middleware |
| Identity (INT-01, RWA-07) | Low partial | NTU SSO/Cognito pattern; employers on passwords; no Okta, no de-provisioning sync |
| 7-year retention (NFR-08) | Low partial | Backups ≠ policy-driven archival/disposal with restoration |
| WCAG 2.1 AA + VPAT (NFR-11) | Gap | Not evidenced anywhere in CSM specs |
| uniCRM bi-directional (INT-03) | Gap | No CRM integration exists |
| Middleware mediation (INT-04) | Gap | All CSM integrations are point-to-point; MiX/MuleSoft mandated |
| Document execution (INT-05) | Gap | No DocuSign or equivalent |

### 2.3 Delivery effort — **High**

Rough work split (indicative, for planning — not a quote):

| Workstream | Nature | Effort band |
|---|---|---|
| Tenantise + brand; reuse comms/forms/portals/bulk ops | Reuse | M |
| Employer → Partner (hierarchy, lifecycle statuses, PRM, attribute-level visibility) | Major enhance | L |
| Internship → Placement (opportunities, offers, allocation conflicts, logbooks) | Major enhance | L |
| **CESS — compliance vault + evaluation service** | Net new | XL |
| **Eligibility & Rules Engine (ER-01–04) + re-evaluation on upstream change** | Net new | XL |
| **Configurable workflow engine + config change management (RWA-03/04/08)** | Net new | XL |
| Placement agreements + versioning/expiry + DocuSign | Net new | L |
| Monash Okta + Callista/Banner + MiX/MuleSoft + uniCRM | Net new integrations | XL |
| Incident management, demand planning, placement finance | Net new | L |
| WCAG 2.1 AA + VPAT + 7-year retention + AI governance | Hardening | M–L |
| Migration / UAT / faculty multi-variant configuration | Delivery | L |

**Implication:** a **multi-release product build** (realistically 12–24+ months depending on the MVP cut), not a 3–6 month CSM rollout.

### 2.4 Delivery risk — **High (~75%)**

| Risk | Severity | Why |
|---|---|---|
| Domain mismatch | Critical | CSM = career services & internships; Monash = regulated, compliance-gated student placements |
| CORE compliance gap | Critical | SM-04/05 + CESS-01/02 absent; compliance is the spine of the Monash spec, not a feature |
| No central rules engine | Critical | ER-01 is referenced by PL, MA, OA, RA — the architecture assumes it exists |
| No configurable workflow engine | Critical | RWA-03 is invoked by PM-05, RWA-04, IM-01; CSM workflows are hardcoded per module |
| Monash integration estate | High | Okta, Callista/Banner, MuleSoft, uniCRM, DocuSign — five mandated CORE integrations, all new |
| Faculty multi-variant config without vendor | High | Monash repeatedly requires "configurable without vendor intervention"; CSM is vendor-configured |
| Accessibility + AU privacy evidence | Medium-High | No VPAT; SG PDPA posture needs mapping to Monash/GDPR-aligned policy |
| Schedule risk if bid as "reuse" | Critical | Mis-scoping this as tenant rollout is the most likely failure mode |

---

## 3. What Monash asked for that NTU CSM does **not** fulfil

Below = requirements where CSM has **no credible product fulfilment** today (fit < ~35%), or only a weak analogue that would score **Does Not Comply / heavily Partial** without major build.

### 3.1 Critical CORE gaps (likely deal-breakers if unanswered)

#### Compliance & student readiness
| ID | Monash requirement | CSM status |
|---|---|---|
| **SM-04** | Student compliance artefacts + work history as structured records | **Not fulfilled** |
| **SM-05** | Admin verify/approve/reject certifications (WWCC, Police Check, immunisations) incl. bulk ops | **Not fulfilled** |
| **CESS-01** | Centralised Compliance Evaluation & Storage Service | **Not fulfilled** |
| **CESS-02** | Versioned, reproducible, auditable compliance evaluations | **Not fulfilled** |
| **SM-02** | Pre-enrolled provisional students + reconciliation to Callista | **Not fulfilled** |

#### Eligibility & rules platform
| ID | Monash requirement | CSM status |
|---|---|---|
| **ER-01** | Centralised rules engine, auto re-evaluation on upstream change | **Not fulfilled** (scattered per-module checks) |
| **ER-02** | Academic + compliance + capacity + geo/travel criteria; hard/soft/exception rules | **Not fulfilled** |
| **ER-03** | Auditable, reproducible eligibility decisions | **Not fulfilled** |
| **ER-04** | Governed overrides with configurable thresholds/escalation | **Not fulfilled** |
| **MA-01 / MA-02** | ERE-driven matching + governed logged overrides | **Not fulfilled** as specified |

#### Workflow, configuration & audit platform
| ID | Monash requirement | CSM status |
|---|---|---|
| **RWA-03** | Configurable workflow engine across all modules, no vendor intervention | **Not fulfilled** (hardcoded workflows) |
| **RWA-04** | Conditional workflows + faculty/placement-type variants | **Not fulfilled** |
| **RWA-08** | Governed config change management with versioning + rollback | **Not fulfilled** |

#### Partner / agreement / relationship governance
| ID | Monash requirement | CSM status |
|---|---|---|
| **PM-05** | Configurable partner assessment routing via workflow engine | **Not fulfilled** |
| **PM-09 / PM-10** | Placement agreement generation/execution + versioning + 30/60/90-day expiry alerts | **Not fulfilled** (auto letters ≠ executed agreements) |
| **PM-07 / PM-11** | Assessment scoring models; master multi-student agreements *(PREFERRED)* | **Not fulfilled** |
| **RA-05 / RA-06 / RA-07** | PRM ownership, workload, engagement reporting | **Not fulfilled** |
| **RA-02** | Reporting consuming ERE/CESS outputs | **Not fulfilled** (nothing to consume) |

#### On-placement operations & planning
| ID | Monash requirement | CSM status |
|---|---|---|
| **OA-03** | On-placement monitoring aligned to Incident Management Framework | **Not fulfilled** (AWO risk alert is not incident management) |
| **IM-01** | Centralised Incident Management *(PREFERRED but wired into OA-03)* | **Not fulfilled** |
| **PP-PL-01** | Placement demand planning vs partner capacity | **Not fulfilled** |
| **PP-FN-01** | Placement cost/invoice/payment tracking without spreadsheets | **Not fulfilled** (financial *support application* forms ≠ this) |

#### Monash enterprise integrations (CORE)
| ID | Monash requirement | CSM status |
|---|---|---|
| **INT-01** | Monash Okta IAM for *all* users incl. partners; provisioning/de-provisioning | **Not fulfilled** as-is (NTU SSO pattern; employers on passwords) |
| **INT-02** | Callista/Banner authoritative, event-driven, read-only | **Not fulfilled** as-is (NTU SIS batch pattern) |
| **INT-03** | uniCRM bi-directional partner/engagement exchange | **Not fulfilled** |
| **INT-04** | All integrations via Monash middleware (MiX/MuleSoft) | **Not fulfilled** (point-to-point today) |
| **INT-05** | Document execution (DocuSign or equivalent) | **Not fulfilled** |
| **NFR-11** | WCAG 2.1 AA + VPAT access | **Not evidenced** |

### 3.2 Significant partials (would still fail a "Complies" claim without redesign)

CSM has real analogues here, but they are too different in behaviour, domain, or depth to claim compliance without material product work:

| ID | Monash need | CSM analogue | Gap to close |
|---|---|---|---|
| **PM-01** | Partner SSO via Okta; guest for registration only | Employer password accounts post-approval | Okta partner SSO + guest registration path |
| **PM-02** | Hierarchical partner orgs + critical-attribute approval workflows | Flat company profile, join-existing wizard | Hierarchy, inheritance, configurable approvals |
| **PM-06** | Configurable status lifecycle that drives eligibility/visibility | Approve, blacklist/watchlist, inactive | Status taxonomy + behavioural hooks into ER |
| **PM-12–15** | Auditable PRM model + attribute-level, faculty-scoped visibility | IRC auto-assignment; workgroups | PRM history, tiered visibility, attribute ACL |
| **SM-01 / SM-03** | Extensible attributes + enforced profile completeness, no vendor | SIS sync; profiles; labels | Callista mapping; self-serve attribute config; enforcement gates |
| **SM-06** | Cohorts feeding rules, allocation, comms; auditable membership | Labels (rule-based) + lists | Wire cohorts into ER/allocation with audit/versioning |
| **PL-02** | ER-01 outcomes applied to opportunities | CB school eligibility checks | Central engine coupling |
| **MA-03 / MA-04** | Real-time ineligibility block; accessible multimedia forms | On-screen ineligible prompt (6.2.1.17.4); forms builder | ERE-backed validation; multimedia + WCAG forms |
| **IN-01 / IN-02** | TZ-normalised multi-party interviews; immutable panel feedback | Coaching calendar + Outlook; assessment templates | Partner-facing scheduling; panel model |
| **OA-01 / OA-02** | Real-time capacity; teaching-period conflict prevention | Offer letters; faculty allocation capacity | Capacity/conflict engine for placements |
| **PP-02** | Configurable logbook (hours/tasks/reflection) with partner sign-off | Journals; attendance tracking (E) | Logbook product with faculty config |
| **BO-01** | Bulk ops incl. partial success + full audit across placement entities | CSV bulk in SGCC/jobs/comms | Extend to partners/compliance/placements + partial-success UX |
| **CN-01 / CN-02** | Event-driven decoupled framework; SMS; enrolment-change triggers; PDF/Word doc gen | Strong campaign/template/prefs platform; letters | Decouple triggers; SMS channel; upstream event consumption |
| **RWA-01 / RWA-05 / RWA-06** | Configurable hierarchical RBAC; comprehensive immutable audit | Fixed L4/L3/L2 + workgroups; scattered audit trails | Role configurability; platform-wide audit plane |
| **RA-01 / RA-03 / RA-09** | Placement lifecycle + compliance reporting; governed exports | Module reports; CSV; analytics-tool integration | Placement reporting layer on ERE/CESS data |
| **UX-01** | Full placement self-service portal (compliance, eligibility, logbook, incidents) | Student/employer career portals | New placement-specific portal journeys |
| **INT-08** | Documented, authenticated, rate-limited REST APIs | Internal NestJS APIs | External API productisation |
| **NFR-08 / NFR-09 / NFR-10** | 7-year policy archival; full auditability; open-format portability | 30-day backups; partial audit; CSV exports | Retention engine; audit plane; structured full export |
| **AI-04** | AI governance: toggles, disclosure, audit, operate-without-AI | OpenAI summaries; AI email authoring | Governance/control layer over all AI features |

### 3.3 PREFERRED items not fulfilled (lower bid weight, still visible)

| ID | Requirement | CSM |
|---|---|---|
| PM-03 | ABN lookup | Partial — ACRA UEN is the direct analogue; rebuild for AU |
| PM-07 / PM-11 / PM-13 / PM-14 | Assessment scoring, master agreements, PRM visibility/logging | Not fulfilled / weak |
| RA-04 | Configurable dashboards + Power BI export | Partial (analytics-tool integration + exports) |
| RA-10 | SAP/finance read-only integration | Not fulfilled |
| INT-07 | Verification, risk, geolocation/travel-time services | Partial (business lookup pattern only; no risk/geo) |
| IM-01 | Incident Management | Not fulfilled |
| AI-01 / AI-02 / AI-03 | Content generation, summarisation, comms drafting | Partial — genuine assets, need governance wrapper (AI-04) |
| NFR-12 | Roadmap/service-model alignment | Process packaging required |

---

## 4. What *does* transfer from NTU CSM (reuse thesis)

Use this in a conditional bid narrative:

1. **Multi-role portals** — students, employers/partners, staff, school-scoped staff, external supervisors (time-limited links)
2. **Employer onboarding & moderation** — registration wizard, approval, business-registry lookup, blacklist/watchlist (seed for Partner Management)
3. **Opportunity → application → offer lifecycle** — WIE/GE internships are the closest domain bridge, incl. offer letters and placement letters
4. **Communications platform** — templates, campaigns, segments, prefs, digests, suppression, per-campaign analytics
5. **Forms engine** — lifecycle, builder, RBAC, response audit, analytics/CSV
6. **Assessment machinery** — journals, configurable assessment templates, supervisor ratings, faculty allocation with real-time capacity
7. **SSO + SIS sync patterns** — proven patterns, must be re-implemented against Okta/Callista, not copy-pasted
8. **RBAC / workgroups / labels** — institutional access model and rule-based segmentation experience
9. **Calendar (Outlook real-time + .ics) and email delivery at scale**
10. **AWS security baseline** — encryption, Multi-AZ, secrets, monitoring, backups
11. **AI assist** — email authoring + feedback summarisation are direct starting points for AI-01/02/03

---

## 5. Decision recommendation

### Go / No-Go

| Signal | Result |
|---|---|
| Bid CSM "as the Monash placement system" with config-only delivery | **NO-GO** |
| Bid a funded Placement MVP on CSM platform + Monash integrations | **CONDITIONAL GO** |
| Strategic entry into AU placement market with multi-year investment | **GO only with board-level product funding** |

### Minimum conditions for a Conditional GO

1. Explicit budget & timeline for the three platform builds — **CESS, Eligibility & Rules Engine, configurable Workflow Engine** — plus **agreements/DocuSign** and **INT-01/02/03/04**.
2. An MVP cut Monash accepts (e.g. defer IM-01, RA-10, INT-07, AI PREFERRED items; phase PRM analytics).
3. Architecture commitment to Monash middleware from day one — no point-to-point interim that must be unwound.
4. Accessibility (VPAT) and AU privacy evidence plan in place before shortlist demos.
5. Honest Part D2 responses: the majority of CORE items are **Partially Complies / Does Not Comply with roadmap** — overclaiming "Complies" creates contract risk.

### Suggested management statement

> NTU CSM provides roughly **33% functional** and **41% technical** fit to Monash Part D2. What transfers is the cross-cutting platform layer — communications, forms, portals, RBAC shell, calendar/email, AWS baseline. What does not exist is the placement core Monash's architecture is built around: the compliance service (CESS), the central eligibility engine (ER), the configurable workflow engine (RWA-03), executed placement agreements, and the five mandated Monash integrations. Bid only if we deliberately fund a Placement product vertical; a "reuse CSM" bid is either unwinnable or undeliverable.

---

## Appendix A — Scoring method

- Sources: `monash-functional-specs.md` (92 Part D2 items), `ntu-csm-functional-specs.md` (CSM product capabilities).
- Each Monash ID scored 0–1 on reuse fit; CORE weight 1.0, PREFERRED 0.5; ER-04 (unspecified priority) treated as CORE.
- Functional vs Technical split: functional = PM…UX; technical = INT + NFR.
- Delivery effort intensity ≈ share of work that is major-enhance / net-new.
- Delivery risk ≈ concentration of CORE gaps in placement-critical and integration domains.
- This is an independent re-score; it is deliberately conservative where the Monash rules text demands configurability, auditability, or reproducibility that CSM documentation does not evidence.

## Appendix B — Bucket counts (all 92)

Appendix B summarises how every Monash Part D2 requirement was classified when compared against NTU CSM.  
It does **not** introduce new requirements — it is the **distribution behind the % scores** in the executive summary.

Each of the **92** Monash specs was scored for reuse fit (0–1), then placed into one band:

| Band | Fit score (approx.) | What it means for CSM vs Monash | Bid implication |
|---|---|---|---|
| **Full** | ≥ ~85% | CSM already fulfils the capability (config / tenantisation only) | Can claim **Complies** with low delivery risk |
| **High partial** | ~65–84% | Strong analogue exists (posting lifecycle, feedback surveys, email/calendar, DR baseline) | Usually **Partially Complies**; remapping + Monash integrations needed |
| **Partial** | ~35–64% | Related feature exists, but behaviour / domain / depth does not match the Monash spec | **Partially Complies**; material product work to close |
| **Low partial** | ~20–34% | Only a weak analogue; would not satisfy evaluation as written | Closer to **Does Not Comply** unless rebuilt |
| **Gap** | < ~20% | No credible product capability to reuse | **Does Not Comply** today; net-new build |

### Counts

| Band | Count | % of 92 |
|---|---|---|
| Full | 1 | 1% |
| High partial | 7 | 8% |
| Partial | 42 | 46% |
| Low partial | 15 | 16% |
| Gap | 27 | 29% |
| **Total** | **92** | **100%** |

### How to read this

- **Only 1 requirement** (encryption, NFR-02) is effectively out of the box. CSM is not a drop-in Monash placement system.
- **7 High partial** items are the genuinely strong analogues: posting lifecycle (PL-01), record reuse (PL-03), post-placement surveys (PP-01), notification preferences (CN-04), communications framework (CN-01), email/calendar integration (INT-06), and backup/DR (NFR-06).
- **42 Partial** is the largest group — real features exist but the Monash rules text (configurability, auditability, reproducibility, faculty variants) pushes each one into material product work.
- **Low partial + Gap = 42 items (~46%)** — nearly half the schedule has little or no product coverage, concentrated in the placement core (CESS, ER, workflow engine, agreements, PRM analytics, planning/finance) and the mandated Monash integrations (Okta, Callista/Banner, uniCRM, middleware, DocuSign).

### Link to the headline metrics

| Metric | How Appendix B relates |
|---|---|
| Functional fit ~33% | Weighted average dominated by the Partial and Gap bands on functional IDs |
| Technical fit ~41% | Lifted by AWS/security/email items; dragged down by the five gap-band integrations |
| High delivery effort / risk | Driven by the 42 Low partial + Gap items, 34 of which are CORE |

**Management takeaway:** Appendix B shows a **platform-reuse + large net-new** profile — the reusable half is horizontal infrastructure, while the vertical placement capabilities Monash is actually procuring must be built.