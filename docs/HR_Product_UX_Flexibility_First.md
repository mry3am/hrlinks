# HR Automation Platform — UX & Flexibility-First Product Contract

**Purpose:** This document is the mandatory review checklist for the AI development agent **before designing or implementing any module**. Our product succeeds only if a new company can start using it easily, employees can finish tasks without training, and companies can adapt processes without custom code.

## 1. Non-negotiable product promise

A user should not have to understand HR terminology, workflows, or system architecture to complete an HR task. The platform must **do the administrative work**, ask for **only necessary input**, explain **what happens next**, and handle exceptions without leaving people stuck.

**Priority order:** (1) usability and accessibility, (2) flexibility and configurability, (3) correctness, security and compliance — **all three are release gates; none may be traded away**, (4) automation coverage, (5) visual polish. Never automate an incorrect or unauthorized action.

## 2. Who enters the system, and what should they see?

| User | First screen | Primary actions | Avoid |
|---|---|---|---|
| Company owner / setup admin | Guided company setup and readiness checklist | Import people, set locations/policies, assign approvers, activate modules | Empty dashboards, giant configuration forms |
| Employee | Personal home: next shift, attendance, leave, requests, payslips | Check in, request leave, download documents, follow request status | HR jargon, other employees' data |
| Supervisor | Team action inbox and today’s roster | Confirm attendance, approve eligible requests, report exceptions | Payroll-wide access or unnecessary settings |
| Manager | Pending decisions and team overview | Hiring approvals, leave decisions, reviews | Repetitive data entry |
| Finance | Payroll readiness, exceptions and approval queue | Review calculations, resolve flagged issues, authorize payroll | Silent deductions, automatic payment authorization |
| HR/admin operator (optional) | Exception queue and operational overview | Resolve cases, manage records and policies | Being required for every routine action |

One person may have multiple roles; show a clear role/workspace switcher and enforce permissions server-side. Users with no smartphone/email must be supported through biometric capture, supervisor-assisted actions, and printable or alternative notifications.

## 3. First-time company experience: progressive activation

**Goal:** A company should reach its first useful workflow without configuring the entire HRMS.

1. **Create workspace:** Company name, work location(s), business type, owner identity. Save progress.
2. **Choose immediate goal:** e.g., “Track attendance”, “Manage leave”, “Prepare payroll”, or “Onboard employees”. Recommend a starting path; do not force unrelated modules.
3. **Import or add people:** Excel/CSV template, field mapping, preview, duplicate detection, error correction, partial success report, and safe retry. Allow manual entry for small teams.
4. **Set only required policies:** Offer clearly labeled **suggested** defaults by company type and location. Require review for legal/financial rules; never imply unverified defaults are legally compliant.
5. **Assign responsibility:** Pick who approves requests, handles exceptions, and authorizes payroll. Offer safe fallbacks if approver is absent.
6. **Connect tools optionally:** Biometric, email, accounting and other systems. Show “Skip for now”; provide built-in or import fallback.
7. **Preview and test:** Show a sample employee request, its approver path, notifications and effect on records; run a non-destructive test.
8. **Go live selectively:** Activate one module/team/location first; invite people through appropriate channels; display readiness and unresolved blockers.

**Onboarding UX rules:** Step-by-step wizard, clear progress, save/resume, plain-language labels, inline examples, contextual help, import templates, review-before-submit, and ability to edit decisions later with versioned effective dates. No mandatory blank-page configuration.

## 4. Daily experience: one action-first interface

