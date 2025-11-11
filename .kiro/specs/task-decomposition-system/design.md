# Design Document: Task Decomposition System

## Overview

The Task Decomposition System is a markdown-based task management solution that decomposes the bordfu+APITable integration project into actionable subtasks for internship participants. The system uses structured markdown files to define tasks, track progress, and manage assignments while explicitly excluding the mentor from implementation work.

The design prioritizes simplicity and async collaboration, leveraging GitHub's native features (issues, projects, markdown) rather than building complex custom software. This aligns with the internship's goal of minimal tooling and maximum learning.

## Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   GitHub Repository                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌────────────────────────────────────────────────┐    │
│  │         TASKS.md (Master Task List)            │    │
│  │  - Global challenge breakdown                  │    │
│  │  - Subtask definitions                         │    │
│  │  - Assignment tracking                         │    │
│  │  - Progress checkboxes                         │    │
│  └────────────────────────────────────────────────┘    │
│                                                          │
│  ┌────────────────────────────────────────────────┐    │
│  │      PARTICIPANTS.md (Team Registry)           │    │
│  │  - Participant list (excludes mentor)          │    │
│  │  - Skills and availability                     │    │
│  │  - Current assignments                         │    │
│  └────────────────────────────────────────────────┘    │
│                                                          │
│  ┌────────────────────────────────────────────────┐    │
│  │    docs/subtasks/ (Detailed Task Specs)        │    │
│  │  - Individual subtask markdown files           │    │
│  │  - Acceptance criteria                         │    │
│  │  - Technical guidance                          │    │
│  │  - AI prompts and resources                    │    │
│  └────────────────────────────────────────────────┘    │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Component Interaction Flow

```
Captain → Runs decomposition script → Generates TASKS.md
                                    → Creates subtask files
                                    → Updates PARTICIPANTS.md

Participants → Read assigned tasks → Work independently
            → Update checkboxes   → Create PRs
            → Request reviews      → Captain merges
```

## Components and Interfaces

### 1. Task Decomposition Script

**Purpose**: Analyzes the global challenge and generates structured subtask files

**Implementation**: Python script (`scripts/decompose_tasks.py`)

**Key Functions**:
- `parse_global_challenge()`: Reads project requirements from README or input
- `generate_subtasks()`: Creates subtask breakdown based on project structure
- `assign_participants()`: Distributes tasks among eligible participants (excludes mentor)
- `create_task_files()`: Generates markdown files for each subtask
- `update_master_list()`: Creates/updates TASKS.md with overview

**Input**: 
- Global challenge description (from README or command line)
- Participant list from PARTICIPANTS.md
- Mentor exclusion list

**Output**:
- TASKS.md (master checklist)
- Individual subtask files in docs/subtasks/
- Updated PARTICIPANTS.md with assignments

### 2. TASKS.md Structure

**Format**:
```markdown
# Project Tasks: Bordfu + APITable Integration

## Sprint Goal
Launch bordfu application with APITable integration via single docker-compose command

## Task Categories

### 1. Environment Setup
- [ ] 1.1 Fork and clone bordfu repository - **Assigned**: [Participant Name] - **Effort**: 0.5 days
- [ ] 1.2 Set up local development environment - **Assigned**: [Participant Name] - **Effort**: 1 day
- [ ] 1.3 Configure APITable instance - **Assigned**: [Participant Name] - **Effort**: 1 day

### 2. Backend Integration
- [ ] 2.1 Replace Airtable SDK with APITable SDK - **Assigned**: [Participant Name] - **Effort**: 2 days
- [ ] 2.2 Update database connection logic - **Assigned**: [Participant Name] - **Effort**: 1.5 days
- [ ] 2.3 Implement CRUD operations for APITable - **Assigned**: [Participant Name] - **Effort**: 2 days

### 3. Docker Configuration
- [ ] 3.1 Create docker-compose.yml for bordfu - **Assigned**: [Participant Name] - **Effort**: 1 day
- [ ] 3.2 Integrate APITable service in docker-compose - **Assigned**: [Participant Name] - **Effort**: 1 day
- [ ] 3.3 Configure networking between services - **Assigned**: [Participant Name] - **Effort**: 0.5 days

### 4. Testing & Documentation
- [ ] 4.1 Test end-to-end application flow - **Assigned**: [Participant Name] - **Effort**: 1 day
- [ ] 4.2 Update README with setup instructions - **Assigned**: [Participant Name] - **Effort**: 0.5 days

## Progress: 0/12 tasks completed (0%)
```

