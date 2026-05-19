# Candidate Onboarding Application - JSON-LD Schema

## Overview

This JSON-LD schema defines a comprehensive data model for a **Real-Time Hiring & Onboarding Application** designed for project managers to efficiently staff projects, conduct interviews, and onboard candidates with automated training and knowledge transfer capabilities.

## 📁 Project Structure

```
context_schema/
├── candidate-onboarding-context-studio.jsonld  # ⭐ Main schema for Context Studio
├── docs/
│   ├── functional-requirements.md              # 20 detailed functional requirements
│   └── non-functional-requirements.md          # 12 NFR categories with metrics
└── README.md                                   # This file
```

## 🚀 Quick Start

### For Context Studio Upload

**Primary File**: [`candidate-onboarding-context-studio.jsonld`](candidate-onboarding-context-studio.jsonld)

1. Open IBM Context Studio
2. Click **"Upload Sample Data"**
3. Select `candidate-onboarding-context-studio.jsonld`
4. Wait for graph visualization to appear (showing entities, operations, and states)
5. Click **"Publish Schema"**

## Schema Files

### Main Schema (Context Studio Format)
- **Primary Schema**: [`candidate-onboarding-context-studio.jsonld`](candidate-onboarding-context-studio.jsonld)
  - ✅ 21 Entities (Project, Candidate, Interview, Training, etc.)
  - ✅ 12 Operations (relationships between entities)
  - ✅ 28 States (workflow states for state machines)
  - ✅ Total: 61 items in @graph

### Documentation
- **Functional Requirements**: [`docs/functional-requirements.md`](docs/functional-requirements.md)
  - 20 detailed functional requirements (FR-001 to FR-020)
  - Complete workflow coverage
  - Traceability matrix
  
- **Non-Functional Requirements**: [`docs/non-functional-requirements.md`](docs/non-functional-requirements.md)
  - 12 NFR categories (Performance, Security, Reliability, etc.)
  - Quality attributes with target metrics
  - Testing requirements

## Key Features

The schema supports the following application features:

### 1. **Staffing & Candidate Management**
- Internal candidate matching within the company
- External resume sourcing from job portals and recruitment agencies
- Automated skill requirement matching with configurable match scores
- Support for both internal and external candidate sources

### 2. **Interview Scheduling & Management**
- Automated interview scheduling with Outlook calendar integration
- Microsoft Teams meeting invite generation
- Technology-based interview panel assignment
- Interview session tracking and recording

### 3. **Candidate Shortlisting & Notifications**
- Automated shortlist decision workflow
- Multi-stakeholder email notifications (Project Manager, Candidate, HR Team, Stakeholders)
- Face-to-face meeting coordination between managers and shortlisted candidates
- Project overview and commitment discussions

### 4. **Onboarding Workflow**
- Structured project onboarding process
- HR team integration for external candidates
- Status tracking (initiated, in_progress, completed)
- Automated stakeholder coordination

### 5. **Training & Knowledge Transfer**
- Comprehensive training programs for new team members
- Automated architecture documentation generation
- Code walkthrough automation
- End-to-end flow tracing across multi-module repositories
- Reduced dependency on developer availability

## Schema Structure

### Entities (21 Total)

The schema defines 21 entity types with complete attributes, invariants, and state machines:

| Entity | Description | Key Attributes |
|--------|-------------|----------------|
| **Project** | Project requiring staffing | projectId, projectName, technologyStack, status |
| **ProjectManager** | Manager responsible for staffing | managerId, name, email, department |
| **StaffingRequest** | Request for staffing needs | requestId, numberOfPositions, urgency, status |
| **SkillRequirement** | Technical skills required | skillId, skillName, proficiencyLevel, mandatory |
| **Candidate** | Internal or external candidate | candidateId, name, email, candidateSource, matchScore |
| **Resume** | Candidate's resume | resumeId, resumeUrl, totalExperience |
| **ResumeSource** | Source for resumes | sourceId, sourceName, sourceType, isActive |
| **InterviewPanel** | Panel of interviewers | panelId, panelName, technologyFocus |
| **PanelMember** | Individual panel member | memberId, name, email, expertise, role |
| **Interview** | Scheduled interview | interviewId, scheduledDate, interviewType, status |
| **CalendarEvent** | Calendar event | eventId, outlookEventId, startTime, isTeamsMeeting |
| **ShortlistDecision** | Shortlist decision | decisionId, isShortlisted, overallScore |
| **Notification** | Email notification | notificationId, notificationType, status |
| **Stakeholder** | Process stakeholder | stakeholderId, name, email, role |
| **FaceToFaceMeeting** | In-person meeting | meetingId, scheduledDate, location, outcome |
| **OnboardingWorkflow** | Onboarding process | workflowId, startDate, status, completionPercentage |
| **TrainingProgram** | Training program | programId, programName, duration, status |
| **TrainingModule** | Individual module | moduleId, moduleName, moduleType, isCompleted |
| **ArchitectureDocumentation** | Architecture docs | docId, title, documentType, isAutomated |
| **CodeWalkthrough** | Code walkthrough | walkthroughId, title, codeModule, isAutomated |
| **KnowledgeTransferSession** | KT session | sessionId, sessionTitle, sessionType, isCompleted |

