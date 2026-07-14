# NTU Career Services Management (CSM) — Functional Specifications

**Source:** `NTU CSM Doc/` → CSM Product Documentation
**Platform:** NTU Career Services Management (CSM) for Career & Attachment Office (CAO)
**Tags:** C = Core; E = Enhancement (tender Original Requirements)

## 1. Platform Overview

CSM covers SGCC clinics, 1:1 coaching/appointments, jobs & internships (FT, NCB GE, CB WIE), employer management, events, mentorship, forms, communications, dashboards/reports, and workspace settings.

### Roles

| Role | Access | Notes |
|---|---|---|
| Admin | L4 | Full admin, cross-workgroup, settings |
| Manager | L3 | Publish/manage, scoped approvals |
| Staff / Coach / IRC | L2 | Assigned operational work |
| Student / Mentee | L1/L2 | Book, apply, profile, preferences |
| Employer Admin / Employer | Employer L2/L1 | Company profile, postings, applications |
| Mentor | External | Mentorship programmes & connections |
| Internship Supervisor | Time-limited link | Assessment only, no account |
| Faculty Supervisor / School Staff | Scoped staff | Internship oversight / school approval |

### Cross-cutting concepts

- **Workgroups** — module access + shared-record visibility (Forms, Comms, Events)
- **Labels** — attribute-rule segmentation for targeting (Comms, SGCC, Forms)
- **Student Remarks** — immutable global profile notes
- **NIE vs NTU** — primary scope NTU students; NIE PG is edge-case / separate data

---

## 2. Integrations (complete)

| Integration | Direction / use | Modules |
|---|---|---|
| **NTU SSO (+ 2FA)** | AuthN for students/staff/mentees | Platform-wide, Mentorship, SGCC |
| **Employer/Mentor password auth** | Non-SSO accounts; post-approval activation | Employer, Events, Mentorship |
| **NTU SIS / Student Management** | Source student attributes; mentee profile seed; SGCC student list sync | Mentorship, SGCC, Labels/profile |
| **Microsoft Outlook calendar** | Real-time sync of appointments/workshops/sessions; .ics attachments | Coaching, SGCC, Events |
| **Microsoft Teams** | Auto-generate online meeting links | Coaching / IR appointments |
| **Zoom/Teams (mentor meetings)** | Meeting invite links (enhancement) | Mentorship |
| **VMock** | Resume critique workflow sync for SGCC resumes | SGCC |
| **ACRA UEN lookup** | Pre-fill Singapore company details on registration | Employer |
| **SSIC / SSOC** | Industry/occupation coding on employer registration | Employer |
| **Email delivery platform** | System emails, campaigns, alerts, digests | Communications + all modules |
| **WhatsApp / Telegram** | Optional verified alert channels for event matching | Events + Communications prefs |
| **Analytics tool (e.g. Denodo/Qlik)** | BI / analytics integration | Coaching, Dashboards |
| **Document parse (PDF/DOCX)** | Extract job posting fields | Employer / Jobs |
| **CareerAxis migration** | Employer accounts & postings import | Employer |
| **Legacy coach/mentee migration** | One-time data migration | Coaching, Mentorship |
| **NTU Maps venues** | F2F venue catalogue for sessions | SGCC, System Venues |

### Architecture / security notes

- **Production:** `csm.ntu.edu.sg` in `ap-southeast-1` (Singapore); frontend/backend as nx + pnpm monorepos under `apps/ntu`.
- **Backend stack:** NestJS 10 (Node/TypeScript), PostgreSQL 14 via Drizzle ORM, Zod validation, AWS Cognito auth, S3, SES email, Secrets Manager, CloudWatch; OpenAI used for feedback summaries/sentiment; templated email + `.ics` calendar invites.
- **Security:** RDS encrypted at rest, Multi-AZ, private network + bastion, 30-day automated backups, deletion protection, credentials in Secrets Manager.

---

## 3. SGCC (Small Group Coaching Clinics)

Standalone module: bundled **Resume Critique** + **Mock Interview** sessions.

### Functional items