### 3. PARTICIPANTS.md Structure

**Format**:
```markdown
# Internship Participants

## Eligible for Task Assignment

| Name | GitHub | Skills | Current Assignment | Status |
|------|--------|--------|-------------------|--------|
| Mumba Patrick | @username | Frontend, Docker | Task 1.1, 3.1 | Active |
| Kidus Messele | @username | Backend, APIs | Task 2.1, 2.2 | Active |
| Lesia Soloviova | @username | Frontend, Testing | Task 4.1, 4.2 | Active |

## Mentor (Not Assigned Tasks)

| Name | GitHub | Role |
|------|--------|------|
| Andrii Rogovskyi | @username | Advisor, Code Reviewer |

## Assignment Rules
- Maximum 2-3 tasks per participant at a time
- Tasks can be reassigned by captain if needed
- Mentor provides guidance but does not implement
```

### 4. Individual Subtask Files

**Location**: `docs/subtasks/task-{category}-{number}.md`

**Template Structure**:
```markdown
# Task {ID}: {Title}

## Objective
Clear one-sentence description of what needs to be accomplished

## Category
{Frontend | Backend | Database | DevOps | Testing}

## Estimated Effort
{X} days

## Prerequisites
- List of tasks that must be completed first
- Or "None - can start immediately"

## Acceptance Criteria
1. Specific, testable criterion
2. Another specific criterion
3. Final criterion

## Technical Guidance

### Files to Create/Modify
- `path/to/file1.js` - Description of changes
- `path/to/file2.py` - Description of changes

### Technologies Involved
- Technology 1 (with link to docs)
- Technology 2 (with link to docs)

### Suggested AI Prompts
1. "How do I configure APITable SDK in Node.js?"
2. "Show me an example of replacing Airtable API calls with APITable"

### Resources
- [Bordfu Repository](https://github.com/craftled/bordfu)
- [APITable Documentation](https://github.com/apitable/apitable)
- [Docker Compose Guide](https://docs.docker.com/compose/)

## Implementation Steps
1. Step-by-step guidance
2. Next step
3. Final step

## Testing
How to verify the task is complete

## Notes
Any additional context or warnings
```

## Data Models

### Task Object (in-memory representation)

```python
@dataclass
class Task:
    id: str  # e.g., "1.1", "2.3"
    title: str
    category: str  # "Frontend", "Backend", "Database", "DevOps", "Testing"
    description: str
    estimated_effort_days: float
    prerequisites: List[str]  # List of task IDs
    assigned_to: Optional[str]  # Participant name or None
    status: str  # "not_started", "in_progress", "completed"
    acceptance_criteria: List[str]
    files_to_modify: List[str]
    resources: List[Dict[str, str]]  # [{"title": "...", "url": "..."}]
    ai_prompts: List[str]
```

### Participant Object

```python
@dataclass
class Participant:
    name: str
    github_username: str
    linkedin: str
    skills: List[str]
    is_mentor: bool
    current_assignments: List[str]  # List of task IDs
    max_concurrent_tasks: int = 3
```

## Error Handling

### Validation Rules

1. **Mentor Exclusion Validation**
   - Before assigning any task, verify participant is not marked as mentor
   - Raise `MentorAssignmentError` if attempt to assign to Andrii Rogovskyi
   - Log warning if mentor name appears in assignment logic

2. **Task Dependency Validation**
   - Check that prerequisite tasks exist before creating dependencies
   - Detect circular dependencies and raise `CircularDependencyError`
   - Warn if dependency chain exceeds 3 levels (too sequential)