### Operations (12 Total)

Operations define relationships and workflows between entities:

| Operation | From → To | Description |
|-----------|-----------|-------------|
| **ManagesProject** | ProjectManager → Project | Manager manages project |
| **CreatesStaffingRequest** | ProjectManager → StaffingRequest | Manager creates request |
| **RequiresSkill** | StaffingRequest → SkillRequirement | Request requires skills |
| **MatchesSkill** | Candidate → SkillRequirement | Candidate matches skills |
| **SchedulesInterview** | Candidate → Interview | System schedules interview |
| **MakesShortlistDecision** | Interview → ShortlistDecision | Interview results in decision |
| **TriggersNotification** | ShortlistDecision → Notification | Decision triggers notifications |
| **SchedulesFaceToFace** | ShortlistDecision → FaceToFaceMeeting | Schedules F2F meeting |
| **InitiatesOnboarding** | FaceToFaceMeeting → OnboardingWorkflow | Meeting initiates onboarding |
| **IncludesTraining** | OnboardingWorkflow → TrainingProgram | Onboarding includes training |
| **GeneratesDocumentation** | TrainingProgram → ArchitectureDocumentation | Generates docs |
| **GeneratesWalkthrough** | TrainingProgram → CodeWalkthrough | Generates walkthroughs |

### States (28 Total)

State machines for workflow management:

- **Project States**: ProjectPlanning, ProjectActive, ProjectOnHold, ProjectCompleted
- **Request States**: RequestOpen, RequestInProgress, RequestFulfilled, RequestCancelled
- **Candidate States**: CandidateSourced, CandidateScreening, CandidateInterviewing, CandidateShortlisted, CandidateRejected, CandidateHired
- **Interview States**: InterviewScheduled, InterviewCompleted, InterviewCancelled
- **Meeting States**: MeetingScheduled, MeetingCompleted, MeetingCancelled
- **Onboarding States**: OnboardingInitiated, OnboardingInProgress, OnboardingCompleted
- **Training States**: TrainingNotStarted, TrainingInProgress, TrainingCompleted
- **Event States**: ProjectStarted, RequestCreated

## Workflow

```
1. Project Manager creates Staffing Request
   ↓
2. System checks Internal Candidates
   ↓
3. If needed, pulls External Resumes
   ↓
4. Matches candidates against Skill Requirements
   ↓
5. Checks Outlook Calendar availability
   ↓
6. Sends Teams Meeting Invites with Interview Panel
   ↓
7. Conducts Interview Session
   ↓
8. Makes Shortlist Decision
   ↓
9. Sends Notifications to all Stakeholders
   ↓
10. Schedules Face-to-Face Meeting
   ↓
11. Discusses Project Overview & Commitments
   ↓
12. Initiates Onboarding Workflow
   ↓
13. Starts Training Program with:
    - Architecture Documentation
    - Automated Code Walkthroughs
    - Knowledge Transfer Sessions
    - End-to-End Flow Tracing
```

## Training & Knowledge Transfer Features

The schema addresses common onboarding challenges:

### Problems Solved
- ❌ **Before**: New developers take weeks to understand architecture
- ✅ **After**: Automated architecture documentation and diagrams

- ❌ **Before**: Knowledge transfer depends on developer availability
- ✅ **After**: Automated knowledge transfer sessions

- ❌ **Before**: Documentation is outdated or missing
- ✅ **After**: Automated documentation generation

- ❌ **Before**: Manual code walkthroughs consume time
- ✅ **After**: Automated code walkthrough videos