- Session statuses: Draft, Published (Scheduled/Ongoing/Completed), Withdrawn (bookings retained), Cancelled (bookings released; restorable).
- Default bundled booking; unbundled only for defined absence/edge cases.
- Admin session creation: manual, CSV bulk (~1,200/semester), ad hoc makeup.
- Session fields: type, date/time (presets + custom), venue (NTU Maps or online URL), multi-coach, capacity (global settings), publish date, eligibility filters, listed/unlisted.
- Edit rules by role/status; conflict checks (coach, venue, weekends/holidays, semester).
- Optional enrolled-student email + refreshed .ics on edits.
- Immutable session edit audit trail.
- Bulk publish and bulk capacity with delta confirmation.
- Student browse/book by eligibility; reschedule/cancel rules; attendance confirmation.
- Absence flows differ for Session 1 vs Session 2; manager approval for some reschedules.
- Resume upload with deadline; VMock sync to CSM.
- Coach attendance marking, grading (5/0), remarks, resume download.
- Automated emails across booking lifecycle (confirmations, reminders, changes, absences).
- Post-session feedback forms (Resume Critique + Mock Interview).
- SIS student list sync; venue management; student activity log; CSV re-upload for published sessions.
- Settings & configuration; reporting/analytics (incl. CP3 enhancements).
- Admin capacity override / escalations; coach assignment.

---

## 4. Coaching & Appointments

1:1 Career Coach / IR Consultant appointments; recurring availability; approval booking; notes; calendar sync.

