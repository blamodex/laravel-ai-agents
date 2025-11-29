# Using Blamodex Dev Agents in a Laravel Project

This guide provides concrete examples for how to use the agent files  
(`DevAgent.md`, `BugAgent.md`, `TestAgent.md`, `CleanupAgent.md`) inside a
Laravel 12 application using Claude Code in VS Code.

The examples below assume your project:

- Uses Laravel 12  
- Organizes tests under `tests/Feature` and `tests/Unit`  
- Uses factories/seeders instead of inline model creation for testing, as much as possible 
- Follows PSR-12  
- Uses Composer scripts:
  - `composer test`
  - `composer test:coverage`
  - `composer lint`
  - `composer analyze`

---

## 1. Adding a New Feature with DevAgent

**Scenario:**  
You want to add a new endpoint:  
`GET /api/user/{user}/activity`  
that returns paginated user activity records.

### Step 1 — Invoke DevAgent

Prompt:

```
Use DevAgent.
Follow /agents/DevAgent.md.

Goal: Add a new endpoint GET /api/user/{user}/activity that returns a paginated
list of Activity models for the specified user. Use small, PR-sized steps.
```

Expected from DevAgent:

- A plan outlining:
  - Add route
  - Add controller method
  - Add pagination logic
  - Add ActivityResource / transformer
  - Add tests
- The first step, for example:
  - Create route declaration  
  - Add empty controller method  
- Full code blocks for each change
- Verification commands (`composer test`, `composer lint`)

---

## 2. Generating Tests with TestAgent

Once the feature step is complete, request tests.

Prompt:

```
Use TestAgent.
Follow /agents/TestAgent.md.

Context: DevAgent created a paginated user activity endpoint at
GET /api/user/{user}/activity.

Write the Feature tests for:
- basic pagination
- unauthorized access
- user cannot access another user's activity
- correct JSON structure
```

Expected from TestAgent:

- Test plan  
- Full test file(s), e.g. `tests/Feature/UserActivityTest.php`  
- Use of factories:
  - `User::factory()`
  - `Activity::factory()->count(10)`  
- Verification commands

---

## 3. Debugging with BugAgent

If a test fails, copy the output into BugAgent.

Example failing output:

```
Expected JSON key "data" missing.
Received:
{
    "message": "No activity found."
}
```

Prompt:

```
Use BugAgent.
Follow /agents/BugAgent.md.

Here is the failing PHPUnit output:

<PASTE FAILURE HERE>

This occurred after creating the new user activity endpoint.
```

Expected from BugAgent:

- Hypotheses (e.g., incorrect empty collection handling)
- Request for relevant files (controller, resource, test)
- Minimal fix (e.g., return empty paginator instead of an error message)
- Full file edits
- Verification commands

---

## 4. Cleanup with CleanupAgent

After DevAgent + TestAgent + BugAgent converge and all tests pass:

Prompt:

```
Use CleanupAgent.
Follow /agents/CleanupAgent.md.

Audit and clean up:
- app/Http/Controllers/UserActivityController.php
- app/Http/Resources/ActivityResource.php
- tests/Feature/UserActivityTest.php

Only perform non-behavioral cleanup.
```

Expected:

- Checklist (unused imports, docblocks, naming consistency, etc.)
- Full updated file contents
- Suggestions for documentation updates
- Lint/analysis commands

---

## 5. End-to-End Example Prompt Flow

```
Use DevAgent. Goal: Add a paginated user activity endpoint.
```

→ DevAgent gives plan + step 1

```
Looks good. Apply step 1. What is step 2?
```

→ DevAgent continues

```
Generate tests using TestAgent.
```

→ TestAgent produces test files

```
Run tests.
composer test
```

→ Some tests fail

```
Use BugAgent. Here's the failing output:
<PASTE>
```

→ BugAgent diagnoses and proposes a fix

```
Use CleanupAgent to finalize cleanup for the touched files.
```

→ CleanupAgent refines formatting, docblocks, imports

```
Everything looks good. Ready for commit.
```

---

## 6. Best Practices

- Keep DevAgent steps small and PR-sized
- Always generate tests before debugging
- Never skip the verification commands
- Limit BugAgent to behavioral fixes only
- Let CleanupAgent handle non-behavioral refinements
- Consider committing each small step if working incrementally

---

## 7. Summary

Using these agents together creates a structured,
repeatable development workflow for Laravel:

1. DevAgent — implementation  
2. TestAgent — test coverage  
3. BugAgent — debugging  
4. CleanupAgent — polish  
5. Commit  