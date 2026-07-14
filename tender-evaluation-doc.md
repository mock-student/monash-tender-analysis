# Monash Tender Go/No-Go Evaluation (Cursor AI)
## NTU CSM (our product) vs Monash Part D2 (client requirements)

**Date:** 14 Jul 2026  
**Method:** Map all 92 Monash Part D2 requirements to NTU CSM functional capabilities. CORE weighted 1.0; PREFERRED 0.5.  
**Fit bands:** Full ≥85% · High partial 65–84% · Partial 35–64% · Low partial / Gap <35%

---

## 1. Executive Summary (for management)

| Metric | Score | Interpretation |
|---|---|---|
| **Functional fit** | **39%** | Large domain gap — placement-core features largely absent |
| **Technical fit** | **49%** | Stack/patterns reusable; Monash-specific integrations mostly new |
| **Platform reuse potential** | **~52%** | Comms, forms, RBAC, employer shell, internship analogues reusable |
| **Delivery effort** | **High (~48% new-build intensity)** | Closer to a new product vertical than a tenant rollout |
| **Delivery risk** | **High (~72%)** | CORE gaps in compliance, eligibility engine, agreements, Monash middleware/CRM |
| **Overall bid readiness** | **~42%** | Not bid-as-is; only viable with explicit placement-product investment |

### One-line decision frame

> **Do not treat this as an NTU CSM configuration bid.**  
> Treat it as: *“Should we fund a Placement Management product line, leveraging CSM platform primitives (~½ of cross-cutting capability), to meet Monash?”*

### Recommended posture

| Option | When it makes sense |
|---|---|
| **No-bid / decline** | Budget/timeline assumes reuse of CSM “as a placement system”; want low risk |
| **Conditional bid** | Multi-year SOW; phased MVP excluding PREFERRED; clear budget for CESS + ERE + agreements + Monash integrations |
| **Strategic bid** | Explicit decision to enter AU university placement market with Monash as lighthouse |

---

## 2. Scorecard Detail

### 2.1 Functional fit — **39%**

| Monash domain | Fit | What CSM has | What Monash needs beyond CSM |
|---|---|---|---|
| Communications | ~68% | Campaigns, prefs, digests, AI email | Placement-event triggers, SMS, structured doc generation |
| Bulk operations | ~70% | CSV bulk (SGCC, jobs, recipients) | Partners / compliance / placements with partial-success UX |
| RBAC / audit | ~51% | L4/L3/L2, workgroups, some audit trails | Configurable workflow engine; config versioning; full audit |
| Placement opportunities | ~53% | Job/internship posting lifecycle | Placement-specific opportunity semantics + ER-driven publish |
| Interviews | ~55% | Coaching calendar; intern interview invite | Multi-party TZ interview scheduling + panel feedback |
| Matching / applications | ~44% | Forms; CB eligibility prompt | Central ERE; real-time gate; governed overrides |
| Offers / on-placement | ~37% | Offer letters, accept/reject | Real-time capacity + conflict allocation; incident monitoring |
| Partner management | ~35% | Employer profiles, IRC, ACRA, blacklist | Hierarchy, PRM, assessment, agreements, expiry |
| Student management | ~36% | SIS sync, profiles, labels | Pre-enrolment; WWCC/police/imms compliance |
| Post-placement & planning | ~38% | Feedback forms; journals; finance *support* forms | Logbooks; demand planning; placement finance tracking |
| Reporting (placement) | ~30% | Module reports / exports | PRM/compliance/ERE-backed placement reporting |
| Eligibility engine (ER) | ~20% | Scattered checks | Centralised, auditable, re-evaluating rules engine |
| Compliance (CESS) | ~5% | — | Full compliance vault + evaluation service |
| Incident management | ~5% | — | Centralised IM framework |

### 2.2 Technical fit — **49%**

| Area | Fit | Notes |
|---|---|---|
| Encryption / AWS resilience | High | RDS encrypt, Multi-AZ, backups, Secrets Manager |
| API / NestJS platform | High-partial | APIs exist; need Monash-grade documented/rate-limited external APIs |
| Email + Outlook calendar | High-partial | SES + Outlook/.ics proven |
| SSO pattern | Partial | NTU SSO/Cognito ≠ Monash Okta IAM + partner SSO + de-provisioning |
| Student data sync | Partial | SIS pattern ≠ Callista/Banner event-driven via middleware |
| Business registry lookup | Partial | ACRA/UEN ≠ ABN (+ risk/geo services) |
| Document e-sign | Gap | No DocuSign (or equivalent) |
| CRM integration | Gap | No uniCRM bi-directional |
| Middleware mediation | Gap | Point-to-point today; Monash requires MiX/MuleSoft |
| WCAG 2.1 AA / VPAT | Low | Not evidenced |
| 7-year retention/archival | Low-partial | Backups ≠ policy-driven 7-year archival/disposal |