- **6.1.1.1.4** [C]: C&C Profile & Availability Setup Workflow (refer to Annex A Appendix 1 PDF)
- **6.1.1.1.5** [C]: IR Consultant / Career Coach Session Workflow (refer to Annex A Appendix 1 PDF)
- **6.1.1.1.6** [C]: Appointment Cancellation Workflow (refer to Annex A Appendix 1 PDF)
- **6.1.1.2.1** [C]: Student filters by appointment type (Coach or IR Consultant) and selects a specific coach/consultant via dropdown or search to view available timeslots
- **6.1.1.2.3** [C]: System displays a conflict prompt if the selected timeslot overlaps with existing appointments
- **6.1.1.2.6** [C]: Approved appointments are flagged in the student's calendar view
- **6.1.1.5.1** [C]: Student filter function: select type (Career Coach or IR Consultant) and specify a coach/consultant via dropdown or search
- **6.1.1.5.2** [C]: System displays all available timeslots of the student's designated coach(es) / IR Consultant(s) in a calendar view
- **6.1.1.6.1** [C]: Approval process: Career Coaches and IR Consultants review and accept student appointment bookings
- **6.1.1.6.2** [C]: System displays a prompt when the selected timeslot conflicts with existing appointments
- **6.1.1.6.3** [C]: System supports configurable criteria enabling students to request or book multiple 1:1 sessions
- **6.1.1.2.2** [C]: Approval process: IR Consultants approve or accept student appointment requests
- **6.1.1.2.4** [C]: Email notification triggered to IR Consultant on student appointment request
- **6.1.1.2.5** [C]: Email confirmation to student on approval or rejection; for Online/MS Teams appointments, a Teams meeting link is included
- **6.1.1.2.7** [C]: Real-time synchronisation with external calendars (Microsoft Outlook)
- **6.1.1.2.8** [C]: IR Consultants specify appointment type (Online / Face-to-Face); for F2F, venue selected from a predefined dropdown visible to students
- **6.1.1.2.9** [C]: Free-text notes field for IR Consultants; visible only to other IR Consultants with appropriate access rights, not to students
- **6.1.1.2.10** [C]: Backdated appointments supported based on role control; manual check-in and check-out time updates allowed
- **6.1.1.2.11** [C]: System supports creation of IR Consultant accounts and one-time data migration for existing IR Consultants with all appointment information
- **6.1.1.4.2** [C]: For Online/MS Teams appointments, system automatically generates a Teams meeting link and includes it in the notification
- **6.1.1.4.3** [C]: Backdated appointments supported based on role permissions; manual updates of check-in and check-out times allowed
- **6.1.1.4.4** [C]: System supports cancellation of approved appointments by student, Coach, or IR Consultant; cancellation email sent to all parties; appointment removed from calendar
- **6.1.1.3.1** [C]: System supports creation of new Career Coach accounts and completion of their profiles
- **6.1.1.3.2** [C]: One-time data migration for existing Career Coaches and IR Consultants (accounts, profiles, appointment history)
- **6.1.1.3.3** [C]: Career Coaches set up recurring appointments for a specified date range, with flexibility for different weekly schedules within that range (e.g., every Monday at 9:30 AM for Jan–Mar)
- **6.1.1.3.4** [C]: Career Coaches choose recurring appointment type: Daily, Weekly, Monthly, Yearly, or Custom
- **6.1.1.3.5** [C]: Career Coaches input their available slots; students can only view slots for their designated coach(es), not all coaches
- **6.1.1.4.1** [C]: System supports creation of new Career Coach accounts and completion of their profiles
- **6.1.1.8.1** [C]: Admin assigns one or more coaches to a specific school for a defined period; in case of absence, admin assigns a covering coach; coaches overseeing multiple schools can have more than one covering coach assigned
- **6.1.1.7.1** [C]: System captures check-in (session start) and check-out (session end) timestamps for the coach in Online mode
- **6.1.1.7.2** [C]: For Face-to-Face sessions: coach has a Check-In / Check-Out button; timestamps are editable by coaches when needed
- **6.1.1.11.1** [C]: Email triggered to Career Coach / IR Consultant for approval on student appointment request; template configurable
- **6.1.1.12.1** [C]: On coach approval: email confirmation triggered to both coach and student; template configurable
- **6.1.1.12.2** [C]: Approved appointment flagged in student's Calendar View
- **6.1.1.12.3** [C]: Real-time synchronisation with external calendar applications (Microsoft Outlook)
- **6.1.1.13.1** [C]: Configurable reminder: 'x' days or 'x' hours before appointment (not a fixed interval); template configurable
- **6.1.1.16.1** [C]: Email confirmation triggered to students on workshop signup with calendar flag; template configurable
- **6.1.1.16.2** [C]: Signed-up workshop flagged in student's Calendar View
- **6.1.1.16.3** [C]: Real-time Outlook calendar sync for workshop signups
- **6.1.1.17.2** [C]: Configurable reminder setting: 'x' hours/days; multiple reminders supported
- **6.1.1.20.2** [C]: System includes a configuration for students to set their mailing preferences
- **6.1.1.27.2** [C]: System supports integration with an Analytics Tool
- **6.1.1.32.2** [E]: System includes a chatbot feature to handle queries
- **6.1.1.94.1** [C]: Free-text field for coach to update student coaching notes per session; notes not viewable by students but viewable by other coaches with access rights
- **6.1.1.95.1** [C]: System supports journal entries for all admin users (view, create, edit)
- **6.1.1.98.1** [C]: Report for coached students: First/Last Name, School, NTU + personal email, Matric No., handphone, nationality, gender, degree, year of study, graduation month/year, mode of delivery, consultation date, start/end time, coaching notes, appointment status, session length, coaching type, coach name, cancellation reason, student request type
- **6.1.1.100.1** [C]: Report for workshop attendees: First/Last Name, School, emails, Matric No., handphone, nationality, gender, degree, year of study, graduation month/year, mode of delivery, workshop date, start/end time, workshop name/type/topic, venue, description, attachment, AV requirements, RSVP dates, slots available, waitlist flag, coach, and more
- **6.1.1.101.1** [C]: Report for briefing attendees: First/Last Name, School, emails, Matric No., handphone, nationality, gender, degree, year of study, graduation month/year, mode of delivery, briefing date, start/end time, briefing title
- **6.3.1.4.1** [E]: Students can book online meetings with mentors; mentors can accept or reject the meeting invite
- **6.3.1.4.2** [E]: Meeting invite auto-generates a meeting link; Zoom/Teams links can be included

---

## 5. Jobs & Internships

Covers Full-Time Job Boards, Non-Credit Bearing (GE) internships, and Credit-Bearing (WIE) internships — employer/admin and student workflows.

