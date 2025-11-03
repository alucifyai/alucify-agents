---
description: Fix issues identified in audit and test reports
---

Use the code-fixer agent to address issues found in audits and tests.

The fixer will:
1. Read audit and test reports to identify all issues
2. Prioritize issues (critical, high, medium priority)
3. Trace root causes via AppGraph impact subgraph
4. Fix issues systematically, addressing root causes
5. Handle pre-existing and cascading issues
6. Generate a gap resolution report

Please specify which task's issues to fix and the fixer will systematically resolve all problems.
