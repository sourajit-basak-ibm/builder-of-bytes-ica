# Functional Requirements - Candidate Onboarding Application

## Overview

This document details the functional requirements for the Real-Time Hiring & Onboarding Application based on the schema defined in [`candidate-onboarding-context-studio.jsonld`](../candidate-onboarding-context-studio.jsonld).

---

## FR-001: Project Management

### Description
The system shall allow project managers to create and manage projects requiring staffing.

### Acceptance Criteria
- Project managers can create new projects with unique identifiers
- Projects must have a defined technology stack
- Projects can be in one of four states: PLANNING, ACTIVE, ON_HOLD, COMPLETED
- Each project must have at least one assigned project manager
- Project information includes: name, technology stack, status, start date, and commitments

### Related Entities
- `Project`
- `ProjectManager`

### Workflow States
1. **ProjectPlanning** (Initial) - Project is being planned
2. **ProjectActive** - Project is actively running
3. **ProjectOnHold** - Project is temporarily paused
4. **ProjectCompleted** (Terminal) - Project has finished

### Business Rules
- Project name must be unique across the system
- Technology stack must be defined before project activation
- A project cannot move to ACTIVE state without an assigned manager

---

## FR-002: Staffing Request Creation

### Description
Project managers shall be able to create staffing requests specifying the number of positions and required skills.

### Acceptance Criteria
- Managers can create staffing requests linked to specific projects
- Each request must specify the number of positions needed
- Requests must define urgency level (HIGH, MEDIUM, LOW)
- Requests must specify at least one required skill
- System tracks request status through its lifecycle

### Related Entities
- `StaffingRequest`
- `ProjectManager`
- `Project`
- `SkillRequirement`

### Workflow States
1. **RequestOpen** (Initial) - Request is newly created
2. **RequestInProgress** - Candidates are being evaluated
3. **RequestFulfilled** (Terminal) - All positions filled
4. **RequestCancelled** (Terminal) - Request cancelled

### Business Rules
- Number of positions must be greater than 0
- Request must be associated with an active or planning project
- At least one skill requirement must be defined
- Request cannot be fulfilled until all positions are filled

---

## FR-003: Skill Requirements Definition

### Description
The system shall allow definition of technical skills and proficiency levels required for project roles.

### Acceptance Criteria
- Skills can be defined with unique identifiers and names
- Each skill must specify a proficiency level (BEGINNER, INTERMEDIATE, ADVANCED, EXPERT)
- Skills can specify minimum years of experience
- Skills can be marked as mandatory or optional
- Skills are reusable across multiple staffing requests

### Related Entities
- `SkillRequirement`
- `StaffingRequest`
- `Candidate`

### Business Rules
- Skill names must be unique
- Proficiency level must be one of the defined values
- Mandatory skills must be matched for candidate consideration
- Years of experience must be non-negative if specified

---

## FR-004: Candidate Sourcing

### Description
The system shall support sourcing candidates from both internal and external sources.

### Acceptance Criteria
- System can pull candidates from internal employee database
- System can source candidates from external platforms (LinkedIn, job portals)
- Each candidate is tagged as INTERNAL or EXTERNAL
- Candidate profiles include: name, email, phone, skills, and experience
- System maintains candidate status throughout the hiring process

### Related Entities
- `Candidate`
- `Resume`
- `ResumeSource`

### Workflow States
1. **CandidateSourced** (Initial) - Candidate identified
2. **CandidateScreening** - Initial screening in progress
3. **CandidateInterviewing** - In interview process
4. **CandidateShortlisted** - Selected for further consideration
5. **CandidateRejected** (Terminal) - Not selected
6. **CandidateHired** (Terminal) - Successfully hired

### Business Rules
- Email must be unique across all candidates
- Candidate source must be either INTERNAL or EXTERNAL
- Internal candidates have priority in matching
- External candidates require HR team involvement

---

## FR-005: Automated Candidate Matching

### Description
The system shall automatically match candidates against skill requirements and calculate match scores.

### Acceptance Criteria
- System compares candidate skills with job requirements
- Match score is calculated (0-100 scale)
- Candidates with score > 70 are automatically considered
- System considers both mandatory and optional skills
- Match results are stored for review

### Related Entities
- `Candidate`
- `SkillRequirement`
- `StaffingRequest`

### Business Rules
- Match score must be between 0 and 100
- Mandatory skills must be present for any match
- Higher proficiency levels increase match score
- Years of experience factor into scoring

---

## FR-006: Interview Scheduling