- **6.1.1.1.2** [C]: Opportunities – Job Approval Workflow (refer to Part 2 Annex A Appendix 1 – Opportunities – Job Approval Workflow.pdf)
- **6.1.1.92.1** [C]: Email trigger to notify IR Consultants for approval of new job postings and any amended postings submitted by employers. Trigger can be case-by-case or by batch.
- **6.1.1.91.1** [C]: System supports overseas employer postings with student-facing filters (programme, city, country, business area, position type). Application tracking equivalent to local postings. ⟳
- **6.1.1.91.2** [C]: CAO staff can post jobs on behalf of overseas employers.
- **6.1.1.91.3** [C]: Overseas employers can track student application records and view application statuses.
- **6.1.1.93.1** [E]: Batch download of students' resumes from job applications.
- **6.1.1.129.1** [C]: System collects students' CVs/Resumes and compiles them into a CV/Resume book.
- **6.1.1.97.1** [C]: Retrieve stats: new roles (FT and internship) by industry/employer; new employers by industry; employer events and vacancies promoted via events; CAO events with employer participation.
- **6.1.2.1.1.a** [C]: Report: List of Job & Internship Opportunities (industry, employer, job details).
- **6.1.2.1.1.b** [C]: Report: List of Employers with Job & Internship Opportunity Postings (industry, employer details, contact details, approval date).
- **6.2.1.1.1** [C]: GE – Non-Credit Bearing Self-Source Internship Placement Workflow (refer to Part 2 Annex A Appendix 1)
- **6.2.1.1.2** [C]: GE – Overseas Internship Onboarding Process (OOP) Workflow (refer to Part 2 Annex A Appendix 1)
- **6.2.1.14.1** [C]: Employer account sign-up for internship posting, with approval process and status-tracking dashboard. ⟳
- **6.2.1.13.1** [C]: IR Consultants and employers (by role access) can view posted jobs, track posting statuses, and access secured intern information. ⟳
- **6.2.1.14.2** [C]: Employers can view the status of student internship applications.
- **6.2.1.16.1** [C]: System supports overseas employers managing internship opportunity postings. ⟳
- **6.2.1.16.2** [C]: CAO staff can post internships on behalf of overseas employers. ⟳
- **6.2.1.5.8** [C]: Users can update overseas placement details in the system, linking them to the student's application. ⟳
- **6.2.1.5.3** [C]: Bulk download of all applications for the Internship/Work Study Programme. ⟳
- **6.2.1.5.4** [C]: System indicates Eligible/Ineligible status; supports interview invitation, result updates, and offer/no-offer decisions. ⟳
- **6.2.1.5.5** [C]: System generates Offer Letter on acceptance, attached to acceptance email. Notifies student of rejection. ⟳
- **6.2.1.17.7** [C]: NCB self-sourced placements: student updates internship details without approval. System auto-generates Placement Letter. If employer is new, WIE team notified; WIE Administrator creates and approves employer account if needed. ⟳
- **6.2.1.17.8** [C]: Student placement records (CB and NCB) and application statuses visible to employers. ⟳
- **6.2.1.15.1** [C]: Employer inputs Employability Potential rating during evaluation. Upon graduation, student is notified of FTP opportunities with the same employer. ⟳
- **6.2.1.15.2** [C]: Employer is notified of interns previously flagged as interest-to-hire for FTP.
- **6.2.1.15.3** [C]: Employer can offer interns FTP conversion upon graduation.
- **6.2.1.10.2** [C]: Feedback results and ratings captured and stored for analytical purposes. ⟳
- **6.2.1.11.1** [C]: Users can manually trigger or auto-send Programme Evaluation emails based on configurable criteria. ⟳
- **6.2.1.11.2** [C]: Users can download student feedback results. ⟳
- **6.2.1.12.2** [C]: Customisation of financial support application forms. ⟳
- **6.2.1.25.1** [C]: Stats: new roles by industry/employer; new employers by industry; employer events; CAO events with employer participation. ⟳
- **6.2.1.26.1** [C]: Internship Performance Report generation with configurable fields and customisable templates. ⟳
- **6.2.1.26.2** [C]: Users can view and print the Internship Performance Report (by access rights: Student, CAO Coordinator, Career Coach). ⟳
- **6.2.1.27.1** [C]: Reporting & Dashboards — refer to Section 6.2.3 of the CSM Functional Specifications document. ⟳
- **6.2.1.1.3** [C]: WIE – Credit-Bearing Internship Opportunities Posting Approval Workflow (refer to Part 2 Annex A Appendix 1)
- **6.2.1.1.4** [C]: WIE/GE – Credit-Bearing Self-Source Internship Placement Workflow (refer to Part 2 Annex A Appendix 1)
- **6.2.1.1.5** [C]: WIE/GE – Credit Bearing Student Internship Placement Workflow (refer to Part 2 Annex A Appendix 1)
- **6.2.1.17.1** [C]: WIE team configures Opportunities Campaign parameters; sends targeted emails to specified student groups; supports bulk upload of targeted student lists for distribution.
- **6.2.1.17.2** [C]: System triggers email inviting employers to participate in the campaign; invited employer group is configurable.
- **6.2.1.17.3** [C]: Employers post opportunities during the campaign window before public release to students. ⟳
- **6.2.1.17.5** [C]: Users can update Faculty Supervisor before internship start. If not updated by start date, system emails school to prompt update. Application rejection triggers notifications to student, employer, and WIE team.
- **6.2.1.17.6** [C]: CB placements: system auto-generates Internship Placement Letter upon approval and emails to student and employer. Downloadable from portal.
- **6.2.1.2.2** [C]: View consolidated student placement information and download placement letter upon approval/allocation (Edge-Plus context). ⟳
- **6.2.1.2.3** [C]: System triggers email on placement confirmation and notifies employer to update placement details.
- **6.2.1.18.1** [C]: System supports AWO ratings; employers input ratings; system calculates final AWO score. Score below threshold triggers notification to school and student. System also notifies school and student if student fails to meet placement criteria (Placement at Risk).
- **6.2.1.21.1** [C]: Assessment types: Journal (non-graded), Journal & Presentation (Graded), Journal & Presentation (Edge-Plus), AWO. Refer to Worksheet NITR15 (A, B, C) & NITR15 (D). ⟳
- **6.2.1.21.3** [C]: Faculty Supervisors assess student submissions, assign ratings, and provide feedback accessible by students.
- **6.2.1.21.4** [C]: Users can create, modify, and publish assessment templates; create configurable assessment survey types.
- **6.2.1.22.1** [C]: Faculty Allocation modes: School Assigned and Faculty Supervisor Selects. Refer to Worksheet NITR14.
- **6.2.1.22.2** [C]: CAO staff and School can set allocation loads, assign faculty supervisors (individually or in bulk), view allocation progress, check supervisor availability, and monitor remaining capacity in real time.
- **6.2.1.22.3** [C]: Faculty Supervisors can self-select supervision assignments based on configurable criteria.
- **6.2.1.2.1** [C]: NTU Edge-Plus: refer to Worksheet ITR09 (Opportunities Management) and NITR15 (A, B, C) (Assessment) for full functional details.
- **6.2.1.2.4** [C]: Edge-Plus: only the nominated team leader can submit the Final Report on behalf of the team. System marks other members as completed once leader submits. Relevant email notifications triggered throughout. ⟳
- **6.2.1.3.1** [C]: WSDEG: configurable multi-block internship structure (e.g., Block 1 PA 10 wks, Block 2 PI 20 wks, Block 3A 11 wks / 3B 5 wks). Refer to Worksheet ITR10.
- **6.2.1.3.2** [C]: System auto-assigns students to three placement blocks under WSDEG Employer and tracks progress block-by-block.
- **6.2.1.23.2** [E]: System notifies student, school, employer, and WIE team (where applicable) of leave application outcome. ⟳
- **6.2.1.23.3** [E]: System tracks student attendance during internship including absences; auto-calculates attendance percentage.
- **6.2.1.23.4** [E]: CB: system auto-adjusts internship period based on leave taken, per defined leave type rules.
- **6.2.1.24.2** [C]: Mark Moderation field for faculty input and score updates after marks release.
- **6.2.1.19.2** [E]: WIE Administrator can update student internship details; depending on approval level, changes may require school forwarding.
- **6.2.1.19.3** [E]: System triggers email notification once a change request is approved. ⟳
- **6.1.1.90.1** [C]: Students set up job search notifications; system notifies students of new postings matching their criteria or interests.
- **6.1.1.90.2** [C]: Students can enable or disable job search notifications.
- **6.1.1.90.3** [C]: Notification frequency customisable (e.g., daily or weekly).
- **6.1.1.124.1** [C]: Students can bookmark industry or company search results as favourites on the portal.
- **6.1.1.89.1** [C]: Students upload resumes for job applications and status tracking. Applications submitted directly through the system. Statuses tracked (Applied, Rejected, Offered, etc.) with synchronisation to employer-visible status.
- **6.2.1.5.2** [C]: Students can apply for overseas internship placements. ⟳
- **6.2.1.5.6** [C]: Students can accept or reject offers; application statuses tracked (Applied, Invited, Offered, Accepted, Rejected). ⟳
- **6.2.1.5.7** [C]: Students indicate acceptance/rejection and upload signed offer letter, with optional remarks or reasons for rejection. ⟳
- **6.2.1.19.1** [E]: Students submit change requests to modify specific placement fields. Changes routed to School/WIE Administrator for approval. ⟳
- **6.2.1.10.1** [C]: Students provide feedback on internship experience upon completion. System notifies and sends reminders. Feedback form is customisable. ⟳
- **6.2.1.12.1** [C]: System includes financial support application forms; students apply directly and receive status updates in the system. ⟳
- **6.2.1.17.4** [C]: Students apply for internship opportunities. System performs eligibility check; if eligible, application routed to school for CB approval. If ineligible, student is prompted on screen.
- **6.2.1.28.1** [E]: System includes Student Skills Tracking feature; matches students' skills with recommended internship opportunities.
- **6.2.1.28.2** [E]: System displays skill match percentage indicator during internship application based on skills selected.
- **6.2.1.28.3** [E]: System uses AI to auto-populate relevant skills, or allows students to manually input skills.
- **6.2.1.21.2** [C]: Students view, enter, edit, and submit journals, presentations, and reports. System tracks progress and sends confirmation/reminder emails.