3. **Workload Balance Validation**
   - Calculate total effort per participant
   - Warn if any participant has >5 days of assigned work
   - Suggest rebalancing if variance exceeds 2 days between participants

4. **Timeline Validation**
   - Sum all task efforts and compare to 10-day sprint limit
   - Flag tasks with >2 day estimates for further decomposition
   - Warn if critical path exceeds 8 days (leaving no buffer)

### Error Recovery

- **Missing Participant Data**: Use default values, prompt captain to update
- **Invalid Task Format**: Skip malformed tasks, log errors, continue processing
- **Assignment Conflicts**: Detect double-assignments, prompt captain for resolution
- **File Write Failures**: Retry once, then fail gracefully with clear error message

## Testing Strategy

### Unit Testing

**Test Coverage Areas**:
1. Task decomposition logic
   - Test that global challenge generates expected number of subtasks
   - Verify subtasks cover all project requirements
   - Ensure no subtask is assigned to mentor

2. Participant filtering
   - Test mentor exclusion logic
   - Verify only eligible participants receive assignments
   - Test workload balancing algorithm

3. Dependency resolution
   - Test prerequisite chain calculation
   - Verify circular dependency detection
   - Test parallel task identification

**Test Framework**: pytest

**Example Test**:
```python
def test_mentor_excluded_from_assignments():
    participants = load_participants("PARTICIPANTS.md")
    tasks = generate_subtasks(global_challenge)
    assigned_tasks = assign_tasks(tasks, participants)
    
    mentor_tasks = [t for t in assigned_tasks if t.assigned_to == "Andrii Rogovskyi"]
    assert len(mentor_tasks) == 0, "Mentor should not be assigned any tasks"
```

### Integration Testing

**Scenarios**:
1. End-to-end task generation
   - Run decomposition script with sample global challenge
   - Verify all markdown files are created correctly
   - Check that TASKS.md is properly formatted

2. Assignment workflow
   - Simulate captain running assignment script
   - Verify participants receive balanced workload
   - Check that task files contain all required sections

### Manual Testing Checklist

- [ ] Run decomposition script and review generated TASKS.md
- [ ] Verify mentor is listed in "Not Assigned Tasks" section
- [ ] Check that each participant has 2-3 tasks maximum
- [ ] Confirm all subtask files have complete information
- [ ] Test that checkboxes in TASKS.md can be updated
- [ ] Verify total effort fits within 10-day sprint

## Implementation Notes

### Technology Choices

- **Python 3.9+**: For decomposition script (widely known, good for text processing)
- **Markdown**: For all task documentation (GitHub-native, version controlled)
- **GitHub Projects** (optional): For visual kanban board if team prefers
- **No database**: Keep everything in markdown for simplicity

### Async Collaboration Support

1. **Minimize Blocking Dependencies**: Design subtasks to be as independent as possible
2. **Clear Interfaces**: Define API contracts between frontend/backend tasks early
3. **Feature Flags**: Suggest using feature flags for incomplete integrations
4. **Stub Data**: Encourage creating mock data for parallel development

### Captain Workflow

1. Review global challenge in README
2. Run: `python scripts/decompose_tasks.py`
3. Review generated TASKS.md and subtask files
4. Adjust assignments in PARTICIPANTS.md if needed
5. Commit and push to repository
6. Announce task assignments in team channel
7. Track progress by monitoring PR activity and checkbox updates

### Participant Workflow

1. Check PARTICIPANTS.md for assigned tasks
2. Read detailed subtask file in docs/subtasks/
3. Update task status to "in progress" in TASKS.md
4. Work on implementation using AI assistance
5. Create PR when complete
6. Check checkbox in TASKS.md after PR is merged
7. Move to next assigned task

## Future Enhancements

- Automated assignment suggestions based on participant skills
- Integration with GitHub Issues for better tracking
- Time tracking and velocity metrics
- Automated progress reports in team channel
- Task template generator for future sprints
