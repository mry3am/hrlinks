# Software Engineering Requirements Before Project

*SDLC roadmap for the HR Automation SaaS*

## Page 1

For our HR Automation SaaS, I recommend following a professional Software Development Life Cycle (SDLC) using a combination of:
• Requirements Engineering — understand what we're building before coding.
• Domain-Driven Design (DDD) — organize the system around real HR business processes.
• Architecture-First Design — define how modules communicate and share data.
• Agile Development — build small, complete, testable features.
• Test-Driven Development (TDD) — write tests before implementation where practical.
• Continuous Integration/Continuous Delivery (CI/CD) — automatically validate every change.
The most important principle is:
Don't build 14 modules separately and hope they work together. Design the whole system, then implement one complete workflow at a time.
Our complete software engineering roadmap
From idea to production
1. Product discovery
2. Requirements engineering
3. Business process modeling
4. UX and user journey design
5. System architecture
6. Database and API design
7. Engineering foundation
8. Vertical-slice development
9. Integration and system testing
10. Security and compliance validation
11. Pilot deployment

## Page 2

12. Production launch and maintenance
Phase 1 — Product discovery
Start here
Before touching the code, understand how real companies operate.
What we do:
1. Select initial customers: Pakistani SMEs and manufacturing businesses.
2. Interview owners, HR administrators, finance staff, supervisors and employees.
3. Study how they currently manage attendance, leave, hiring and payroll.
4. Identify manual work, repeated data entry, delays and common exceptions.
5. Compare existing HR software and identify what we can genuinely automate.
6. Define our first release's boundaries.
Deliverables:
• Product_Vision.md
• Market_Research.md
• Personas.md
• Problem_Statements.md
• MVP_Scope.md
Completion condition: We can explain who will buy the product, what problems it solves, and which workflows the first release must support.
Phase 2 — Requirements engineering
This is where we turn ideas into precise, testable requirements.
For every workflow, define:
• Who initiates it?
• What information is required?
• What does the system do automatically?
• What requires approval?

## Page 3

• What happens when something fails?
• Which records change?
• How does the user know it is complete?
• What permissions and legal rules apply?
For example, a leave request must include normal approval, rejection, cancellation, insufficient balance, overlapping requests, unavailable approvers, and payroll cutoff handling.
Deliverables:
• SRS.md — Software Requirements Specification
• Functional_Requirements.md
• Non_Functional_Requirements.md
• Workflow_Catalog.md
• Business_Rules.md
• Acceptance_Criteria.md
Completion condition: Each MVP requirement is clear enough that a developer can implement it and a tester can independently verify it.
Phase 3 — Business process modeling
Now design the actual processes before designing the database.
For our HR platform, map:
1. Company registration and configuration.
2. Employee creation and import.
3. Roles and permissions.
4. Attendance collection and correction.
5. Leave requests and approvals.
6. Shift and overtime management.
7. Payroll calculation and authorization.
8. Recruitment, onboarding and offboarding.

## Page 4

For each process, create a normal flow, alternative flows, failure paths, recovery rules and state transitions.
Deliverables:
• BPMN workflow diagrams
• UML sequence diagrams
• State-machine diagrams
• Exception-handling matrix
• Workflow dependency map
Completion condition: Every critical process has defined success, failure, cancellation and recovery behavior.
Phase 4 — UX/UI design
This is especially important for our product because flexibility and ease of use are our main competitive advantages.
Design the user experience before developing screens.
Example: Company onboarding journey
1. Company owner creates an account.
2. System asks a few essential company questions.
3. System suggests policies and configurations.
4. Owner reviews and customizes the suggestions.
5. Company imports employee records.
6. System identifies missing or conflicting information.
7. Owner assigns managers and finance permissions.
8. System opens a personalized workspace with relevant next actions.
We should create low-fidelity wireframes, interactive prototypes, reusable design components and responsive layouts.
Test the prototypes with people who represent actual users, including factory supervisors and employees with limited technical experience.

## Page 5

Deliverables:
• User_Journeys.md
• Information_Architecture.md
• UX_Guidelines.md
• Figma prototypes
• Design system and component library
• Usability test findings
Completion condition: Representative users can complete critical tasks without extensive instruction, and serious usability problems have been resolved.
Phase 5 — System architecture
Only after understanding the business and user experience should we finalize the technical design.
For our product, I recommend:
Layer Technology
Frontend Next.js, React, TypeScript
Backend NestJS, TypeScript
Database PostgreSQL, Prisma
Workflow engine Temporal, subject to an early proof of concept Background jobs Redis, BullMQ where appropriate AI services Python FastAPI, LLM APIs File storage S3-compatible storage Authentication OIDC-compatible authentication and RBAC Infrastructure Docker, managed cloud services Testing Vitest/Jest, Playwright, Pytest CI/CD GitHub Actions

## Page 6

Start with a modular monolith, clear module boundaries, and asynchronous workers for long-running operations.
Deliverables:
• High-level architecture diagram
• Component diagram
• Deployment diagram
• Architecture Decision Records (ADRs)
• Security architecture
• Integration contracts
• Failure and recovery design
Completion condition: We can explain where each responsibility belongs, how components communicate, and how failures are handled.
Phase 6 — Database and API design
Design the data around business responsibilities rather than individual UI pages.
Start with company, user, employee, employment, policy, request, approval, task and audit entities.
Then add attendance, leave, shifts, payroll and other domains as their workflows are implemented.
Define database constraints, indexes, migrations, API contracts, authorization rules, transactions, idempotency and data retention.
Deliverables:
• ER diagrams
• Database schema
• API specification (OpenAPI)
• Permission matrix
• Migration strategy
• Data integrity rules