### Additional Jobs/Internship capability themes (from product docs)

- Job approval workflow; IRC email on new/amended employer postings.
- Overseas postings with student filters; CAO can post on behalf; employer application tracking.
- Student job alerts (enable/disable, daily/weekly); bookmarks; in-system apply + status sync.
- CV/Resume book compilation.
- NCB self-source placement + auto Placement Letter; new employer triggers WIE onboarding.
- CB campaign windows; targeted student emails; employer early-post before public release; school eligibility checks.
- Offer letter generation; accept/reject + signed letter upload.
- Internship-to-FTP conversion / interest-to-hire.
- Assessments: journals, presentations, Edge-Plus team leader final report; leave management (E).
- Skills tracking + AI skill match % (E).
- Financial support application forms; programme evaluation feedback.
- Internship Performance Report (configurable; role-based print/view).

---

## 6. Employer Management

- **Registration:** CareerAxis migration onboarding; self-registration join-existing vs new-company wizard.
- **ACRA UEN verify** pre-fills company fields (editable); overseas company path without UEN.
- IRC auto-assign on registration; mandatory industry; designation types.
- Company profile, contacts, addresses; Email Subscribers flag; inactive contact auto-deactivation.
- Import non-account contacts for mass email; bulk label/search.
- Blacklist / watchlist.
- Job create methods: bulk CSV, manual, duplicate previous; PDF/DOCX parse; Publish Date/Expiry; CAO approval; scheduled auto-publish.
- Employer events: recruitment talks/roadshows; career fair registration forms.
- Mobile-friendly employer portal.
- Application views by role; batch student resume download (E).
- Internship supervisor time-limited assessment link (no persistent account).
- Faculty supervisor / school staff scoped visibility & approvals.
- Employer dashboards: internship history, tracker, FTP conversion signals.
- Employer status reporting.