### Description
The system shall automatically schedule interviews with candidates, checking calendar availability and sending Teams invites.

### Acceptance Criteria
- System checks project manager's Outlook calendar for availability
- Interviews are scheduled with appropriate interview panels
- Microsoft Teams meeting links are automatically generated
- Calendar events are created in Outlook
- Interview types include: TECHNICAL, BEHAVIORAL, MANAGERIAL, HR
- Interview duration is configurable

### Related Entities
- `Interview`
- `CalendarEvent`
- `InterviewPanel`
- `PanelMember`
- `Candidate`

### Workflow States
1. **InterviewScheduled** (Initial) - Interview is scheduled
2. **InterviewCompleted** (Terminal) - Interview finished
3. **InterviewCancelled** (Terminal) - Interview cancelled

### Business Rules
- Scheduled date must be in the future for SCHEDULED status
- Interview must have at least one panel member
- Teams link must be generated for all interviews
- Interview feedback is required before completion

---

## FR-007: Interview Panel Management

### Description
The system shall assign interview panels based on technology requirements and expertise.

### Acceptance Criteria
- Panels are created based on required technology stack
- Each panel has a defined technology focus
- Panel members have specified expertise areas
- Panel members can have roles: LEAD, TECHNICAL_EXPERT, HR_REPRESENTATIVE
- Panels are reusable across multiple interviews

### Related Entities
- `InterviewPanel`
- `PanelMember`
- `Interview`

### Business Rules
- Panel must have at least one member
- Technology focus must align with job requirements
- At least one technical expert must be present
- Panel lead is responsible for final decision

---

## FR-008: Shortlist Decision Making

### Description
The system shall record shortlist decisions after interviews and trigger appropriate notifications.

### Acceptance Criteria
- Decisions are recorded with date and reasoning
- Overall candidate score is calculated
- Shortlist status (selected/rejected) is clearly indicated
- Decision triggers automated notifications
- Decision history is maintained for audit

### Related Entities
- `ShortlistDecision`
- `Interview`
- `Candidate`
- `Notification`

### Business Rules
- Decision can only be made after interview completion
- Interview feedback must be provided
- Overall score must be between 0 and 100 if provided
- Shortlisted candidates automatically move to next stage

---

## FR-009: Stakeholder Notifications

### Description
The system shall send automated email notifications to all relevant stakeholders at key stages.

### Acceptance Criteria
- Notifications sent to project manager, candidate, and HR team
- Notification types: SHORTLIST, INTERVIEW_SCHEDULED, ONBOARDING_INITIATED
- Email includes relevant details and next steps
- Notification delivery status is tracked
- Failed notifications are retried

### Related Entities
- `Notification`
- `Stakeholder`
- `ProjectManager`
- `Candidate`

### Business Rules
- Must have at least one recipient
- Notification must be associated with a triggering event
- HR team is notified for all external candidates
- Delivery failures are logged and reported

---

## FR-010: Face-to-Face Meeting Coordination

### Description
The system shall schedule face-to-face meetings between shortlisted candidates and project managers.

### Acceptance Criteria
- Meetings are scheduled after shortlist decision
- Physical meeting location is specified
- Meeting agenda includes project overview and commitments
- Meeting outcome is recorded
- Successful meetings trigger onboarding workflow

### Related Entities
- `FaceToFaceMeeting`
- `Candidate`
- `ProjectManager`
- `OnboardingWorkflow`

### Workflow States
1. **MeetingScheduled** (Initial) - Meeting is scheduled
2. **MeetingCompleted** (Terminal) - Meeting finished successfully
3. **MeetingCancelled** (Terminal) - Meeting cancelled

### Business Rules
- Only shortlisted candidates can have face-to-face meetings
- Location must be defined
- Meeting must be completed before onboarding initiation
- Agreement must be reached for onboarding to proceed

---

## FR-011: Onboarding Workflow Initiation

### Description
The system shall initiate and manage the onboarding workflow for hired candidates.

### Acceptance Criteria
- Onboarding is triggered after successful face-to-face meeting
- Workflow includes training program assignment
- HR team is notified for external candidates
- Onboarding progress is tracked (0-100%)
- Expected completion date is set

### Related Entities
- `OnboardingWorkflow`
- `Candidate`
- `Project`
- `Stakeholder`
- `TrainingProgram`

### Workflow States
1. **OnboardingInitiated** (Initial) - Onboarding started
2. **OnboardingInProgress** - Onboarding activities ongoing
3. **OnboardingCompleted** (Terminal) - Onboarding finished