- **Home = “What needs my attention?”** Show next action, deadlines, exceptions, recent completed actions and relevant quick starts.
- **Universal request launcher:** “What do you need?” → search or choose a simple action (leave, certificate, correction, new hire, etc.). Do not make users hunt through modules.
- **One request, one timeline:** Submitted → checks → approval/task → completed, with current owner, next step, expected timing when known, and history.
- **Progressive disclosure:** Ask only fields needed for this employee, company policy and request type; reveal conditional fields only when relevant.
- **Reuse known data:** Prefill employee, manager, location, payroll group and document information; show the source and allow authorized correction.
- **Plain language:** “Request time off” instead of “Leave transaction initiation”; provide bilingual/localized copy where feasible (English and Urdu priorities), with accessible typography and dates/currency appropriate to Pakistan.
- **Consistent patterns:** Same status names, action positions, approval cards, error messages, confirmation screens and audit views across all modules.
- **Responsive and accessible:** Desktop for admins/finance; mobile-first employee/supervisor flows; keyboard navigation, screen-reader labels, clear focus, adequate contrast, large touch targets, no color-only status.
- **Trust:** Explain why a request is blocked, who can fix it, what data is used, and what an action will change. Never show success until confirmed.

## 5. Flexibility model: configurable without custom coding

The company must be able to configure **policies, not program logic**.

**Configuration hierarchy:** Platform safety/legal constraints → company-wide policies → location/branch → department/employee category/payroll group → approved employee-specific exception. Define conflict resolution explicitly; show the effective policy before activation.

**Configurable areas:**
- Organizational structure, branches, departments, reporting lines and delegated approvers.
- Employment types and onboarding/offboarding templates.
- Leave types, eligibility, accrual, balances, holidays and approval paths.
- Fixed/rotating/overnight/flexible shifts, breaks, attendance sources and correction windows.
- Overtime eligibility, authorization and applicable pay rules.
- Monthly/daily/hourly/piece-rate payroll groups, allowances, pay cycles, deductions and finance approval.
- Request forms: optional company-specific fields, required evidence, routing and notifications.
- Role permissions, notification channels, reminder frequency and escalation rules.
- Module enable/disable and staged rollout by branch/team.

**Safe customization requirements:** Template gallery + guided builder; preview on realistic sample employee; validation before publishing; draft/publish; version history; effective dates; impact analysis (“affects 42 employees”); rollback where safe; approval for sensitive changes; audit logs. Existing completed requests and historical payroll retain their original policy version. Statutory protections cannot be disabled through configuration.

## 6. Automation experience: visible, controllable, recoverable

For each request, the engine should:

1. Accept the request or scheduled trigger.
2. Collect existing data; ask for missing information.
3. Check permissions, eligibility, company rules and applicable verified legal constraints.
4. Execute authorized, deterministic actions automatically.
5. Route human decisions/physical tasks to the right person only when necessary.
6. Resume after approval/task completion; notify affected people.
7. Confirm downstream effects, record audit evidence and close the request.

**Four outcomes:** Auto-completed; waiting for approval; waiting for physical/manual action; blocked/needs information. Show which one applies, why, and who owns the next step.

**Automation safeguards:** Idempotent retries, duplicate-request detection, timeouts, escalation, compensating actions where possible, manual fallback for disconnected tools, and a visible failure/retry history. AI may suggest, draft or explain; it must not independently decide hiring, discipline, statutory entitlements or payroll amounts. Finance must authorize payment.

## 7. Exceptions are part of the primary UX, not an afterthought

| Exception | User-friendly behavior | System behavior |
|---|---|---|
| Missing/invalid form field | Inline explanation, preserve entered data | Reject only invalid fields, never lose draft |
| Duplicate employee/import | Show likely matches and safe choices | Prevent accidental duplicate identities |
| Partial import failure | Show successes and downloadable error rows | Retry failed rows without duplicating successes |
| No approver / approver absent | Show who can resolve routing | Delegation/escalation according to policy; no self-approval loophole |
| Policy conflict / missing rule | Explain conflicting settings and affected users | Block sensitive execution until resolved |
| Device offline / missing punch | Show unverified attendance and correction route | Queue/import later; do not silently treat as unpaid absence |
| Overtime not approved | Show pending verification and pay implications | Preserve actual work evidence; apply legal rules |
| Leave balance conflict | Explain balance and alternative request paths | Prevent double consumption and inconsistent records |
| Payroll calculation anomaly | Show line-item explanation and comparison | Block finalization for critical unresolved issues |
| Payroll already approved | Explain adjustment/correction process | Never silently rewrite locked payroll |
| Payment/export failure | Show pending/failed state and responsible owner | Reconcile status, retry safely, prevent double payment |
| Integration disconnected | Explain fallback and reconnect action | Preserve queued work and source provenance |
| Notification undelivered | Show alternative contact/communication route | Retry/escalate without changing request status incorrectly |
| Unauthorized access | Clear, non-revealing denial and support route | Enforce server-side permissions and audit attempts |
| Concurrent edits | Show latest version and safe conflict resolution | Optimistic locking/version checks |
| Network interruption | Preserve drafts and provide retry | Idempotent submission, never double-create |
| Sensitive complaint | Private case intake and limited updates | Strict restricted access, human handling, no public inbox exposure |
| Employee has no device | Supervisor-assisted or kiosk/paper-supported path | Attribute submitter and obtain required confirmation |
| Rule changes mid-process | Explain which version applies | Snapshot rules or apply controlled migration |
| Employee leaves mid-workflow | Explain reassignment/closure | Reconcile open requests, access and final payroll |