### Tender refs (Employer)

6.1.1.1.1, 6.1.1.9.1, 6.1.1.35.1–35.9, 6.1.1.36.1–36.11, 6.1.1.37.1–37.7, 6.1.1.38.1, 6.1.1.39.1, 6.1.1.40.1, 6.1.1.41.1–41.2, 6.1.1.42.1, 6.1.1.43.1, 6.1.1.44.1, 6.1.1.45.1, 6.1.1.46.1, 6.1.1.47.1, 6.1.1.91.1–91.3, 6.1.1.92.1, 6.1.1.93.1, 6.1.2.1.2.a, 6.2.1.7.1–7.2, 6.2.1.8.1, 6.2.1.9.1, 6.2.1.15.1–15.3

---

## 7. Events

- Attach pre-screening form (template or inline); required or optional.
- Optional approval gate: Registrant Submitted → Approved/Rejected → Participant; bulk approve/reject with optional reason.
- Non-NTU open events: invite registrant to create Employer or Mentor account; fuzzy company match; pending approval.
- Student interest tags (Industry / Function / Topic) + admin event tags; match engine; alerts or digest.
- Alert channels: email default; WhatsApp/Telegram if verified in preferences; rate limits; suppression/blacklist.
- Event feedback forms with SSO auto-metadata; public-event metadata UX TBD.
- Participants attendance marking separate from Registrants list.

