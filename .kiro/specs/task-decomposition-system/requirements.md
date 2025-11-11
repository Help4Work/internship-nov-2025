# Requirements Document

## Introduction

This document defines the requirements for a task decomposition system that breaks down the global challenge (completing the bordfu project with APITable integration) into manageable subtasks for internship participants. The system ensures that the mentor (Andrii Rogovskyi) is excluded from task assignments and that participants can work asynchronously on their assigned subtasks.

## Glossary

- **Task Decomposition System**: The software system that analyzes the global challenge and creates subtasks for participants
- **Global Challenge**: The main project goal of launching the bordfu application with APITable integration via docker compose
- **Participant**: An internship team member who will execute subtasks (excludes the mentor)
- **Mentor**: Andrii Rogovskyi, who provides guidance but does not perform implementation subtasks
- **Subtask**: A discrete, actionable work item that can be completed independently by a participant
- **Captain**: The weekly rotating team leader who coordinates task assignments and reviews
- **Async Execution**: The ability for participants to work on subtasks independently without blocking each other

## Requirements

### Requirement 1

**User Story:** As a captain, I want the system to decompose the global challenge into subtasks, so that I can assign work to team members efficiently

#### Acceptance Criteria

1. WHEN the captain provides the global challenge description, THE Task Decomposition System SHALL generate a list of subtasks that collectively accomplish the global challenge
2. THE Task Decomposition System SHALL ensure each subtask is independently executable without requiring sequential completion
3. THE Task Decomposition System SHALL include estimated effort and skill level for each subtask
4. THE Task Decomposition System SHALL organize subtasks by logical groupings such as frontend, backend, database, and deployment
5. THE Task Decomposition System SHALL ensure all subtasks combined cover 100% of the global challenge requirements

### Requirement 2

**User Story:** As a captain, I want the mentor excluded from task assignments, so that only participants perform implementation work

#### Acceptance Criteria

1. THE Task Decomposition System SHALL maintain a list of participants eligible for task assignment
2. THE Task Decomposition System SHALL exclude Andrii Rogovskyi from the participant list used for task assignment
3. WHEN generating task assignments, THE Task Decomposition System SHALL only assign subtasks to non-mentor participants
4. THE Task Decomposition System SHALL allow the mentor role to be marked as "advisor only" in the system

### Requirement 3

**User Story:** As a participant, I want to see my assigned subtasks with clear acceptance criteria, so that I can complete them independently

#### Acceptance Criteria

1. THE Task Decomposition System SHALL provide each subtask with a clear objective statement
2. THE Task Decomposition System SHALL include specific acceptance criteria for each subtask
3. THE Task Decomposition System SHALL specify required files or components to be created or modified for each subtask
4. THE Task Decomposition System SHALL include references to relevant documentation or resources for each subtask
5. WHEN a participant views their subtask, THE Task Decomposition System SHALL display dependencies on other subtasks if any exist

### Requirement 4

**User Story:** As a captain, I want to track subtask progress and dependencies, so that I can coordinate the team effectively

#### Acceptance Criteria

1. THE Task Decomposition System SHALL maintain the status of each subtask as "not started", "in progress", or "completed"
2. THE Task Decomposition System SHALL identify and display dependencies between subtasks
3. WHEN a subtask is marked complete, THE Task Decomposition System SHALL notify participants assigned to dependent subtasks
4. THE Task Decomposition System SHALL generate a visual progress report showing completion percentage
5. THE Task Decomposition System SHALL allow the captain to reassign subtasks between participants

### Requirement 5

**User Story:** As a participant, I want to work on my subtasks asynchronously, so that I can contribute without waiting for others

#### Acceptance Criteria

1. THE Task Decomposition System SHALL minimize blocking dependencies between subtasks
2. WHEN decomposing the global challenge, THE Task Decomposition System SHALL create parallel work streams where possible
3. THE Task Decomposition System SHALL identify which subtasks can be started immediately versus which require prerequisites
4. THE Task Decomposition System SHALL support participants working in different time zones
5. THE Task Decomposition System SHALL allow participants to mark subtasks as complete without requiring synchronous approval

### Requirement 6

**User Story:** As a captain, I want subtasks to align with the 2-week sprint timeline, so that the team can complete the project on schedule

#### Acceptance Criteria

1. THE Task Decomposition System SHALL estimate completion time for each subtask in hours or days
2. THE Task Decomposition System SHALL ensure the total estimated effort fits within a 10-day working period
3. WHEN generating subtasks, THE Task Decomposition System SHALL prioritize minimal viable implementation over comprehensive features
4. THE Task Decomposition System SHALL flag subtasks that exceed 2 days of estimated effort as needing further decomposition
5. THE Task Decomposition System SHALL distribute workload evenly across participants based on available participants

### Requirement 7

**User Story:** As a participant, I want subtasks to include technical guidance, so that I can complete them with AI assistance

#### Acceptance Criteria

1. THE Task Decomposition System SHALL include suggested file paths and code locations for each subtask
2. THE Task Decomposition System SHALL provide example prompts or questions to ask AI tools for each subtask
3. THE Task Decomposition System SHALL reference specific technologies or frameworks needed for each subtask
4. THE Task Decomposition System SHALL include links to relevant documentation for bordfu and APITable
5. WHEN a subtask involves unfamiliar technology, THE Task Decomposition System SHALL include learning resources