## Page 7

Completion condition: The model supports multiple companies, configurable policies, historical changes and reliable workflow execution.
Phase 7 — Engineering foundation
Before implementing business features, establish the development standards.
• Git repository and branch protection.
• Code formatting, linting and type checking.
• Environment configuration and secret management.
• Automated testing framework.
• CI pipeline.
• Database migration process.
• Structured logging and error monitoring.
• Authentication and authorization foundations.
• Test data factories.
• Development, staging and production environments.
Completion condition: Every pull request automatically runs checks, and the application can be deployed to staging reliably.
Phase 8 — Build one vertical slice at a time
This is the phase where coding begins.
A vertical slice is one complete user workflow, including its UI, backend, database, permissions, automation, errors and tests.
For example, don't build the entire leave frontend first, then all backend APIs, then testing.
Build one complete leave request from submission to final outcome.
Our recommended implementation order
1. Foundation: Company registration, authentication, roles, company isolation.
2. Company configuration: Departments, policies, approval rules, working calendars.
3. Employee management: Employee profiles, imports, reporting relationships.
4. Workflow engine: Requests, approvals, notifications, retries, audit history.

## Page 8

5. Leave management: Complete request and approval lifecycle.
6. Attendance and shifts: Capture, reconciliation, corrections, rosters.
7. Overtime: Eligibility, verification and authorization.
8. Payroll: Salary policies, calculations, review, authorization, payslips.
9. Employee lifecycle: Recruitment, onboarding, performance and offboarding.
10. Advanced services: Integrations, AI assistance, reporting and additional workflows.
Phase 9 — Testing methodology
Testing must happen during every development slice, not only after the entire system is built.
Test type What it verifies
Unit tests Individual functions, calculations and rules
Integration tests Database, services, queues and external adapters Contract tests API and integration compatibility Workflow tests Full success, rejection, cancellation and recovery paths End-to-end tests Real user actions through the application Security tests Permissions, company isolation, authentication Performance tests Large employee imports, payroll runs and concurrent users Regression tests Existing features still work after changes Usability tests Real users can complete tasks efficiently User acceptance tests The workflow meets approved business requirements For payroll, use independently verified calculation examples. For attendance, test missing punches, overnight shifts, duplicates and conflicting records.
Every production defect should result in a regression test whenever feasible.
Phase 10 — Security and compliance validation
Before processing real employee records or wages:

## Page 9

• Verify company data isolation.
• Test role-based permissions and sensitive approval boundaries.
• Review data protection and document access.
• Verify payroll calculations against applicable laws and employment terms.
• Test backups and restoration.
• Test failed payments and payroll correction handling.
• Conduct dependency, vulnerability and penetration testing appropriate to the release.
• Confirm auditability of salary and employee record changes.
Completion condition: Critical security and payroll issues are resolved, and required legal rules have been independently verified.
Phase 11 — Pilot deployment
Do not immediately launch to hundreds of companies.
Start with one or two pilot customers.
Import real employee records with authorization, run representative workflows, and compare payroll against their existing verified process.
Measure task completion, automation success, error rates, time saved and usability problems.
Completion condition: The pilot meets agreed accuracy, security, reliability and usability criteria.
Phase 12 — Production and continuous improvement
After a successful pilot:
• Release with controlled feature flags.
• Monitor errors and workflow failures.
• Track automation completion rates.
• Maintain backups and recovery procedures.
• Review customer feedback.

## Page 10

• Fix defects before expanding scope.
• Release additional workflows incrementally.
• Update statutory rules through controlled review and deployment.
The development cycle we must repeat for every module
This is the most important part for preventing uncontrolled AI-generated code.
1. Understand the requirement
2. Design the user journey and exceptions
3. Review architecture, data and API impact
4. Define acceptance criteria and tests
5. Implement the smallest complete workflow
6. Run automated and manual tests
7. Review code, security and usability
8. Merge, deploy to staging and verify
A feature is Done only when it meets its acceptance criteria, tests pass, permissions and exception paths are verified, documentation is updated, and staging validation succeeds.
Our immediate task is to complete Phase 1: Product Discovery, then produce a sufficiently detailed MVP requirements baseline.
For our specific HR system, the first critical question to validate with real companies is:
Can a company configure its organization, policies, employee records and approvals without needing a technical person or dedicated implementation consultant?
That is the foundation of our product's flexibility.
After discovery, our first implemented vertical slice should be:
Company registration → guided company setup → department creation → employee import → role assignment → personalized dashboard.
It should include input validation, duplicate detection, permission checks, import rollback or correction, interrupted setup recovery, and complete tests.
Once this works properly, we build the reusable request and approval engine, then the first complete HR workflow.

## Page 11

The engineering rule for this project: One requirement → one reviewed design → one complete implementation → verified tests → merge. Then move to the next requirement.
That is how we build a maintainable commercial SaaS rather than a collection of AIgenerated features.