### Business Rules
- Onboarding can only start after face-to-face meeting completion
- Must be associated with both candidate and project
- Training program must be assigned
- Completion percentage must be between 0 and 100

---

## FR-012: Training Program Management

### Description
The system shall create and manage comprehensive training programs for new team members.

### Acceptance Criteria
- Training programs are automatically created during onboarding
- Programs include multiple training modules
- Program duration is defined in days
- Training status is tracked
- Programs include: architecture overview, code walkthroughs, technology training

### Related Entities
- `TrainingProgram`
- `TrainingModule`
- `OnboardingWorkflow`
- `Candidate`

### Workflow States
1. **TrainingNotStarted** (Initial) - Training not yet begun
2. **TrainingInProgress** - Training activities ongoing
3. **TrainingCompleted** (Terminal) - All training finished

### Business Rules
- Program name must be unique
- Must have at least one training module
- Modules must be completed in sequence
- All modules must be completed for program completion

---

## FR-013: Training Module Delivery

### Description
The system shall deliver individual training modules covering different aspects of the project.

### Acceptance Criteria
- Modules have defined types: ARCHITECTURE_OVERVIEW, CODE_WALKTHROUGH, TECHNOLOGY_TRAINING
- Each module has an estimated duration
- Module completion is tracked
- Modules can be accessed on-demand
- Progress is saved automatically

### Related Entities
- `TrainingModule`
- `TrainingProgram`
- `Candidate`

### Business Rules
- Module names must be unique within a training program
- Module type must be one of the defined values
- Modules must be completed before moving to next
- Completion status is binary (completed/not completed)

---

## FR-014: Automated Architecture Documentation

### Description
The system shall automatically generate architecture documentation and diagrams for onboarding.

### Acceptance Criteria
- Documentation is auto-generated from project codebase
- Multiple document types: SYSTEM_ARCHITECTURE, COMPONENT_DIAGRAM, DATA_FLOW
- Documentation is kept up-to-date automatically
- Documents are accessible via URL
- Last update timestamp is maintained

### Related Entities
- `ArchitectureDocumentation`
- `TrainingProgram`
- `Project`

### Business Rules
- Document titles must be unique
- Must be associated with a project
- Auto-generated documents are marked as such
- Documentation is regenerated when codebase changes

---

## FR-015: Automated Code Walkthroughs

### Description
The system shall generate automated code walkthrough videos for key modules.

### Acceptance Criteria
- Walkthroughs are auto-generated for important code modules
- Videos explain code structure and flow
- Walkthroughs include narration and annotations
- Videos are accessible via URL
- Duration is tracked for planning

### Related Entities
- `CodeWalkthrough`
- `TrainingProgram`
- `Project`

### Business Rules
- Walkthrough titles must be unique
- Code module must be specified
- Auto-generated walkthroughs are marked as such
- Videos are regenerated when code changes significantly

---

## FR-016: Knowledge Transfer Sessions

### Description
The system shall facilitate automated knowledge transfer sessions to reduce dependency on developer availability.

### Acceptance Criteria
- Sessions can be LIVE, RECORDED, or AUTOMATED
- Session recordings are available on-demand
- Sessions cover end-to-end flows and architecture
- Attendance is tracked
- Session completion is recorded

### Related Entities
- `KnowledgeTransferSession`
- `TrainingProgram`
- `Candidate`

### Business Rules
- Session titles must be unique
- Recorded sessions must have accessible URLs
- Automated sessions are available 24/7
- Completion is required for onboarding progress

---

## FR-017: Resume Management

### Description
The system shall manage candidate resumes and profile information.

### Acceptance Criteria
- Resumes are stored with unique identifiers
- Resume URLs are maintained for access
- Resume summaries are extracted
- Total experience is calculated
- Last update date is tracked

### Related Entities
- `Resume`
- `Candidate`
- `ResumeSource`

### Business Rules
- Each candidate must have exactly one resume
- Resume must be associated with a source
- Last updated date is automatically maintained
- Resume content is searchable

---

## FR-018: Multi-Source Resume Integration

### Description
The system shall integrate with multiple resume sources for candidate sourcing.

### Acceptance Criteria
- Internal employee database integration
- LinkedIn API integration
- Job portal integrations
- Source status tracking (active/inactive)
- API endpoint configuration

### Related Entities
- `ResumeSource`
- `Resume`
- `Candidate`

### Business Rules
- Source names must be unique
- Source type must be INTERNAL or EXTERNAL
- Active sources are queried automatically
- API endpoints must be valid URLs

