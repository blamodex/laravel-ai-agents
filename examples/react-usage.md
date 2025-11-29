# Using Blamodex Dev Agents in a React + TypeScript Project

This guide shows how to use the Blamodex agent files  
(`DevAgent.md`, `BugAgent.md`, `TestAgent.md`, `CleanupAgent.md`)  
inside a modern React + TypeScript codebase, typically within a Laravel + Vite setup.

This document assumes your React project:

- Uses TypeScript
- Lives under `resources/js` or `src`
- Uses Vite for bundling
- Uses:
  - Jest + React Testing Library for unit tests
  - ESLint + Prettier for formatting
- Uses standard scripts:
  - `npm run test`
  - `npm run test:watch`
  - `npm run lint`
  - `npm run build`

---

## 1. Adding or Updating a Component with DevAgent

**Scenario:**  
You want to add a new `<UserActivityList />` component that displays paginated activities.

### Step 1 — Invoke DevAgent

Prompt:

```
Use DevAgent.
Follow /agents/DevAgent.md.

Goal: Create a new UserActivityList component under resources/js/components.
It should:
- Fetch activity data from /api/user/{id}/activity
- Support pagination
- Show a loading + error state
- Be fully typed with TypeScript
Use small, PR-sized steps.
```

Expected from DevAgent:

- A *plan* outlining:
  - Component scaffold
  - Types/interfaces
  - Fetch hook
  - Pagination logic
  - Tests
- The *first step* such as:
  - Create a `UserActivityList.tsx` skeleton with placeholder UI
- Full code blocks for each change
- Verification commands (run Jest, run lint)

---

## 2. Generating React Component Tests with TestAgent

After DevAgent creates the component logic, use TestAgent to produce tests.

Prompt:

```
Use TestAgent.
Follow /agents/TestAgent.md.

Context: DevAgent created UserActivityList.tsx.

Write React Testing Library + Jest tests that cover:
- initial loading state
- successful fetch
- pagination logic
- error state
```

Expected from TestAgent:

- Test plan  
- Full test file content, e.g.:

  `resources/js/components/UserActivityList.test.tsx`

- Use of RTL:
  - `render(...)`
  - `screen.getByText(...)`
  - `await waitFor(...)`
- Verification commands (`npm run test`)

---

## 3. Debugging Component or Hook Issues with BugAgent

If tests fail or runtime errors occur, use BugAgent.

Example error:

```
TypeError: Cannot read properties of undefined (reading 'map')
```

Prompt:

```
Use BugAgent.
Follow /agents/BugAgent.md.

Here is the failing test output:

<PASTE FAILURE HERE>

This happened inside UserActivityList.tsx while rendering activity items.
```

Expected from BugAgent:

- Hypotheses (uninitialized state, incorrect mock response, missing default)
- Request for relevant files (component, test, hook)
- Minimal fix (e.g., add loading guards, default array)
- Full code block of the corrected component
- Verification commands

---

## 4. Cleanup and Styling with CleanupAgent

After the component works and tests pass, tidy the code.

Prompt:

```
Use CleanupAgent.
Follow /agents/CleanupAgent.md.

Audit and clean up:
- resources/js/components/UserActivityList.tsx
- resources/js/components/UserActivityList.test.tsx

Only perform non-behavioral improvements.
```

Expected:

- Checklist (unused imports, missing type annotations, redundant state)
- Final updated files
- Suggested doc updates
- ESLint/Prettier commands (`npm run lint`)

---

## 5. End-to-End Prompt Flow Example

```
Use DevAgent. Goal: Add a typed UserActivityList component.
```

→ DevAgent gives plan + step 1

```
Looks good. Apply step 1. What is step 2?
```

→ DevAgent continues with incremental changes

```
Use TestAgent. Create RTL tests for this component.
```

→ TestAgent generates full test file

```
npm run test
```

→ A test fails

```
Use BugAgent. Here's the failing Jest output:
<PASTE>
```

→ BugAgent diagnoses and provides a minimal fix

```
Use CleanupAgent on the modified files.
```

→ CleanupAgent finishes with safe, non-behavioral cleanup

---

## 6. Best Practices for React Usage

- Keep DevAgent steps extremely small — new hook, new state, new effect, etc.
- Let TestAgent create tests *before* debugging, not after
- Use BugAgent only for behavioral fixes
- Use CleanupAgent for:
  - Type cleanup
  - Imports
  - JSDoc
  - Removing redundant logic
- Rely on TypeScript interfaces, not `any`
- Mock network calls using:
  - `jest.mock()`
  - or a local test helper

---

## 7. Summary

React development follows the same workflow as Laravel:

1. DevAgent — component implementation  
2. TestAgent — test coverage  
3. BugAgent — debugging  
4. CleanupAgent — polish  
5. Commit  

This produces stable, maintainable React code with minimal regressions.