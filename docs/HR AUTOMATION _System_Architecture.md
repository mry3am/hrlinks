# HR Automation OS — Deep System Architecture

*Detailed architecture and exception-complete design diagrams | Design baseline v1.1*

> Converted from PDF. Diagram-heavy pages retain extracted captions and sequence notes; visual diagrams themselves are not embedded.

## Page 1

HR AUTOMATION OPERATING SYSTEM
Detailed architecture and exception-complete design diagrams | Pakistan-focused, self-contained multi-tenant SaaS |
Design baseline v1.1
Scope: 14 functional modules; request-driven and scheduled automation; attendance and payroll; employee lifecycle; finance-controlled disbursement; optional integrations. This document expands the previously supplied seven-page product blueprint into implementable diagrams. Proposed entity relationships and interfaces are design recommendations, not claims of an existing implementation.
Diagram index
Family
Coverage
High-level architecture
System context, deployment, modular monolith, workflow control, trust boundaries UML sequences Request engine, onboarding, attendance import/correction, leave/overtime, payroll, payment, offboarding, failures Data flow diagrams Context DFD, Level 1, Level 2 attendance, payroll, employee lifecycle ER diagrams Organization/identity, workflow, time/payroll, lifecycle/integrations Controls Exception catalog, invariants, state transitions, traceability and validation Notation: blue arrows are normal exchanges; red sequence messages marked ! are alternate, blocked or failure paths. DFD cylinders are stores, rectangles are external actors, rounded boxes are processes. ER labels use PK/FK, 1:N and effective-dated relationships.

## Page 2

A1. System context and trust boundary
External actors and integrations cannot bypass tenant authorization, policy checks or durable workflow execution.

## Page 3

A2. Runtime and deployment architecture
Proposed deployment: one application with well-defined modules, separate durable workers, and protected data stores.

## Page 4

A3. Request orchestration and failure containment
A request is never marked complete merely because an external command was sent.

## Page 5

S1. UML sequence — generic request and approvals
UML alternatives shown explicitly: unauthorized, missing data, rejection, approval timeout, and action failure.
Requester
API
Workflow
Policy
Approver
Worker
Submit request + idempotency key
Check tenant, role, eligibility, rule version
Decision + required evidence
Persist request and event atomically
Approval task + deadline
Approve / reject / delegate
Execute approved actions
Confirmed result + evidence
Completed status + notifications
! Invalid or unauthorized -> 403 / needs info
! Timeout -> escalation / substitute
! Transient failure -> retry / DLQ
! Rejected / blocked / recoverable failure

## Page 6

S2. UML sequence — hire, onboard and probation
Manager
Hiring
Employee
Workflow
IT/Admin
Approver
Approved requisition or external hire
Convert candidate / dedupe identity
Create draft profile + type-specific checklist
Offer / contract approvals
Assign device, badge, accounts
Confirm asset / provisioning evidence
Activate on verified joining date
Schedule probation review
! Duplicate candidate -> merge review
! Offer rejected -> stop activation
! Account API fails -> manual task
! Joining date changed -> reschedule
! Probation overdue -> escalate

## Page 7

S3. UML sequence — biometric/import attendance
Device/CSV
Adapter
Time Service
Roster
Supervisor
Audit
Deliver punches + source metadata
Validate schema, tenant, employee map
Resolve shift / overnight boundaries
Store raw immutable events
Deduplicate + reconcile punches
Open missing punch / conflict task
Verify correction + evidence
Append adjustment + provenance
Publish verified payable time
! Unknown employee -> quarantine
! Repeated file -> idempotent skip
! Roster conflict -> hold calculation
! Device offline -> sync gap alert
! Dispute -> contested, no silent deduction

## Page 8

S4. UML sequence — leave, overtime and pay inputs
Employee
Manager
Time/Leave
Rules
Payroll
Audit
Leave / overtime request
Check balance, roster, eligibility
Route approval when required
Decision + rationale
Version event and audit
Publish approved time/pay component
! Insufficient balance -> exception
! Approval overdue -> escalate
! Cancel leave -> reverse with audit
! Overtime unapproved -> hold/review
! Legal entitlement conflict -> block

## Page 9

S5. UML sequence — payroll calculation and finance
Scheduler
Payroll
Time/Output
Rules
Reviewer
Finance
Open period / snapshot policy versions
Fetch verified hours, leave, pieces
Calculate earnings, tax, deductions
Explainable component ledger
Validate floors, overlaps, variances
Review exceptions + payroll register
Approve reviewed payroll
Request independent authorization
Authorize / reject release
Lock authorized version
! Missing punch / piece proof -> hold
! Missing legal rule -> block
! Duplicate membership / invalid rate
! Self-approval conflict -> deny
! Post-lock edit -> adjustment run

## Page 10