---

## FR-19: Stakeholder Management

### Description
The system shall manage stakeholders involved in the hiring process.

### Acceptance Criteria
- Stakeholders include HR team, team leads, department heads
- Each stakeholder has a defined role
- Contact information is maintained
- Stakeholders receive relevant notifications
- Department associations are tracked

### Related Entities
- `Stakeholder`
- `Notification`
- `OnboardingWorkflow`

### Business Rules
- Email must be unique and valid
- Role must be one of: HR_TEAM, TEAM_LEAD, DEPARTMENT_HEAD
- Stakeholders receive notifications based on their role
- HR team is involved in all external candidate processes

---

## FR-020: End-to-End Workflow Orchestration

### Description
The system shall orchestrate the complete hiring workflow from staffing request to onboarding completion.

### Acceptance Criteria
- Workflow progresses automatically through stages
- State transitions are tracked and logged
- Workflow can be paused and resumed
- Progress is visible to all stakeholders
- Workflow completion triggers appropriate events

### Related Operations
- `ManagesProject`: ProjectManager → Project
- `CreatesStaffingRequest`: ProjectManager → StaffingRequest
- `RequiresSkill`: StaffingRequest → SkillRequirement
- `MatchesSkill`: Candidate → SkillRequirement
- `SchedulesInterview`: Candidate → Interview
- `MakesShortlistDecision`: Interview → ShortlistDecision
- `TriggersNotification`: ShortlistDecision → Notification
- `SchedulesFaceToFace`: ShortlistDecision → FaceToFaceMeeting
- `InitiatesOnboarding`: FaceToFaceMeeting → OnboardingWorkflow
- `IncludesTraining`: OnboardingWorkflow → TrainingProgram
- `GeneratesDocumentation`: TrainingProgram → ArchitectureDocumentation
- `GeneratesWalkthrough`: TrainingProgram → CodeWalkthrough

### Business Rules
- Each stage must complete before moving to next
- State transitions must follow defined paths
- Terminal states cannot be exited
- Events are emitted at key transitions

---

## Traceability Matrix

| Requirement ID | Entity | Operation | States |
|---------------|--------|-----------|--------|
| FR-001 | Project | ManagesProject | ProjectPlanning, ProjectActive, ProjectOnHold, ProjectCompleted |
| FR-002 | StaffingRequest | CreatesStaffingRequest | RequestOpen, RequestInProgress, RequestFulfilled, RequestCancelled |
| FR-003 | SkillRequirement | RequiresSkill | N/A |
| FR-004 | Candidate, Resume, ResumeSource | N/A | CandidateSourced, CandidateScreening, CandidateInterviewing, CandidateShortlisted, CandidateRejected, CandidateHired |
| FR-005 | Candidate, SkillRequirement | MatchesSkill | N/A |
| FR-006 | Interview, CalendarEvent | SchedulesInterview | InterviewScheduled, InterviewCompleted, InterviewCancelled |
| FR-007 | InterviewPanel, PanelMember | N/A | N/A |
| FR-008 | ShortlistDecision | MakesShortlistDecision | N/A |
| FR-009 | Notification, Stakeholder | TriggersNotification | N/A |
| FR-010 | FaceToFaceMeeting | SchedulesFaceToFace | MeetingScheduled, MeetingCompleted, MeetingCancelled |
| FR-011 | OnboardingWorkflow | InitiatesOnboarding | OnboardingInitiated, OnboardingInProgress, OnboardingCompleted |
| FR-012 | TrainingProgram | IncludesTraining | TrainingNotStarted, TrainingInProgress, TrainingCompleted |
| FR-013 | TrainingModule | N/A | N/A |
| FR-014 | ArchitectureDocumentation | GeneratesDocumentation | N/A |
| FR-015 | CodeWalkthrough | GeneratesWalkthrough | N/A |
| FR-016 | KnowledgeTransferSession | N/A | N/A |
| FR-017 | Resume | N/A | N/A |
| FR-018 | ResumeSource | N/A | N/A |
| FR-019 | Stakeholder | N/A | N/A |
| FR-020 | All | All | All |

---

## Version History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-05-18 | System | Initial functional requirements based on schema |

---

## References

- Schema Definition: [`candidate-onboarding-context-studio.jsonld`](../candidate-onboarding-context-studio.jsonld)
- Non-Functional Requirements: [`non-functional-requirements.md`](non-functional-requirements.md)
- README: [`README.md`](../README.md)