### 2.3 Delivery effort — **High**

Rough work split (indicative, for planning — not a quote):

| Workstream | Nature | Effort band |
|---|---|---|
| Tenantise + brand Monash; reuse Comms/Forms/RBAC | Reuse | M |
| Employer → Partner model (hierarchy, PRM, status lifecycle) | Major enhance | L |
| Internship → Placement opportunity / offer / allocation | Major enhance | L |
| **CESS (compliance vault + evaluation)** | Net new | XL |
| **Eligibility & Rules Engine** | Net new | XL |
| Placement agreements + DocuSign | Net new | L |
| Monash Okta + Callista/Banner + MiX + uniCRM | Net new integrations | XL |
| Incident management + demand planning + finance tracking | Net new | L |
| Accessibility (WCAG AA) + retention policy | Hardening | M |
| Data migration / UAT / faculty multi-tenancy | Delivery | L |

**Implication:** Expect a **multi-release product build** (often 12–24+ months depending on MVP cut), not a 3–6 month CSM rollout.

### 2.4 Delivery risk — **High (~72%)**

| Risk | Severity | Why |
|---|---|---|
| Domain mismatch | Critical | CSM = career services; Monash = regulated student placement |
| CORE compliance gap | Critical | WWCC/police/imms + CESS absent — often non-negotiable for placements |
| No central rules engine | Critical | Monash architecture assumes ER everywhere |
| Monash integration estate | High | Okta, Callista/Banner, MiX/MuleSoft, uniCRM, DocuSign — all new |
| Configurable workflow engine | High | CSM workflows are largely productised/hardcoded per module |
| Faculty multi-variant config | High | Monash requires faculty-configurable rules without vendor interven­tion |
| Accessibility / AU privacy | Medium-High | WCAG AA + Monash policy/GDPR-aligned controls need evidence |
| Schedule risk if bid as “reuse” | Critical | Mis-scoping is the most likely bid failure mode |

---

## 3. What Monash asked for that NTU CSM does **not** fulfil

Below = requirements where CSM has **no credible product fulfilment** today (fit &lt; ~35%), or only a weak analogue that would still score **Does Not Comply / heavily Partial** without major build.

### 3.1 Critical CORE gaps (likely deal-breakers if unanswered)

#### Compliance & student readiness
| ID | Monash requirement | CSM status |
|---|---|---|
| **SM-04** | Student compliance artefacts + work history | **Not fulfilled** |
| **SM-05** | Admin verify/approve/reject certifications (WWCC, Police Check, immunisations) | **Not fulfilled** |
| **CESS-01** | Centralised Compliance Evaluation & Storage Service | **Not fulfilled** |
| **CESS-02** | Versioned, reproducible, auditable compliance evaluations | **Not fulfilled** |
| **SM-02** | Pre-enrolled provisional students + SMS reconciliation | **Not fulfilled** |

#### Eligibility & rules platform
| ID | Monash requirement | CSM status |
|---|---|---|
| **ER-01** | Centralised eligibility & rules engine across lifecycle | **Not fulfilled** |
| **ER-02** | Academic + compliance + capacity + geo/travel rules | **Not fulfilled** (scattered checks only) |
| **ER-03** | Auditable/reproducible eligibility decisions | **Not fulfilled** |
| **ER-04** | Governed overrides with thresholds/escalation | **Not fulfilled** |
| **MA-01 / MA-02 / MA-03** | ERE-driven matching, governed overrides, real-time block | **Not fulfilled** as specified |

#### Partner / agreement / relationship governance
| ID | Monash requirement | CSM status |
|---|---|---|
| **PM-05** | Configurable partner assessment routing via workflow engine | **Not fulfilled** |
| **PM-07** | Configurable assessment criteria & scoring *(PREFERRED)* | **Not fulfilled** |
| **PM-09 / PM-10** | Placement agreement generation/execution + versioning + expiry alerts | **Not fulfilled** (letters ≠ executed agreements) |
| **PM-11** | Master multi-student agreements *(PREFERRED)* | **Not fulfilled** |
| **RA-05 / RA-06 / RA-07** | PRM ownership, workload, engagement reporting | **Not fulfilled** |

#### On-placement operations
| ID | Monash requirement | CSM status |
|---|---|---|
| **OA-03** | On-placement monitoring aligned to Incident Management Framework | **Not fulfilled** |
| **IM-01** | Centralised Incident Management *(PREFERRED but operationally linked to OA-03)* | **Not fulfilled** |
| **PP-PL-01** | Placement demand planning vs partner capacity | **Not fulfilled** |
| **PP-FN-01** | Placement cost/invoice/payment tracking & reporting | **Not fulfilled** (finance *support forms* ≠ this) |