## 8. Module design contract — repeat for every feature

Before implementation, the AI agent must produce:

1. **User goal:** What is the user trying to finish in everyday language?
2. **Personas and access:** Who starts, views, approves, edits, and audits it?
3. **Happy path:** Minimum steps from entry to confirmed outcome.
4. **Input contract:** Required vs optional fields, defaults, reused data, validation, supporting documents.
5. **Configuration points:** What varies by company, location, employee group and person?
6. **Automation map:** Which steps execute automatically, which need approval, which need physical work?
7. **State machine:** Draft, submitted, waiting, blocked, failed, completed, cancelled and correction/reopen states as relevant.
8. **Exception matrix:** Missing data, duplicates, offline tools, permission failure, conflicts, timeouts, retries, policy changes, partial success and disputes.
9. **Data and audit effects:** Records read/written, history, versioning, privacy, notifications and downstream dependencies.
10. **Screen flow:** Entry point, empty/loading/error/success states, mobile layout and accessible controls.
11. **Acceptance tests:** Normal case, every high-risk exception, permission tests, retry/idempotency, audit and regression.
12. **Success measures:** Completion rate, time-to-complete, manual touches, error rate, support requests and user comprehension.

**Do not start coding until these twelve items are reviewed for the module.** Reuse shared components and workflows rather than building duplicate approval engines, status systems or forms.

## 9. Prioritized implementation order

**Foundation:** Company setup, authentication, roles, employee records, policies, workflow engine, approvals, audit, notifications, imports, design system.

**First end-to-end demonstration:** Import employees → configure attendance/leave policies → employee request or punch → supervisor exception/approval → verified records → payroll preview → finance review. The first release may stop before live payments until validated.

**Then expand:** Payroll production readiness and legal verification → onboarding/offboarding → recruitment → performance/training/employee services → integrations and advanced AI assistance.

Build vertical slices that a real user can complete, not isolated dashboards.

## 10. Release gates: what “user-friendly and flexible” means

A module is **not done** unless:

- A first-time user can find and complete its primary action without an explanation from a developer.
- Users see only actions and fields relevant to their role and company configuration.
- A company can change ordinary business policies through UI without a code deployment.
- Defaults are clearly suggested, editable and legally checked where required.
- The process survives refresh, duplicate submission, interruption and unavailable integrations.
- Errors say what happened, what is preserved and what the user can do next.
- Requests show their status, current owner and next step.
- Every financial or sensitive change is permission-checked and auditable.
- Automated work is observable; failures do not masquerade as success.
- Mobile and desktop layouts, accessibility, and empty/loading/error states are tested.
- The team has tested with representative office users, supervisors, finance staff and workers with limited digital access.

## 11. Instruction to the AI development agent

> Treat this document as the product UX and flexibility contract. Before building any module, review the actual end-user journey from first login to completion, propose the simplest configurable experience, identify what can be automated safely, enumerate normal and exceptional states, and produce acceptance tests. Challenge unnecessary fields, duplicated workflows, rigid assumptions, and hidden failures. Prioritize a usable complete journey over a large feature list. Do not implement until the module design passes the review checklist above.