### Tender refs

6.1.1.20.1, 6.1.1.20.2, 6.1.1.24.1, 6.1.1.31.1, 6.1.1.31.2, 6.1.1.127.1

---

## 8. Mentorship

- **6.3.1.5.1** [C]: Admin can set up mentorship programmes for both mentors and mentees
- **6.3.1.7.1** [C]: System auto-generates and sends e-certificates to programme participants
- **6.3.1.8.1** [C]: Mentors and mentees see a feed of all upcoming programmes in the system
- **6.3.1.8.2** [C]: Programme feed viewing restrictions are configurable by specified parameters
- **6.3.1.9.1** [C]: Students register as Mentee, create a profile with image, edit skills / work experience / career interests
- **6.3.1.9.2** [C]: System retrieves name, programme, and year of study from NTU Student Management (SIS) at registration
- **6.3.1.9.3** [C]: Mentees can add personal information beyond what SIS provides
- **6.3.1.9.4** [C]: Existing mentee data migrated from legacy system with data integrity preserved
- **6.3.1.10.1** [C]: Mentees can upload and download resumes in PDF/Word format
- **6.3.1.11.1** [C]: Mentees can search for mentors and send connection requests
- **6.3.1.11.2** [C]: Mentees can browse mentor profiles using filtered criteria
- **6.3.1.12.1** [C]: Mentees can view and register for upcoming events and programmes
- **6.3.1.13.1** [E]: Mentees can upload a Video CV to their profile for mentors to view
- **6.3.1.13.2** [E]: Mentees can record a Video CV within the platform
- **6.3.1.14.1** [C]: Mentors and mentees can specify preferences for matching
- **6.3.1.14.2** [C]: System uses AI to recommend mentors based on mentee's course of study, interests, and skills
- **6.3.1.15.1** [C]: Alumni and industry professionals can register as mentors
- **6.3.1.15.2** [C]: Admin approval workflow for new mentor accounts (approve / reject)
- **6.3.1.15.3** [C]: Mentees and admins can search approved mentors only; unapproved mentors are not searchable
- **6.3.1.16.1** [C]: Mentors create and edit profiles (image, personal info, skills, work experience, career interests)
- **6.3.1.17.1** [C]: Mentees submit connection requests to mentors
- **6.3.1.17.2** [C]: Mentors view mentee profiles and accept or decline connection requests
- **6.3.1.18.1** [C]: Mentors view all their connection requests with statuses (approved / pending)
- **6.3.1.19.1** [C]: Mentors can view and register for upcoming events and programmes
- **6.3.1.21.1** [C]: Mentees register via SSO (NTU Single Sign-On)
- **6.3.1.22.1** [E]: System tracks graduating students and auto-triggers an email invitation to sign up as mentors; trigger criteria and template are admin-configurable
- **6.3.1.23.1** [C]: Admin extracts mentor details (name, email, designation, company) and mentee details (name, email, programme, year of study)
- **6.3.1.23.2** [C]: Admin dashboard: real-time metrics on applications, rejections per user, successful matches, and interaction frequency between matched pairs
- **6.3.1.23.3** [C]: Admin analytics on matching outcomes and participation rates (applications, rejections, post-match engagement)
- **6.3.1.24.1** [C]: Admin dashboard overview: total mentees, mentors, connections, and programmes
- **6.3.1.24.2** [C]: Admins generate customised reports with configurable filter criteria
- **6.3.2.1.1.a** [C]: Report: list of mentoring programmes (type, details, period, participant details)
- **6.3.2.1.1.b** [C]: Report: total programmes and participant count per programme
- **6.3.2.1.2.b** [C]: Mentees submit ratings and testimonials of their mentor; admin sees collated results
- **6.3.2.1.3.a** [C]: System auto-triggers an outcome form to mentees 6 months post-programme to track internship/job success; responses collated to admin

