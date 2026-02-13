---
description: Audit an existing implementation plan for completeness and alignment
---

Use the plan-auditor agent to audit the implementation plan.

The auditor will:
1. Detect project mode (greenfield vs brownfield)
2. Verify all PRD requirements are covered
3. Validate AppGraph alignment (all nodes/edges in impact subgraphs)
4. Check tech stack alignment with architecture specs
5. Assess task quality and success criteria
6. Identify gaps, drifts, and improvements needed

**For greenfield projects**, the auditor will additionally:
- Validate Phase 1 produces a deployable component
- Check for vertical slice organization and parallelization opportunities
- Verify architecture document references (no codebase file:line references)
- Assess cross-codebase coordination

Please perform a comprehensive audit of the implementation plan and generate a detailed audit report.