S6. UML sequence — payment and reconciliation
Finance
Payroll
Bank/Export
Reconciler
Employee
Audit
Authorize payment batch
Prepare signed export / bank request
Receipt / bank reference
Poll/import settlement status
Reconcile by employee and amount
Record confirmation + immutable trace
Release confirmed payslip
! Timeout / unknown status -> do not resend
! Partial payment -> isolate failures
! Mismatch -> investigation + hold
! Reissue only with verified no-duplicate
! Payment failed -> status notice

## Page 11

S7. UML sequence — offboarding and settlement
Manager
HR Service
Workflow
IT/Assets
Payroll
Finance
Submit resignation / separation event
Verify authority, dates, notice
Assign clearance and revoke access
Confirm assets and accounts
Compute final pay, leave, recoveries
Send settlement for approval
Authorize final settlement
Close employment / archive records
! Disputed separation -> human review
! Asset missing -> tracked unresolved item
! Final attendance dispute -> hold
! Rejected settlement -> recalculate
! Revocation fails -> security escalation

## Page 12

S8. UML sequence — integration outage and recovery
External
Adapter
Queue
Workflow
Operator
Audit
! API timeout / malformed payload
Persist error + correlation key
Retry exponential backoff
Re-execute idempotent action
Verified success -> continue
! Unknown outcome -> reconcile first
! Max retries -> dead-letter alert
Manual fallback with evidence
Audit resolution + responsible actor
No duplicate payments / records

## Page 13

D0. Context data flow diagram
DFD context: all external exchanges pass through the platform boundary.

## Page 14

D1. Level 1 data flow — major processes and stores

## Page 15

D2. Level 2 DFD — attendance, roster, leave and overtime

## Page 16

D3. Level 2 DFD — payroll and disbursement

## Page 17

D4. Level 2 DFD — hiring to exit

## Page 18

E1. ER diagram — tenancy, organization and identity
All tenant-owned records carry tenant_id even where omitted from the diagram for readability. Cross-tenant foreign keys are prohibited.

## Page 19

E2. ER diagram — requests, approvals and reliable actions
Approvals, action attempts and audit events are append-only; request state uses optimistic concurrency/version checks.

## Page 20

E3. ER diagram — time, payroll and payments
Payroll line references immutable calculation input snapshots and rule versions; payment retries cannot create duplicate disbursements.

## Page 21

E4. ER diagram — hiring, employee lifecycle, files and integrations Case confidentiality uses additional access scopes; external sync records retain source identity, import checksum and reconciliation result.

## Page 22

X1. Exceptional handling — exhaustive operational categories
Each exception must have a detectable trigger, explicit state, responsible owner, safe recovery and audit evidence. The following catalog covers the specified workflows and major cross-cutting failures;
individual implementations must extend it as integrations and jurisdictions are validated.
Domain
Trigger / failure
Required response
Identity / tenant
Unknown user, role revoked, cross-tenant ID, expired session
Reject before read/write; security audit; MFA/reauth where required Requests / concurrency Duplicate submit, stale version, two approvers acting Idempotency key, optimistic lock, one terminal decision Approval Reject, timeout, absent approver, delegation, conflict of interest Escalate or reassign within policy; never self-authorize Employee import Duplicate employee, missing identifier, inconsistent salary Quarantine row; preview diff; explicit merge/override approval Recruitment Duplicate candidate, expired offer, changed joining date Human merge; offer versioning; reschedule pending tasks Onboarding Missing document, unsigned contract, IT provisioning failed Keep blocked/pending; manual task and evidence; no premature activation Attendance ingest Device offline, malformed file, unknown employee, timezone drift Quarantine, gap alert, retry; source integrity retained Attendance reconcile Missing punch, duplicates, overnight mismatch, roster change No silent deduction; review task; versioned correction

## Page 23

X1. Exceptional handling — continued
Domain
Trigger / failure
Required response
Leave
Negative balance, overlapping requests, paid/unpaid conflict
Rule validation, human exception, payroll hold if material
Overtime
Unapproved extra time, rest-day work, legal entitlement conflict Review work evidence; preserve statutory entitlements Piece rate Unverified output, rejected pieces, incorrect rate, wage-floor shortfall Block or top-up per verified policy; trace quantity and rate Payroll inputs Missing rate, group overlap, employee exited midperiod Block affected line; effective-date reconciliation Payroll compliance Missing/expired rule, illegal deduction, tax configuration mismatch Block finalization; qualified review; version rule and rationale Payroll execution Concurrent runs, rounding drift, prior-period corrections Run lock; deterministic rounding; adjustment/reopen workflow Finance authorization Reviewer=authorizer, revoked permission, changed batch Enforce separation; invalidate approvals on material changes Bank payments Timeout, duplicate request, partial settlement, returned payment Reconcile before retry; isolate failed items; no duplicate payment

## Page 24

