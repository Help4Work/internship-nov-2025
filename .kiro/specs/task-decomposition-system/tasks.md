# Implementation Plan

## Overview
This plan implements a task decomposition system that breaks down the bordfu+APITable integration project into subtasks for participants, excluding the mentor from assignments.

---

- [ ] 1. Create project structure and participant registry
  - Set up docs/subtasks/ directory for individual task files
  - Create PARTICIPANTS.md with participant table structure including mentor exclusion section
  - Add participant data from README (Mumba Patrick, Kidus Messele, Lesia Soloviova)
  - Mark Andrii Rogovskyi as mentor in separate "Not Assigned Tasks" section
  - _Requirements: 2.1, 2.2, 2.3, 2.4_

- [ ] 2. Implement task decomposition script
- [ ] 2.1 Create Python script foundation
  - Create scripts/decompose_tasks.py with main entry point
  - Implement command-line argument parsing for global challenge input
  - Add configuration for sprint duration (10 days) and max tasks per participant (3)
  - _Requirements: 1.1, 6.2, 6.3_

- [ ] 2.2 Implement participant management
  - Write parse_participants() function to read PARTICIPANTS.md
  - Create Participant dataclass with name, github, skills, is_mentor flag
  - Implement filter_eligible_participants() that excludes mentor (Andrii Rogovskyi)
  - Add validation to ensure mentor is never included in assignment pool
  - _Requirements: 2.1, 2.2, 2.3, 2.4_

- [ ] 2.3 Implement task generation logic
  - Create Task dataclass with id, title, category, effort, prerequisites, assigned_to
  - Write generate_subtasks() function that creates tasks for bordfu+APITable project
  - Implement task categories: Environment Setup, Backend Integration, Docker Configuration, Testing
  - Generate specific subtasks: fork repo, setup env, replace Airtable SDK, create docker-compose, etc.
  - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5_

- [ ] 2.4 Implement task assignment algorithm
  - Write assign_tasks() function that distributes tasks to eligible participants only
  - Implement workload balancing to ensure even distribution (max 3 tasks per person)
  - Add validation check that raises error if mentor is assigned any task
  - Calculate and validate total effort fits within 10-day sprint
  - _Requirements: 2.3, 4.5, 5.5, 6.2, 6.5_

- [ ] 2.5 Implement dependency management
  - Add identify_dependencies() function to detect task prerequisites
  - Implement check for circular dependencies
  - Create get_parallel_tasks() to identify tasks that can start immediately
  - Flag tasks with >2 day effort for potential decomposition
  - _Requirements: 4.2, 5.1, 5.2, 5.3, 6.4_

- [ ] 3. Create markdown file generators
- [ ] 3.1 Implement TASKS.md generator
  - Write create_master_task_list() function
  - Generate markdown with task categories and checkbox format
  - Include assignment info (participant name) and effort estimates
  - Add progress counter at bottom (X/Y tasks completed)
  - _Requirements: 1.1, 1.2, 1.3, 4.1, 4.4_

- [ ] 3.2 Implement individual subtask file generator
  - Create generate_subtask_file() function using template from design
  - Include sections: Objective, Effort, Prerequisites, Acceptance Criteria
  - Add Technical Guidance with files to modify and technologies
  - Include Suggested AI Prompts section with example questions
  - Add Resources section with links to bordfu, APITable, and Docker docs
  - Write files to docs/subtasks/task-{category}-{number}.md
  - _Requirements: 3.1, 3.2, 3.3, 3.4, 3.5, 7.1, 7.2, 7.3, 7.4, 7.5_

- [ ] 3.3 Implement PARTICIPANTS.md updater
  - Write update_participants_file() to add current assignments
  - Update "Current Assignment" column with task IDs
  - Ensure mentor section remains separate and unchanged
  - Add assignment timestamp or status
  - _Requirements: 2.1, 2.2, 2.3, 4.5_

- [ ] 4. Add bordfu project-specific task definitions
- [ ] 4.1 Define Environment Setup tasks
  - Create task 1.1: Fork and clone bordfu repository (0.5 days)
  - Create task 1.2: Set up local development environment (1 day)
  - Create task 1.3: Configure APITable instance (1 day)
  - Include acceptance criteria and technical guidance for each
  - _Requirements: 1.1, 1.2, 1.3, 1.4, 3.1, 3.2, 3.3, 3.4_

