# Alucify Claude Code Agents

A comprehensive suite of specialized Claude Code agents for AppGraph-driven software development with systematic planning, implementation, testing, auditing, and refinement.

## Overview

This repository contains 6 specialized Claude Code agents that work together to implement a complete software development lifecycle based on AppGraph specifications, PRDs, and architecture documents. The agents follow a systematic approach to ensure code quality and implementation plan alignment

## Agents Overview

### Planning & Analysis Agents

1. **implementation-planner**
   - Creates detailed implementation plans from AppGraph, PRD and Architecture document
   - Identifies new and modified nodes/edges
   - Breaks down features into phases and tasks with impact subgraphs
   - Output: `.alucify/implementation-plans/[feature]-implementation-plan.md`

2. **plan-auditor**
   - Audits implementation plans for completeness and alignment
   - Validates PRD coverage, AppGraph alignment, tech stack compliance
   - Identifies gaps and drifts
   - Output: `.alucify/implementation-plans/[feature]-implementation-plan-audit.md`

3. **plan-refiner** 
   - Refines implementation plans based on audit findings
   - Addresses critical, major, and minor issues
   - Produces complete, standalone updated plans
   - Output: `.alucify/implementation-plans/[feature]-implementation-plan-v[X.Y].md`

### Implementation & Quality Assurance Agents

4. **task-implementer**
   - Implements specific tasks from implementation plans
   - Follows 4-step process: Analysis → Implementation → Testing → Validation
   - Generates production code and comprehensive unit tests
   - Output: Code files, test files, and validation report

5. **code-auditor**
   - Audits implemented code for compliance and quality
   - Validates implementation plan alignment, code quality, test coverage
   - Detects unrequired functionality and scope drift
   - Output: `docs/code-generations/[task-id]-implementation-audit.md`

6. **code-fixer** 
   - Fixes issues identified in audit and test reports
   - Traces root causes via AppGraph impact subgraph
   - Addresses critical, high, and medium priority issues iteratively
   - Handles pre-existing and related issues
   - Output: `docs/code-generations/[task-id]-gap-resolution-report.md`

## Repository Structure

This repository is organized as a Claude Code marketplace containing a plugin:

```
alucify-agents/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace definition
├── appgraph-development/         # Agent directory
│   ├── agents/                  # Agent definitions
│   │   ├── implementation-planner.md
│   │   ├── plan-auditor.md
│   │   ├── plan-refiner.md
│   │   ├── task-implementer.md
│   │   ├── code-auditor.md
│   │   └── code-fixer.md
└── README.md                    # This file
```

## Installation

This repository contains a Claude Code marketplace with the `appgraph-development` plugin, which includes 9 specialized agents for AppGraph-driven development.

### Prerequisites

1. **Claude Code** installed and configured (with marketplace support)
2. **Project Structure** with required directories:
   ```
   your-project/
   ├── .alucify/
   │   ├── appgraph.json          # AppGraph specification
   │   ├── prd.md                 # Product Requirements Document
   │   ├── architecture.md        # Architecture specification
   │   └── implementation-plans/  # Generated plans and audits
   └── docs/
       └── code-generations/      # Implementation docs and reports
   ```

### Installation Steps

1. Clone this repository:
   ```bash
   git clone --branch cg-langbuilder https://github.com/your-org/alucify-agents.git
   ```
#### Global Installation
2. Copy agents to your Claude Code agents directory:
   ```bash
   # For macOS/Linux
   mkdir -p ~/.claude-code/agents
   cp -r alucify-agents/* ~/.claude-code/agents/

   # For Windows (PowerShell)
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude-code\agents\appgraph-development"
   Copy-Item -Recurse -Force .\appgraph-development\agents\* "$env:USERPROFILE\.claude-code\agents\appgraph-development\"
   ```

3. Restart Claude Code or reload agents

#### Project-Local Installation

Install agents locally in your project:

2. Create agents directory in your project:
   ```bash
   mkdir -p .claude/agents
   ```

3. Copy agent files:
   ```bash
   cp -r path/to/alucify-agents/* .claude/agents/
   ```

3. Claude Code will automatically discover these agents

## Verification

To verify agents are installed correctly:

1. Open Claude Code
2. Type `@` to see available commands
3. Look for the agent names in the available agents list
4. Or use the Task tool and specify one of the agent names as `subagent_type`

## Project Setup

Before using the agents, set up your project structure:

