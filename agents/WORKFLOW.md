# Agent Workflow: Request -> Agent -> Test -> Refine

This document describes the workflow for using the project’s agents:

- DevAgent  
- TestAgent  
- BugAgent  
- CleanupAgent  

The purpose of this workflow is to take a high-level request and turn it into a safe, incremental change that is:

- Planned  
- Implemented  
- Tested  
- Debugged (if needed)  
- Cleaned up

---

## 1. Overview of the Loop

1. Request – You describe the goal.  
2. Plan & Implement – DevAgent plans and proposes small, safe changes.  
3. Test – You run tests or use TestAgent to generate them.  
4. Debug – If tests fail, BugAgent diagnoses the issue and proposes a minimal fix.  
5. Refine & Cleanup – DevAgent and CleanupAgent polish the solution.  
6. Repeat – Move to the next small step.

---

## 2. Request: Define the Goal

Start with a clear high-level description of what you want.

**Template:**

Describe the objective as:

- What you want  
- Where it applies (backend, frontend, both)  
- Any constraints  

**Example Prompt:**

Use DevAgent.  
Goal: {{your goal here}}  
Follow the workflow in `/agents/WORKFLOW.md`.

---

## 3. Plan & Implement with DevAgent

After giving the goal, call DevAgent.

**Prompt:**

Use DevAgent.  
Goal: {{your goal}}  
Follow `/agents/DevAgent.md`.  
Start by outlining the plan, then provide only the first small step.

**DevAgent should output:**

1. Plan – a short outline of steps.  
2. Changes – for step 1 only (summary, files, full code blocks).  
3. Verification – commands to run (composer test, composer lint, etc.).  
4. Next Step – waits for confirmation.

**You do:**

- Review changes.  
- Apply or request revisions.  
- Move to testing.

---

## 4. Create or Update Tests with TestAgent (Optional but Recommended)

If tests are missing or incomplete, call TestAgent.

**Prompt:**

Use TestAgent.  
Context: DevAgent implemented {{short description of change}}.  
Follow `/agents/TestAgent.md`.  
1. Propose a test plan.  
2. Then generate complete test files.
3. Insure we have full coverage of the changes.

**TestAgent should produce:**

- Test plan  
- Full test files  
- Verification commands (for example: `composer test`, `composer test:coverage`, `npm run test`)

Then run the tests yourself.

---

## 5. Run Tests and Capture Output

Run whichever commands DevAgent or TestAgent recommended, for example:

```bash
composer test
composer lint
composer analyze
npm run test
npm run lint
```

- If everything passes, go to Step 7.  
- If anything fails, go to **BugAgent**.

---

## 6. Debug Failures with BugAgent

If tests fail or exceptions occur, call BugAgent.

**Prompt:**

Use BugAgent.  
Follow `/agents/BugAgent.md`.  
Here is the failing output:

```text
{{paste failing test or error output here}}
```

You may also briefly describe what was just changed.

**BugAgent should produce:**

1. Hypotheses – plausible causes.  
2. Information requests – specific files, lines, or additional output needed.  
3. Diagnosis – explanation of the most likely root cause.  
4. Fix – minimal code changes with full code blocks.  
5. Verification – commands to re-run.

**You do:**

- Provide any requested files or context.  
- Apply the fix (or ask for a refinement).  
- Re-run the suggested tests.  
- If new failures occur, you can loop with BugAgent again.

---

## 7. Refine & Cleanup

Once the code is working and all tests pass:

### 7.1. Optional small refactor with DevAgent

If the change works but could be structured more cleanly (without changing behavior), ask DevAgent for a small follow-up refactor.

**Prompt:**

Use DevAgent.  
Goal: Lightly refactor the updated {{service/component/etc.}} for clarity while keeping behavior unchanged.  
Follow `/agents/DevAgent.md`.  
Work in a single, small step.

### 7.2. Cleanup with CleanupAgent

Use CleanupAgent for formatting, docblocks, imports, and documentation.

**Prompt:**

Use CleanupAgent.  
Follow `/agents/CleanupAgent.md`.  
Audit and clean up the files modified during this change.  
Only perform non-behavioral cleanup.

**CleanupAgent should provide:**

- Checklist of cleanup tasks.  
- Full updated files.  
- Suggested documentation updates (for example in `/docs/`).  
- Verification commands (composer lint, composer analyze, npm run lint, etc.).

Then run the suggested commands.

---

## 8. Finalize & Commit

When everything is tested and cleaned up:

1. Review your diff (`git diff`).  
2. Run tests and lint one final time.  
3. Commit.

Example:

```bash
git add .
git commit -m "Implement {{feature or fix}} using agent workflow"
```

If the change was large, consider multiple commits that correspond to the steps in DevAgent’s plan.

---

## 9. Quick Cheat Sheet

**New feature or refactor**

1. You -> DevAgent  
2. DevAgent -> plan + first change  
3. You -> apply changes  
4. (Optional) You -> TestAgent for tests  
5. You -> run tests  
6. If failing -> BugAgent  
7. If passing -> CleanupAgent  
8. Commit

**Bugfix**

1. You -> BugAgent with failing output  
2. BugAgent -> diagnosis + minimal fix  
3. You -> apply fix and re-run tests  
4. (Optional) You -> TestAgent for regression tests  
5. (Optional) You -> CleanupAgent  
6. Commit