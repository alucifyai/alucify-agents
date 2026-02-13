# Alucify Claude Code Agents

A comprehensive suite of specialized Claude Code agents for AppGraph-driven software development with systematic planning, implementation, testing, auditing, and refinement.

## Overview

This repository contains 7 specialized Claude Code agents that work together to implement a complete software development lifecycle based on AppGraph specifications, PRDs, and architecture documents. The agents follow a systematic approach to ensure code quality, implementation plan alignment, and comprehensive test coverage.

## Agents Overview

### Planning Agents

1. **implementation-planner** (Green)
   - Creates detailed implementation plans from PRD and AppGraph
   - Identifies new and modified nodes/edges
   - Breaks down features into phases and tasks with impact subgraphs
   - Output: `.alucify/plans/[feature]-implementation-plan.md`

2. **plan-auditor** (Blue)
   - Audits implementation plans for completeness and alignment
   - Validates PRD coverage, AppGraph alignment, tech stack compliance
   - Identifies gaps and drifts
   - Output: `.alucify/plans/[feature]-implementation-plan-audit.md`

3. **plan-refiner** (Purple)
   - Refines implementation plans based on audit findings
   - Addresses critical, major, and minor issues
   - Produces complete, standalone updated plans
   - Output: `.alucify/plans/[feature]-implementation-plan-v[X.Y].md`

### Implementation & Quality Assurance Agents

4. **task-implementer** (Orange)
   - Implements specific tasks from implementation plans
   - Follows 4-step process: Analysis → Implementation → Testing → Validation
   - Generates production code and comprehensive unit tests
   - Output: Code files, test files, and validation report

5. **test-executor** (Yellow)
   - Executes unit tests for implemented tasks
   - Collects comprehensive test statistics and coverage metrics
   - Analyzes test failures and performance
   - Output: `docs/code-generations/[task-id]-test-report.md`

6. **code-auditor** (Cyan)
   - Audits implemented code for compliance and quality
   - Validates implementation plan alignment, code quality, test coverage
   - Detects unrequired functionality and scope drift
   - Output: `docs/code-generations/[task-id]-implementation-audit.md`

7. **code-fixer** (Red)
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
├── appgraph-development/         # Plugin directory
│   ├── .claude-plugin/
│   │   └── plugin.json          # Plugin metadata
│   ├── agents/                  # Agent definitions
│   │   ├── implementation-planner.md
│   │   ├── plan-auditor.md
│   │   ├── plan-refiner.md
│   │   ├── task-implementer.md
│   │   ├── test-executor.md
│   │   ├── code-auditor.md
│   │   └── code-fixer.md
│   └── commands/                # Slash commands
│       ├── plan-create.md       # /plan-create - Create plan
│       ├── plan-audit.md        # /plan-audit - Audit plan
│       ├── plan-refine.md       # /plan-refine - Refine plan
│       ├── implement-task.md    # /implement-task - Implement a task
│       ├── test-execute.md      # /test-execute - Run tests
│       ├── code-audit.md        # /code-audit - Audit code
│       └── code-fix.md          # /code-fix - Fix issues
└── README.md                    # This file
```

### Marketplace Configuration

The `.claude-plugin/marketplace.json` defines the marketplace and references the plugin:
- **Marketplace name**: `alucify-marketplace`
- **Plugin**: `appgraph-development` (located in `./appgraph-development`)

### Plugin Configuration

The `appgraph-development/.claude-plugin/plugin.json` contains:
- Plugin metadata (name, description, version, author)
- All 7 agents are located in the `agents/` subdirectory
- Custom slash commands are located in the `commands/` subdirectory

## Installation

This repository contains a Claude Code marketplace with the `appgraph-development` plugin, which includes 7 specialized agents for AppGraph-driven development.

### Prerequisites

1. **Claude Code** installed and configured (with marketplace support)
2. **Project Structure** with required directories:
   ```
   your-project/
   ├── .alucify/
   │   ├── appgraph-project.json          # AppGraph specification
   │   ├── artifacts/
   │   │   ├── prd.md             # Product Requirements Document
   │   │   └── architecture.md    # Architecture specification
   │   └── plans/                 # Generated plans and audits
   └── docs/
       └── code-generations/      # Implementation docs and reports
   ```

   **Legacy Structure Support**: The agents also support the older folder structure for backward compatibility:
   ```
   your-project/
   ├── .alucify/
   │   ├── appgraph.json              # Legacy AppGraph path
   │   ├── prd.md                     # Legacy PRD path
   │   ├── architecture.md            # Legacy architecture path
   │   └── implementation-plans/      # Legacy plans folder
   ```
   Agents will automatically fall back to legacy paths if the new paths are not found.

### Installation Methods

#### Option 1: Install from GitHub (Recommended)

Add the marketplace from GitHub and install the plugin:

```bash
# In Claude Code, add the marketplace
/plugin marketplace add alucifyai/alucify-agents

