---
name: workflow-templates
description: Pre-configured workflows for common tasks. Use when starting bug fixes, feature additions, or refactoring to save setup tokens.
---

# Workflow Templates

Pre-configured workflows for common development tasks. Use these to save setup time and tokens.

## When to Use

- Starting a bug fix
- Adding a new feature
- Refactoring existing code
- Code review tasks
- Documentation updates

## Available Workflows

### 1. Bug Fix Workflow

**Trigger:** User says "fix bug", "debug", "issue"

**Steps:**
1. Understand the bug (ask for error message, expected vs actual behavior)
2. Reproduce the issue (create minimal test case)
3. Locate the bug (use grep/glob to find relevant code)
4. Fix the bug (minimal change)
5. Verify the fix (run tests, check edge cases)
6. Document the fix (update comments if needed)

**Token Savings:** 15-20% by skipping exploration phase

---

### 2. Feature Addition Workflow

**Trigger:** User says "add feature", "implement", "new functionality"

**Steps:**
1. Clarify requirements (ask for acceptance criteria)
2. Design approach (propose 2-3 options, pick simplest)
3. Create implementation plan (break into small steps)
4. Implement incrementally (one step at a time, verify each)
5. Write tests (cover happy path + edge cases)
6. Update documentation (if needed)

**Token Savings:** 20-25% by avoiding re-planning

---

### 3. Refactoring Workflow

**Trigger:** User says "refactor", "clean up", "improve code"

**Steps:**
1. Identify target (specific function, module, or pattern)
2. Analyze current code (understand dependencies)
3. Propose refactoring approach (explain benefits)
4. Apply refactoring (small, atomic changes)
5. Verify no regressions (run tests)
6. Update documentation (if API changed)

**Token Savings:** 15-20% by focused scope

---

### 4. Code Review Workflow

**Trigger:** User says "review", "check code", "audit"

**Steps:**
1. Understand context (what is this code for?)
2. Check for bugs (logic errors, edge cases)
3. Check for security issues (input validation, auth)
4. Check for performance issues (loops, queries)
5. Check for code quality (naming, structure, DRY)
6. Provide actionable feedback (specific, with examples)

**Token Savings:** 10-15% by structured approach

---

### 5. Documentation Workflow

**Trigger:** User says "document", "write docs", "update README"

**Steps:**
1. Identify audience (developers, users, operators)
2. Determine scope (overview, API reference, tutorial)
3. Extract information from code (use grep/read)
4. Write documentation (clear, concise, with examples)
5. Verify accuracy (test examples if possible)
6. Update related docs (cross-references)

**Token Savings:** 20-25% by avoiding redundant reads

---

## Tool Call Optimization Guidelines

### Batch Operations

**❌ Bad:**
```
read file1
read file2
read file3
```

**✅ Good:**
```bash
cat file1 file2 file3 | head -200
```

**Savings:** 50-70% tokens for file reads

---

### Selective File Reading

**❌ Bad:**
```
read file="large.py"  # 1000 lines
```

**✅ Good:**
```bash
sed -n '100,200p' large.py  # Only 100 lines
```

**Savings:** 90% tokens for large files

---

### Parallel Tool Calls

**✅ Good:**
Call multiple independent tools in same message:
- read file1
- grep pattern
- glob files

**Savings:** 20-30% latency (no token savings, but faster)

---

### Avoid Duplicate Calls

**❌ Bad:**
```
read file.py  # Line 1-100
read file.py  # Line 1-100 (duplicate)
```

**✅ Good:**
```
read file.py  # Line 1-100
# Use cached result
```

**Savings:** 100% for duplicate reads

---

## Usage

When starting a task, mention the workflow:
- "Fix bug using bug-fix workflow"
- "Add feature using feature-addition workflow"
- "Refactor using refactoring workflow"

Or just describe the task and I'll auto-select the appropriate workflow.

---

## Token Savings Summary

| Workflow | Savings | When to Use |
|----------|---------|-------------|
| Bug Fix | 15-20% | Any bug/issue |
| Feature Addition | 20-25% | New functionality |
| Refactoring | 15-20% | Code cleanup |
| Code Review | 10-15% | Review/audit |
| Documentation | 20-25% | Writing docs |
| **Tool Optimization** | **10-30%** | **Always** |

**Total Potential Savings:** 25-55% per session