- ❌ **Before**: Difficult to trace end-to-end flows
- ✅ **After**: Automated end-to-end flow tracing

## Requirements Documentation

### Functional Requirements
See [`docs/functional-requirements.md`](docs/functional-requirements.md) for:
- **FR-001 to FR-020**: 20 detailed functional requirements
- Complete workflow coverage from staffing to training
- Acceptance criteria for each requirement
- Traceability matrix linking requirements to entities and operations
- Business rules and invariants

### Non-Functional Requirements
See [`docs/non-functional-requirements.md`](docs/non-functional-requirements.md) for:
- **NFR-001 to NFR-012**: 12 NFR categories
  - Performance (response time < 2s, 99.9% uptime)
  - Security (MFA, encryption, GDPR/CCPA compliance)
  - Reliability (data integrity, error handling)
  - Us ability (WCAG 2.1 AA, responsive design)
  - Integration (Outlook, Teams, LinkedIn, HR systems)
  - Automation (matching, scheduling, documentation)
- Quality attributes with measurable targets
- Testing requirements and compliance standards

## Usage for Context Studio

This schema is designed for IBM Context Studio and provides:

1. **Entity Definitions**: 21 entities with complete attribute definitions
2. **Relationship Modeling**: 12 operations defining entity interactions
3. **State Management**: 28 states for workflow orchestration
4. **Business Rules**: Invariants and constraints for data integrity
5. **Automation Support**: Preconditions and postconditions for operations
6. **Integration Ready**: Support for external systems (Outlook, Teams, LinkedIn)

### Schema Validation

The schema includes comprehensive validation:
- **Data Types**: String, number, date, boolean with constraints
- **Enums**: Predefined values for status fields
- **Invariants**: Business rules enforced at entity level
- **State Transitions**: Valid paths through workflow states
- **Referential Integrity**: Proper entity relationships

## Technical Details

### Schema Format
- **Format**: JSON-LD 1.1 with Context Studio extensions
- **Base Vocabulary**: `https://schema.org/`
- **Custom Namespace**: `http://example.org/candidate-onboarding#` (prefix: `co`)
- **Entity Types**: Entity, Operation, State
- **Total Items**: 61 items in @graph

### Key Features
- **State Machines**: Entities have defined states with initial and terminal states
- **Operations**: Relationships with preconditions and postconditions
- **Invariants**: Business rules enforced at entity level
- **Events**: State transitions emit events for workflow orchestration
- **Attributes**: Strongly typed with validation rules

### Extending the Schema

To customize for your organization:

1. **Add Entities**: Define new entity types in @graph
2. **Add Operations**: Create new relationships between entities
3. **Add States**: Define additional workflow states
4. **Modify Attributes**: Adjust entity properties as needed
5. **Update Invariants**: Add organization-specific business rules

## Integration Points

The schema supports integration with:

- **Microsoft Outlook**: Calendar event management
- **Microsoft Teams**: Meeting invite generation
- **HR Systems**: External candidate tracking
- **Resume Databases**: Internal and external candidate sources
- **Training Platforms**: Automated training delivery
- **Documentation Tools**: Architecture diagram generation
- **Code Analysis Tools**: Automated code walkthrough generation

## Implementation Notes

### Automation Capabilities
- **Candidate Matching**: Automated skill-based matching with scoring
- **Interview Scheduling**: Calendar integration with conflict resolution
- **Documentation Generation**: Auto-generated architecture docs and code walkthroughs
- **Notification System**: Multi-channel notifications with delivery tracking
- **Training Delivery**: Automated training content generation and delivery

### Integration Points
- **Microsoft Outlook**: Calendar availability and event management
- **Microsoft Teams**: Meeting link generation and attendance tracking
- **LinkedIn**: Candidate sourcing and profile import
- **HR Systems**: Employee data sync and compliance reporting
- **Training Platforms**: Content delivery and progress tracking

### Compliance & Security
- **Data Privacy**: GDPR and CCPA compliance built-in
- **Security**: Multi-factor authentication and role-based access
- **Audit Trail**: Complete logging of all actions and state changes
- **Data Retention**: Configurable retention policies by entity type

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-05-18 | Initial schema with 21 entities, 12 operations, 28 states |

## License

This schema is provided as-is for use in candidate onboarding applications.

---

**Next Steps**: Upload [`candidate-onboarding-context-studio.jsonld`](candidate-onboarding-context-studio.jsonld) to IBM Context Studio to begin using this schema.