# Install the appgraph-development plugin
/plugin install appgraph-development@alucify-marketplace
```

Or browse and install interactively:
```bash
/plugin
```
Then select "Browse Plugins" and install `appgraph-development` from the marketplace.

#### Option 2: Install from Local Directory

If you've cloned the repository locally:

1. Clone this repository:
   ```bash
   git clone https://github.com/alucifyai/alucify-agents.git
   ```

2. In Claude Code, add the local marketplace:
   ```bash
   /plugin marketplace add /path/to/alucify-agents
   ```

3. Install the plugin:
   ```bash
   /plugin install appgraph-development@alucify-marketplace
   ```

#### Option 3: Install from Git URL

Install directly from any Git repository:

```bash
# Add marketplace from Git URL
/plugin marketplace add https://github.com/alucifyai/alucify-agents.git

# Install the plugin
/plugin install appgraph-development@alucify-marketplace
```

#### Option 4: Project-Local Installation

Install the plugin locally in your project (useful for project-specific customizations):

1. Copy the plugin to your project:
   ```bash
   mkdir -p .claude/plugins
   cp -r /path/to/alucify-agents/appgraph-development .claude/plugins/
   ```

2. Claude Code will automatically discover the plugin

### Managing the Installation

View installed marketplaces:
```bash
/plugin marketplace list
```

Update marketplace metadata:
```bash
/plugin marketplace update alucify-marketplace
```

Uninstall the plugin:
```bash
/plugin uninstall appgraph-development
```

Remove the marketplace (this will also uninstall plugins from it):
```bash
/plugin marketplace remove alucify-marketplace
```

## Verification

To verify the plugin is installed correctly:

1. Check installed plugins:
   ```bash
   /plugin list
   ```
   You should see `appgraph-development` in the list.

2. Verify the agents are available:
   - Type `/` to see available commands
   - Use the Task tool and specify one of the agent names as `subagent_type`:
     - `implementation-planner`
     - `plan-auditor`
     - `plan-refiner`
     - `task-implementer`
     - `test-executor`
     - `code-auditor`
     - `code-fixer`

3. Check marketplace status:
   ```bash
   /plugin marketplace list
   ```
   You should see `alucify-marketplace` if installed from marketplace.

## Project Setup

Before using the agents, set up your project structure:

### 1. Create Required Directories

```bash
mkdir -p .alucify/plans .alucify/artifacts
mkdir -p docs/code-generations
```

### 2. Create Required Input Files

#### `.alucify/appgraph-project.json`

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

#### `.alucify/artifacts/prd.md`

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

#### `.alucify/artifacts/architecture.md`

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

### Slash Commands (Quick Access)

The plugin includes convenient slash commands for all agents:

#### Planning Commands
- `/plan-create` - Create an implementation plan
- `/plan-audit` - Audit an existing implementation plan
- `/plan-refine` - Refine a plan based on audit findings

#### Implementation Commands
- `/implement-task` - Implement a specific task
- `/test-execute` - Execute tests and generate test report
- `/code-audit` - Audit implemented code for quality and compliance
- `/code-fix` - Fix issues identified in audit and test reports

**Example Usage:**
```
/plan-create
```
This will launch the implementation-planner agent to create an implementation plan.

```
/implement-task
```
This will launch the task-implementer agent to implement a specific task.

### Complete Development Workflow

#### Phase 1: Planning

Use the individual commands or agents to complete the planning workflow:

1. **Create Implementation Plan**
   ```
   /plan-create
   ```
   Or using the Task tool:
   ```
   Use implementation-planner agent to create a detailed implementation plan
   ```
   - Agent reads PRD, AppGraph, and architecture
   - Generates phased implementation plan with tasks

2. **Audit the Plan**
   ```
   /plan-audit
   ```
   Or using the Task tool:
   ```
   Use plan-auditor agent to audit the implementation plan
   ```
   - Agent validates completeness and alignment
   - Identifies gaps and drifts

3. **Refine the Plan** (if needed)
   ```
   /plan-refine
   ```
   Or using the Task tool:
   ```
   Use plan-refiner agent to address audit findings
   ```
   - Agent produces updated plan version
   - Addresses all critical, major, and minor issues

#### Phase 2: Implementation (for each task)

Use the individual commands or agents to complete the implementation workflow:

4. **Implement Task**
   ```
   /implement-task
   ```
   Or using the Task tool:
   ```
   Use task-implementer agent for Phase X, Task X.Y
   ```
   - Agent reads implementation plan
   - Implements code and tests
   - Generates validation report

5. **Execute Tests**
   ```
   /test-execute
   ```
   Or using the Task tool:
   ```
   Use test-executor agent to run tests and collect statistics
   ```
   - Agent executes unit tests
   - Collects coverage metrics
   - Generates test report

6. **Audit Implementation**
   ```
   /code-audit
   ```
   Or using the Task tool:
   ```
   Use code-auditor agent to audit the implementation
   ```
   - Agent reviews code quality and compliance
   - Identifies gaps, drifts, and issues
   - Generates audit report

7. **Fix Issues**
   ```
   /code-fix
   ```
   Or using the Task tool:
   ```
   Use code-fixer agent to address audit and test findings
   ```
   - Agent fixes critical, high, and medium priority issues
   - Traces root causes via impact subgraph
   - Generates gap resolution report

8. **Iterate** - Repeat steps 5-7 until all issues resolved

9. **Next Task** - Return to step 4 for next task

### Example Agent Invocations

#### Using Slash Commands (Easiest)

```bash
# Phase 1: Planning
/plan-create     # Create implementation plan
/plan-audit      # Audit the plan
/plan-refine     # Refine based on audit findings