### 1. Create Required Directories

```bash
mkdir -p .alucify/implementation-plans
mkdir -p docs/code-generations
```

### 2. Create Required Input Files

#### `.alucify/appgraph.json`

Your AppGraph specification with nodes and edges:

```json
{
  "nodes": [
    {
      "id": "node-1",
      "type": "component",
      "name": "UserAuthComponent",
      "status": "new",
      "properties": {
        "description": "User authentication component"
      }
    }
  ],
  "edges": [
    {
      "id": "edge-1",
      "source": "node-1",
      "target": "node-2",
      "type": "depends-on"
    }
  ]
}
```

#### `.alucify/prd.md`

Your Product Requirements Document:

```markdown
# Feature Name

## Overview
[Feature description]

## Epics
### Epic 1: User Authentication
[Epic details]

## User Stories
- As a user, I want to...

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
```

#### `.alucify/architecture.md`

Your architecture and tech stack specification:

```markdown
# Architecture Specification

## Tech Stack
- Framework: React 18
- Language: TypeScript 5.x
- State Management: Zustand
- Testing: Vitest

## Design Patterns
- Component pattern: Functional components with hooks
- Service pattern: Async/await with error boundaries

## File Structure
- Components: `src/components/`
- Services: `src/services/`
- Tests: `src/**/__tests__/`
```

## Usage

### Complete Development Workflow

#### Phase 1: Planning

1. **Create Implementation Plan**
   ```
   Use implementation-planner agent to create a detailed implementation plan
   ```
   - Agent reads PRD, AppGraph, and architecture
   - Generates phased implementation plan with tasks

2. **Audit the Plan**
   ```
   Use plan-auditor agent to audit the implementation plan
   ```
   - Agent validates completeness and alignment
   - Identifies gaps and drifts

3. **Refine the Plan** (if needed)
   ```
   Use plan-refiner agent to address audit findings
   ```
   - Agent produces updated plan version
   - Addresses all critical, major, and minor issues

#### Phase 2: Implementation (for each task)

4. **Implement Task**
   ```
   Use task-implementer agent for next task ( or Phase X, Task X.Y)
   ```
   - Agent reads implementation plan
   - Implements code and tests
   - Run tests
   - Generates validation report

5. **Audit Implementation**
   ```
   Use code-auditor agent to audit the implementation
   ```
   - Agent reviews code quality and compliance
   - Identifies gaps, drifts, and issues
   - Generates audit report

6. **Fix Issues**
   ```
   Use code-fixer agent to address audit and test findings
   ```
   - Agent fixes critical, high, and medium priority issues
   - Traces root causes via impact subgraph
   - Generates gap resolution report

7. **Iterate** - Repeat steps 5-6 until all issues resolved

9. **Next Task** - Return to step 4 for next task

### Example Agent Invocations

#### Using Task Tool in Claude Code

```typescript
// Create implementation plan
task("Create implementation plan for user authentication feature",
     "implementation-planner")

// Audit the plan
task("Audit the implementation plan for user authentication",
     "plan-auditor")

// Implement a specific task
task("Implement Phase 1, Task 1.1: Create authentication component",
     "task-implementer")

// Audit implementation
task("Audit code implementation for Phase 1, Task 1.1",
     "code-auditor")

// Fix issues
task("Fix issues identified in audit and test reports for Phase 1, Task 1.1",
     "code-fixer")
```

#### Manual Usage in Chat

You can also describe what you want to accomplish, and Claude Code will automatically use the appropriate agent:

```
"Please create an implementation plan for the user authentication feature
based on the PRD and AppGraph"

"Audit the implementation plan and check for gaps"

"Implement Phase 1, Task 1.1 from the implementation plan"

"Run tests for the implementation and generate a report"

"Audit the code I just implemented for Task 1.1"

"Fix all critical and high priority issues from the audit report"
```

## Agent Interaction Flow

```
┌─────────────────────────────────────────────────────────┐
│                    Planning Phase                        │
├─────────────────────────────────────────────────────────┤
│  PRD + AppGraph + Architecture                          │
│              ↓                                           │
│  1. implementation-planner → Implementation Plan        │
│              ↓                                           │
│  2. plan-auditor → Audit Report                         │
│              ↓                                           │
│  3. plan-refiner → Refined Plan (if needed)             │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│            Implementation Phase (per task)               │
├─────────────────────────────────────────────────────────┤
│  4. task-implementer → Code + Tests                     │
│              ↓                                           │
│  5. code-auditor → Audit Report                         │
│              ↓                                           │
│  6. code-fixer → Fixed Code + Gap Resolution Report     │
│              ↓                                           │
│  ┌───────────────────────┐                              │
│  │ Issues remaining?     │                              │
│  └───────────────────────┘                              │
│           ↓          ↓                                   │
│         Yes         No                                   │
│          ↓           ↓                                   │
│    Back to 5    Next Task                               │
└─────────────────────────────────────────────────────────┘
```