- [ ] 4.2 Define Backend Integration tasks
  - Create task 2.1: Replace Airtable SDK with APITable SDK (2 days)
  - Create task 2.2: Update database connection logic (1.5 days)
  - Create task 2.3: Implement CRUD operations for APITable (2 days)
  - Add prerequisites (task 1.3 must complete before 2.1)
  - Include specific file paths and API documentation links
  - _Requirements: 1.1, 1.2, 1.3, 1.4, 3.3, 3.4, 7.1, 7.3, 7.4_

- [ ] 4.3 Define Docker Configuration tasks
  - Create task 3.1: Create docker-compose.yml for bordfu (1 day)
  - Create task 3.2: Integrate APITable service in docker-compose (1 day)
  - Create task 3.3: Configure networking between services (0.5 days)
  - Mark tasks 3.1 and 3.2 as parallelizable
  - Add Docker Compose documentation and example configurations
  - _Requirements: 1.1, 1.2, 1.3, 3.3, 5.1, 5.2, 7.4_

- [ ] 4.4 Define Testing & Documentation tasks
  - Create task 4.1: Test end-to-end application flow (1 day)
  - Create task 4.2: Update README with setup instructions (0.5 days)
  - Set prerequisites: all previous tasks must complete
  - Include testing checklist and documentation template
  - _Requirements: 1.1, 1.2, 1.3, 3.1, 3.2, 3.3_

- [ ] 5. Implement validation and error handling
- [ ] 5.1 Add mentor assignment validation
  - Implement validate_no_mentor_assignments() function
  - Raise MentorAssignmentError if Andrii Rogovskyi appears in any assignment
  - Add logging for all assignment operations
  - _Requirements: 2.2, 2.3, 2.4_

- [ ] 5.2 Add workload and timeline validation
  - Implement validate_workload_balance() to check effort distribution
  - Add validate_sprint_timeline() to ensure total effort ≤ 10 days
  - Implement validate_task_dependencies() to detect circular dependencies
  - Generate warnings for tasks >2 days or unbalanced assignments
  - _Requirements: 4.2, 6.1, 6.2, 6.4, 6.5_

- [ ] 5.3 Add error recovery mechanisms
  - Implement graceful handling for missing participant data
  - Add retry logic for file write operations
  - Create detailed error messages for validation failures
  - Log all errors to console with actionable suggestions
  - _Requirements: 1.1, 2.1, 4.5_

- [ ] 6. Create script execution and output
- [ ] 6.1 Implement main execution flow
  - Create main() function that orchestrates all steps
  - Add progress indicators during script execution
  - Implement dry-run mode for preview without file creation
  - Generate summary report of created tasks and assignments
  - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5_

- [ ] 6.2 Add command-line interface
  - Implement argparse for --input, --dry-run, --output-dir flags
  - Add --help documentation with usage examples
  - Support reading global challenge from stdin or file
  - _Requirements: 1.1, 7.1_

- [ ] 6.3 Generate all output files
  - Execute create_master_task_list() to generate TASKS.md
  - Execute generate_subtask_file() for each task
  - Execute update_participants_file() to update PARTICIPANTS.md
  - Print summary with file paths and task count
  - _Requirements: 1.1, 1.2, 1.3, 3.1, 3.2, 3.3, 3.4, 4.1, 4.4_

- [ ]* 7. Add testing and documentation
- [ ]* 7.1 Write unit tests for core functions
  - Test parse_participants() with sample PARTICIPANTS.md
  - Test filter_eligible_participants() ensures mentor exclusion
  - Test assign_tasks() workload balancing
  - Test validate_no_mentor_assignments() catches violations
  - _Requirements: 2.2, 2.3, 2.4_

- [ ]* 7.2 Write integration tests
  - Test end-to-end script execution with sample input
  - Verify all markdown files are created correctly
  - Validate TASKS.md format and content
  - Check that mentor appears only in "Not Assigned Tasks" section
  - _Requirements: 1.1, 2.2, 2.3, 2.4_

- [ ]* 7.3 Create usage documentation
  - Write README section explaining how to run decomposition script
  - Document captain workflow for task assignment
  - Document participant workflow for task execution
  - Add troubleshooting section for common issues
  - _Requirements: 3.4, 7.2_

---

## Notes
- All tasks reference specific requirements from requirements.md
- Tasks are ordered to build incrementally
- Parallel work possible: tasks 2.x can be developed simultaneously, tasks 4.x can be defined in parallel
- Testing tasks marked optional (*) to focus on core functionality first
- Total estimated effort: ~8-9 days for implementation