---

## 9. Forms

- Form lifecycle: Draft → Published → Withdrawn → soft-Deleted (60-day restore).
- SGCC CP2: two authenticated feedback forms (Resume Critique, Mock Interview).
- Builder types: date, single-select, Likert table scale, free text (+ broader types for events).
- Workgroup scoping; RBAC for publish/withdraw/override.
- Share via slug URL + QR; inactive when withdrawn.
- Response audit trail; Admin response override retains history.
- Analytics charts + CSV export.
- Event pre-screen & feedback templates; campaign-embedded polls/charts (with Communications).

---

## 10. Communications

- System emails for user lifecycle triggers (active/inactive badge, timing/receiver labels).
- Campaigns from templates: Segment / List / Ad-hoc recipients; send now / schedule / recurrence (SGT).
- Manager limited to immediate sends; Admin schedules & recurrence.
- Recipient Excel/CSV import with reusable column mappings; named lists.
- Segmentation by programme/school/year/grad year/role/registration date, etc.
- Contact hygiene: suppression & blacklist.
- Event alert + digest preferences; job alert delivery.
- AI-assisted email authoring.
- Configurable action digests; notification trigger governance.
- Communication preferences framework; unsubscribe / suppression.
- Interactive polls and charts in emails; audience labels.
- Reports: email report, student segment export, per-campaign performance dashboard.

---

## 11. Dashboards & Reports

- SGCC analytics & reporting dashboard (first instance; pattern may generalise).
- Cross-module operational reports (jobs, employers, coaching attendees, workshop/briefing attendees, internship performance).
- Exportable datasets for analytics tools.


---

## 12. System / Workspace Settings

- Work Groups: module-level access + record sharing for platform modules.
- Venues catalogue (name, building, level, room, category, type, capacity) with filters.
- Labels: declarative attribute rules (AND/OR) auto-assign users; used by Comms/SGCC/Forms.
- Student Remarks: global immutable timestamped notes.
- Concurrent edit locking.
- RBAC matrix for workgroups/venues/labels (Admin/Manager/Staff).

---

## 13. NIE vs NTU Context

Students: Context Background NIE (National Institute of Education) is an autonomous institute within the NTU family. While it operates under NTU's umbrella, it has its own governance structure and academic identity. As you heard earlier, this distinction carries through to how student data is managed, with NIE and NTU student records sitting in separate tables with only limited syncing between them. Who NIE students are NIE is NTU's teacher education institute. At the undergraduate level, NIE students are training to be teachers. This means they go straight to MOE (the Ministry of Education) after graduation, so career services don't apply to them. Therefore, NIE undergrads are excluded from CAO's remit. Why NIE…

---

*Requirement IDs harvested: Coaching 50, Jobs/Internships 79, Mentorship 35. SGCC/Employer/Events/Forms/Comms enumerated as capability lists from product docs.*

*End of NTU CSM functional specifications.*