#### Monash enterprise integrations (CORE)
| ID | Monash requirement | CSM status |
|---|---|---|
| **INT-03** | uniCRM bi-directional partner/engagement integration | **Not fulfilled** |
| **INT-04** | All integrations via Monash middleware (MiX / MuleSoft) | **Not fulfilled** |
| **INT-05** | Document execution (DocuSign or equivalent) | **Not fulfilled** |
| **INT-01** | Monash Okta IAM for *all* users incl. partners | **Not fulfilled** as-is (SSO pattern only) |
| **INT-02** | Callista / Banner authoritative student integration | **Not fulfilled** as-is (NTU SIS pattern only) |

### 3.2 Significant partials (would still fail a “Complies” claim without redesign)

These are **not** full gaps, but CSM analogues are too different to claim compliance without material product work:

| ID | Monash need | CSM analogue | Gap to close |
|---|---|---|---|
| **PM-01** | Partner SSO (Okta); guest only for registration | Employer password accounts | Partner SSO + registration guest path |
| **PM-02** | Hierarchical partner orgs + critical-field approvals | Flat employer profile | Hierarchy, inheritance, approval workflows |
| **PM-06** | Partner status model drives eligibility | Employer approve/blacklist | Status taxonomy + behavioural hooks |
| **PM-12–15** | Primary Relationship Manager model | IRC assignment | PRM ownership, history, attribute-level ACL |
| **SM-01 / SM-03** | Extensible placement attributes + completeness gates | SIS + profiles | Callista mapping + enforceable profile gates |
| **SM-06** | Placement cohorts for rules/allocation | Labels / SGCC eligibility | Cohort engine wired to ER/allocation |
| **PL-01–03** | Placement opportunity object + ER-driven publish | Job/internship board | Domain remapping + ER coupling |
| **OA-01 / OA-02** | ER-informed offers + no double allocation | Internship offers | Capacity/conflict engine for teaching periods |
| **IN-01 / IN-02** | Placement interview scheduling + panel feedback | Coaching + intern invite | Partner-facing interviews + structured panels |
| **PP-02** | Placement logbook (hours/tasks/reflection) | Internship journals | Configurable logbook product |
| **RWA-03 / RWA-04 / RWA-08** | Admin-configurable workflow engine + config versioning | Hardcoded module workflows | True workflow/config platform |
| **RA-01–03 / RA-09** | Placement lifecycle reporting on ER/CESS data | Module reports | Placement reporting plane |
| **UX-01** | Full placement self-service portal | Student/employer career portals | Compliance, eligibility, logbook, incidents UX |
| **NFR-08 / NFR-11** | 7-year retention; WCAG 2.1 AA + VPAT | Backups; no VPAT | Policy archival + a11y programme |
| **AI-04** | AI governance (toggleable, disclosed, auditable) | OpenAI summaries / AI email | Monash-grade AI controls |

### 3.3 PREFERRED items not fulfilled (lower bid weight, still visible)

| ID | Requirement | CSM |
|---|---|---|
| PM-03 | ABN lookup | Partial (ACRA only) — rebuild for AU |
| PM-07 / PM-11 / PM-13 / PM-14 | Assessment scoring, master agreements, PRM visibility/logging | Not fulfilled / weak |
| RA-04 | Configurable dashboards + Power BI | Partial (exports/analytics pattern) |
| RA-10 | SAP/finance integration for commitments/forecasts | Not fulfilled |
| INT-07 | Risk + geolocation/travel-time services | Not fulfilled |
| IM-01 | Incident Management | Not fulfilled |
| AI-01 / AI-02 | Placement content gen + summarisation | Partial |
| NFR-12 | Monash-aligned continuous improvement/service model | Process packaging required |

---

## 4. What *does* transfer from NTU CSM (reuse thesis)

Use this in a conditional bid narrative:

1. **Multi-role portals** — students, employers/partners, staff, school-scoped staff  
2. **Employer onboarding & moderation** — registration, approvals, blacklist (seed for Partner Mgmt)  
3. **Opportunity posting + applications + offers** — closest domain bridge via WIE/GE internships  
4. **Communications platform** — templates, segments, prefs, digests  
5. **Forms engine** — applications, feedback, surveys  
6. **SSO + student-system sync patterns** — must be rebuilt for Monash systems, not copy-pasted  
7. **RBAC / workgroups / labels** — institutional access model experience  
8. **Calendar sync (Outlook) + email (.ics)**  
9. **AWS security baseline** — encryption, Multi-AZ, secrets, monitoring  
10. **AI assist for communications** — starting point for AI-03  