# Phase 2: Implementation (for each task)
/implement-task  # Implement a specific task
/test-execute    # Execute tests
/code-audit      # Audit code
/code-fix        # Fix issues
```

#### Using Task Tool in Claude Code

```typescript
// Phase 1: Planning
task("Create implementation plan for user authentication feature",
     "implementation-planner")
task("Audit the implementation plan for user authentication",
     "plan-auditor")
task("Refine the implementation plan based on audit findings",
     "plan-refiner")

// Phase 2: Implementation (for each task)
task("Implement Phase 1, Task 1.1: Create authentication component",
     "task-implementer")

// Execute tests
task("Execute tests for Phase 1, Task 1.1 implementation",
     "test-executor")

// Audit implementation
task("Audit code implementation for Phase 1, Task 1.1",
     "code-auditor")

// Fix issues
task("Fix issues identified in audit and test reports for Phase 1, Task 1.1",
     "code-fixer")
```

#### Manual Usage in Chat

You can also use slash commands or describe what you want to accomplish:

```
# Using slash commands
/plan-create     # Create implementation plan
/plan-audit      # Audit plan
/plan-refine     # Refine plan
/implement-task  # Implement a task
/test-execute    # Execute tests
/code-audit      # Audit code
/code-fix        # Fix issues

# Or describe in natural language - Claude Code will use the appropriate agent:

# Planning
"Please create an implementation plan for the user authentication feature"
(Uses implementation-planner)

"Audit the implementation plan and check for gaps"
(Uses plan-auditor)

"Refine the implementation plan based on the audit findings"
(Uses plan-refiner)

# Implementation
"Implement Phase 1, Task 1.1 from the implementation plan"
(Uses task-implementer)

"Run tests for the implementation and generate a report"
(Uses test-executor)

"Audit the code I just implemented for Task 1.1"
(Uses code-auditor)

"Fix all critical and high priority issues from the audit report"
(Uses code-fixer)
```

## Agent Interaction Flow

### Development Workflow

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
                         ↓
┌─────────────────────────────────────────────────────────┐
│            Implementation Phase (per task)               │
├─────────────────────────────────────────────────────────┤
│  4. task-implementer → Code + Tests                     │
│              ↓                                           │
│  5. test-executor → Test Report                         │
│              ↓                                           │
│  6. code-auditor → Audit Report                         │
│              ↓                                           │
│  7. code-fixer → Fixed Code + Gap Resolution Report     │
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
│   ├── appgraph-project.json     # AppGraph specification (REQUIRED)
│   ├── artifacts/                # Input artifacts (REQUIRED)
│   │   ├── prd.md                # Product Requirements (REQUIRED)
│   │   └── architecture.md       # Tech stack & patterns (REQUIRED)
│   └── plans/                    # Generated by agents
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

### Plugin Not Found
- Verify the marketplace is added: `/plugin marketplace list`
- Check if the plugin is installed: `/plugin list`
- Try reinstalling: `/plugin install appgraph-development@alucify-marketplace`
- Verify plugin structure has `.claude-plugin/plugin.json` file

### Agents Not Available
- Ensure the plugin is enabled: `/plugin list` (check status)
- Verify agents directory exists in the plugin: `appgraph-development/agents/`
- Check agent file extensions are `.md`
- Restart Claude Code after installation

### Marketplace Cannot Be Added
- Verify the repository URL or path is correct
- Ensure `.claude-plugin/marketplace.json` exists in repository root
- Validate marketplace.json structure: `claude plugin validate .` (in repo directory)
- Check network connectivity for remote repositories

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

## Acknowledgments

Built for AppGraph-driven development with Claude Code to ensure systematic, high-quality software implementation with comprehensive testing and quality assurance.