## Key Features

### Implementation Plan Fidelity
- All agents maintain strict alignment with implementation plans
- Impact subgraphs track AppGraph nodes and edges for each task
- Success criteria ensure completeness
- Tech stack alignment ensures consistency

### Root Cause Analysis
- code-fixer traces issues through AppGraph impact subgraph
- Fixes root causes, not symptoms
- Addresses cascading impacts and related issues

### Iterative Refinement
- Multiple audit and fix cycles ensure quality
- Strategic iteration management for large fixes
- Clear progress tracking and handoffs

### Comprehensive Testing
- Unit test generation follows existing patterns
- Coverage tracking and gap identification
- Test execution with detailed statistics
- Validation against success criteria

### Quality Assurance
- Multiple audit points (plan, code, tests)
- Gap and drift detection
- Priority-based issue resolution
- Pre-existing issue identification

## Directory Structure Reference

```
your-project/
├── .alucify/                      # Alucify configuration
│   ├── appgraph.json             # AppGraph specification (REQUIRED)
│   ├── prd.md                    # Product Requirements (REQUIRED)
│   ├── architecture.md           # Tech stack & patterns (REQUIRED)
│   └── implementation-plans/      # Generated by agents
│       ├── [feature]-implementation-plan.md
│       ├── [feature]-implementation-plan-audit.md
│       └── [feature]-implementation-plan-v1.1.md
│
├── docs/
│   └── code-generations/          # Generated by agents
│       ├── [task-id]-test-report.md
│       ├── [task-id]-implementation-audit.md
│       └── [task-id]-gap-resolution-report.md
│
├── src/                           # Your source code
│   ├── components/
│   ├── services/
│   └── **/__tests__/             # Test files
│
└── .claude/                       # Optional: Project-local agents
    └── agents/
        └── *.md                   # Agent definitions
```

## Best Practices

### 1. Follow the Workflow Sequentially
- Complete planning phase before implementation
- Implement tasks in dependency order
- Don't skip audit and fix cycles

### 2. Maintain Input Documents
- Keep AppGraph up-to-date with implementation
- Update PRD when requirements change
- Maintain accurate architecture specification

### 3. Review Agent Outputs
- Review implementation plans before implementing
- Review audit reports and understand issues
- Make manual decisions when agents flag intervention needs

### 4. Iterate Until Complete
- Run test-executor → code-auditor → code-fixer cycle until all issues resolved
- Don't proceed to next task with unresolved critical issues
- Address manual intervention items promptly

### 5. Use Task Identifiers
- Always reference Phase X, Task X.Y when working on implementations
- Maintain clear task-to-code traceability
- Use consistent naming for reports

## Troubleshooting

### Agents Not Found
- Verify agents are in correct directory (`~/.claude-code/agents/` or `.claude/agents/`)
- Check file extensions are `.md`
- Restart Claude Code

### Missing Input Files
- Ensure `.alucify/` directory exists with required files
- Validate AppGraph JSON is well-formed
- Check file paths in error messages

### Agent Fails to Read Files
- Verify directory structure matches expected paths
- Check file permissions
- Ensure files are not empty

### Context Size Issues with code-fixer
- Agent will automatically break into iterations
- Follow iteration guidance in gap resolution reports
- Run code-fixer multiple times for large fix sets

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with real projects
5. Submit a pull request

## License

[Specify your license]

## Support

For issues, questions, or contributions:
- GitHub Issues: [your-repo-url]/issues
- Documentation: [your-docs-url]
- Email: [your-email]

## Version History

- **v1.0.0** - Initial release with 7 agents
  - Planning agents: implementation-planner, plan-auditor, plan-refiner
  - Implementation agents: task-implementer, test-executor, code-auditor, code-fixer

## Acknowledgments

Built for AppGraph-driven development with Claude Code to ensure systematic, high-quality software implementation with comprehensive testing and quality assurance.