X1. Exceptional handling — continued
Domain
Trigger / failure
Required response
Documents
Template missing, signature refused, storage unavailable
Task blocked; safe retry; signed version and access history
Employee relations
Confidential complaint, retaliation risk, unauthorized viewing Restricted case access; human investigation; access audit Offboarding Disputed exit, unreturned asset, incomplete final attendance Track clearance; hold only lawful items; security escalation Integrations Rate limit, expired token, provider schema change, callback replay Circuit breaker; reconnect; dead-letter; dedupe callbacks Notifications Email/SMS failed, recipient changed, confidential content exposure Retry and in-app fallback; minimal sensitive data Platform reliability DB outage, queue loss, job replay, restore, backup failure Fail closed for money; outbox replay; restore drills; alerting AI assistant Hallucinated rule, prompt injection, unauthorized data request No AI financial authority; tool allowlist; permission checks Retention / privacy Deletion request vs statutory retention, export leakage Legal hold review, scoped export, secure retention policy

## Page 25

X2. State machines and irreversible boundaries
Object
Allowed progression
Failure / recovery
Request
DRAFT -> SUBMITTED -> VALIDATING -> PROCESSING -> WAITING -> PROCESSING -> COMPLETED NEEDS_INFO / BLOCKED / REJECTED / CANCELLED / FAILED; only authorized reactivation Approval PENDING -> APPROVED / REJECTED / EXPIRED / REASSIGNED No second terminal decision; delegated actor audited Attendance RAW -> NORMALIZED -> MATCHED -> VERIFIED -> PAY_INPUT QUARANTINED / DISPUTED / CORRECTED; original event immutable Payroll run DRAFT -> CALCULATED -> VALIDATED -> REVIEWED -> AUTHORIZED -> PAYING -> RECONCILED ->
CLOSED
BLOCKED / REJECTED / PARTIAL; corrections through versioned adjustment Payment item PREPARED -> SUBMITTED -> CONFIRMED / FAILED UNKNOWN requires reconciliation, never blind retry Onboarding DRAFT -> IN_PROGRESS -> VERIFIED -> ACTIVE BLOCKED / CANCELLED; account provisioning confirmation Offboarding INITIATED -> CLEARANCE -> SETTLEMENT -> APPROVED -> CLOSED DISPUTED / BLOCKED; access revocation tracked separately Cross-cutting invariants
• Tenant isolation is checked on every read, write, file access and background job.
• An action is successful only after verified completion, not after enqueue or request submission.
• No payroll is authorized with critical unresolved input or statutory-rule exceptions.
• Every payroll component identifies source inputs, applicable rule version and calculation details.
• A salary or policy change never silently rewrites closed historical payroll.
• Finance release is distinct from payroll preparation and sensitive compensation changes.
• Unknown bank outcomes must be reconciled before any retry.
• No missing punch alone automatically becomes an unpaid absence.
• External callbacks, file imports and background jobs are idempotent and auditable.
• Human employment decisions and confidential investigations remain authorized human actions.

## Page 26

X3. Diagram-to-module traceability
Module
Primary diagrams
Critical exceptions
Company setup / records
A1, A2, E1, D1
Tenant leakage, duplicate records, policy versions
Recruitment / onboarding
S2, D4, E4
Duplicate candidates, offer withdrawal, provisioning failure
Attendance / shifts
S3, D2, E3
Device gaps, duplicates, overnight roster mismatch
Leave / overtime
S4, D2, E3
Overlapping leave, approval delays, entitlement conflict
Payroll / compensation
S5, S6, D3, E3
Missing inputs, compliance blocks, partial payment
Employee services / performance
S1, D1, E2, E4
Permission denial, delegation, stale approval
Relations / training / recognition
S1, D4, E4
Confidentiality, missing evidence, approvals
Offboarding / settlement
S7, D4, E4
Disputes, asset clearance, revocation failure
Compliance / reporting
A2, D1, D3, E1
Expired legal rules, export leakage
Automation / integrations
A3, S8, E2, E4
Replay, timeout, retry exhaustion, dead-letter
Implementation validation gates
Unit: deterministic payroll formulas, rounding, overtime and leave policies. Integration: actual biometric devices, email, export and banking receipt formats. Contract: external adapter schema changes and replay. Security: tenant boundary, role revocation, MFA and restricted cases. End-to-end: hire-to-pay, missing-punch-to-correction, payroll-to-payment reconciliation, exit-to-settlement. Resilience: database restore, queue replay, network partition, partial settlement and dead-letter recovery. Regulatory: qualified review of current province- and category-specific statutory requirements before live payroll.
Source and design status
Based on the existing Pakistan HR Automation Complete System Design v1.0 (7 pages). Detailed tables, entity attributes, failure states, and sequence interactions are proposed elaborations of that baseline. They require confirmation against actual customer processes, provider APIs and applicable law before implementation.
