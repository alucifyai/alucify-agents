---
description: Create an implementation plan from PRD, AppGraph, and architecture specs
---

Use the implementation-planner agent to create a detailed implementation plan.

The planner will:
1. Detect project mode (greenfield vs brownfield)
2. Analyze your PRD, AppGraph, and architecture specifications
3. Identify all new and modified components (all new for greenfield)
4. Break down the implementation into phases and tasks
5. Specify impact subgraphs, tech stack, and success criteria for each task

**For greenfield projects**, the planner will additionally:
- Prioritize phases that produce deployable components early
- Structure phases for incremental deployment and end-to-end testing
- Identify parallelization opportunities between phases
- Derive all patterns from architecture specifications

Please create a comprehensive implementation plan for the feature.