---

## 5. Decision recommendation

### Go / No-Go

| Signal | Result |
|---|---|
| Bid CSM “as the Monash placement system” with config-only delivery | **NO-GO** |
| Bid a funded Placement MVP on CSM platform + Monash integrations | **CONDITIONAL GO** |
| Strategic entry to placement market with multi-year investment | **GO only with board-level product funding** |

### Minimum conditions for a Conditional GO

1. Explicit budget & timeline for **CESS + Eligibility Engine + Agreements/DocuSign + Monash INT-01/02/03/04**.  
2. MVP scope cut that Monash accepts (e.g. defer PREFERRED AI/IM/finance; phase PRM analytics).  
3. Architecture commitment to Monash middleware (no long-term point-to-point).  
4. Accessibility & AU privacy evidence plan before shortlist demos.  
5. Honest Part D2 responses: majority CORE items = **Partially Complies / Does Not Comply** with roadmap — not “Complies”.

### Suggested management statement

> NTU CSM provides roughly **40% functional** and **50% technical** fit to Monash Part D2. The largest gaps are Monash’s placement-core capabilities (compliance/CESS, eligibility engine, agreements, incident/demand planning) and Monash enterprise integrations. We should only pursue this tender if we intentionally fund a Placement product vertical; otherwise we risk an unwinnable bid or an undeliverable contract.

---

## Appendix A — Scoring method

- Sources: `monash-functional-specs.md` (92 Part D2 items), `ntu-csm-functional-specs.md` (CSM product capabilities).  
- Each Monash ID scored 0–1 on reuse fit; CORE weight 1.0, PREFERRED 0.5.  
- Functional vs Technical split: functional = PM…UX; technical = INT + NFR.  
- Delivery effort intensity ≈ share of work that is major enhance / net-new.  
- Delivery risk ≈ concentration of CORE gaps in placement-critical + integration domains.

Paste this in place of the current Appendix B section:


## Appendix B — Bucket counts (all 92)

Appendix B summarises how every Monash Part D2 requirement was classified when compared against NTU CSM.  
It does **not** introduce new requirements — it is the **distribution behind the % scores** in the executive summary.

Each of the **92** Monash specs was scored for reuse fit (0–1), then placed into one band:

| Band | Fit score (approx.) | What it means for CSM vs Monash | Bid implication |
|---|---|---|---|
| **Full** | ≥ ~85% | CSM already fulfils the capability (config / tenantisation only) | Can claim **Complies** with low delivery risk |
| **High partial** | ~65–84% | Strong analogue exists (e.g. employer ≈ partner shell, internship ≈ placement flows, comms/forms/RBAC) | Usually **Partially Complies**; remapping + Monash integrations needed |
| **Partial** | ~35–64% | Related feature exists, but behaviour / domain / depth does not match the Monash spec | **Partially Complies**; material product work to close |
| **Low partial** | ~20–34% | Only a weak analogue; would not satisfy evaluation as written | Closer to **Does Not Comply** unless rebuilt |
| **Gap** | &lt; ~20% | No credible product capability to reuse | **Does Not Comply** today; net-new build |

### Counts

| Band | Count | % of 92 |
|---|---|---|
| Full | 1 | 1% |
| High partial | 25 | 27% |
| Partial | 38 | 41% |
| Low partial | 8 | 9% |
| Gap | 20 | 22% |
| **Total** | **92** | **100%** |

### How to read this

- **Only 1 requirement** is effectively “out of the box.” CSM is not a drop-in Monash placement system.
- **25 High partial** items are the reusable platform layer (communications, forms, RBAC, employer/internship patterns, email/calendar, AWS baseline).
- **38 Partial** is the largest group — these drive most “Partially Complies” responses and mid-size delivery work.
- **Low partial + Gap = 28 items (~30%)** — roughly one-third of the schedule has little or no product coverage. These concentrate in placement-core areas (compliance/CESS, eligibility engine, agreements, PRM analytics) and Monash enterprise integrations (uniCRM, middleware, DocuSign).

### Link to the headline metrics

| Metric | How Appendix B relates |
|---|---|
| Functional fit ~39% | Weighted average across mostly Partial / Gap bands on functional IDs |
| Technical fit ~49% | Slightly better because stack/NFR patterns land more in High partial |
| High delivery effort / risk | Driven by the 28 Low partial + Gap items, especially CORE ones |

**Management takeaway:** Appendix B shows a **partial-reuse + large net-new** profile — not a near-fit configuration opportunity.
