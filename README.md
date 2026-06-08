# Contribution 1: Adapter: Qwen Code

**Contribution Number:** 1
**Student:** [Your Name]
**Issue:** https://github.com/orthogonalhq/nous-core/issues/296
**Status:** Phase I Complete

---

## Why I Chose This Issue

I chose this issue because it focuses on implementing a new `AgentAdapter` for Qwen Code in the `nous-core` project. The issue is asking for support for Qwen Code, a terminal-based coding agent, by creating a new adapter file that follows the same structure as the existing Claude and Codex adapters.

This issue matters because adapters allow the project to support different coding agents through a shared interface. I chose it because it gives me a chance to work with TypeScript, adapter patterns, command-line tool integration, schema validation, and tests while still having existing reference files to guide my implementation.

---

## Understanding the Issue

### Problem Description

The project currently has adapter implementations for other coding agents, but it does not yet have an `AgentAdapter` implementation for Qwen Code. The issue asks for a new file, `self/subcortex/coding-agents/src/qwen-code-adapter.ts`, that implements the required adapter contract for Qwen Code.

The adapter needs to support the required `AgentAdapter` methods, including `prepare`, `execute`, `captureTrace`, `captureSideEffects`, `collectArtifacts`, and `cleanup`. It also needs to validate inputs and outputs using the existing Zod schemas, be exported from the coding agents index file, and include tests.

### Expected Behavior

After the issue is completed, the project should have a working Qwen Code adapter that follows the existing `AgentAdapter` contract. The adapter should be available through the package exports and should pass tests that confirm it prepares, executes, captures results, collects artifacts, and cleans up correctly.

### Current Behavior

Currently, Qwen Code is listed as a desired adapter, but the project does not yet include a `qwen-code-adapter.ts` implementation. This means the system cannot use Qwen Code through the same adapter interface used by other coding agents.

### Affected Components

The main affected components appear to be:

* `self/subcortex/coding-agents/src/qwen-code-adapter.ts`
* `self/subcortex/coding-agents/src/index.ts`
* `self/subcortex/coding-agents/src/__tests__/`
* Existing reference adapters:

  * `self/subcortex/coding-agents/src/claude-adapter.ts`
  * `self/subcortex/coding-agents/src/codex-adapter.ts`
* Shared adapter contract:

  * `self/shared/src/types/adapter.ts`

The issue specifically says not to modify the `AgentAdapter` contract or the shared Zod schemas.

---

## Reproduction Process

### Environment Setup

I have not completed full local reproduction yet. During the next phase, I plan to fork the repository, clone my fork locally, install the project dependencies, and inspect the existing Claude and Codex adapter implementations.

### Steps to Reproduce

1. Open the `orthogonalhq/nous-core` repository.
2. Review the existing coding agent adapters in `self/subcortex/coding-agents/src/`.
3. Check whether a Qwen Code adapter exists.
4. Observe that there is no `qwen-code-adapter.ts` file implementing Qwen Code support.

### Reproduction Evidence

* **Commit showing reproduction:** Not available yet.
* **Screenshots/logs:** Not available yet.
* **My findings:** The issue asks for a new Qwen Code adapter implementation, tests, schema validation, and export updates.

---

## Solution Approach

### Analysis

My initial understanding is that this issue is mainly an adapter implementation task. The project already has similar adapters for Claude and Codex, so the Qwen Code adapter should likely follow the same structure while changing the command execution details for Qwen Code.

The root cause is not a bug in the existing adapters. Instead, the missing functionality is that Qwen Code has not yet been connected to the project’s shared `AgentAdapter` interface.

### Proposed Solution

I plan to implement a new `qwen-code-adapter.ts` file that follows the `AgentAdapter` contract. I will use the Claude and Codex adapters as references, validate data with the existing Zod schemas, export the new adapter from `index.ts`, and add tests in the existing test directory.

### Implementation Plan

Using the UMPIRE framework:

**Understand:**
The issue asks for a new Qwen Code adapter that implements the required `AgentAdapter` methods without changing the shared adapter contract or Zod schemas.

**Match:**
I will study the existing `claude-adapter.ts` and `codex-adapter.ts` files to understand the expected structure, method behavior, error handling, artifact collection, and test patterns.

**Plan:**

1. Fork and clone `orthogonalhq/nous-core`.
2. Install dependencies and run the existing test suite.
3. Review `AgentAdapter` in `self/shared/src/types/adapter.ts`.
4. Review `claude-adapter.ts` and `codex-adapter.ts`.
5. Create `self/subcortex/coding-agents/src/qwen-code-adapter.ts`.
6. Implement the required methods:

   * `prepare`
   * `execute`
   * `captureTrace`
   * `captureSideEffects`
   * `collectArtifacts`
   * `cleanup`
7. Validate input and output using the existing Zod schemas.
8. Export the adapter from `self/subcortex/coding-agents/src/index.ts`.
9. Add tests in `self/subcortex/coding-agents/src/__tests__/`.
10. Run tests and fix any failures.
11. Open a pull request explaining the implementation.

**Implement:**
Implementation has not started yet.

**Review:**
Before submitting a pull request, I will confirm that:

* The adapter implements the existing `AgentAdapter` contract.
* The shared adapter type and Zod schemas are not modified.
* The implementation follows the style of the Claude and Codex adapters.
* The new adapter is exported from `index.ts`.
* Tests are included for the expected behavior.
* No unrelated files are changed.

**Evaluate:**
I will verify the solution by running the relevant tests and manually checking that the new Qwen Code adapter behaves consistently with the existing coding agent adapters.

---

## Testing Strategy

### Unit Tests

* [ ] Test that the Qwen Code adapter can be created and exposes the required `AgentAdapter` methods.
* [ ] Test that input and output validation uses the existing Zod schemas.
* [ ] Test that the adapter handles execution results correctly.
* [ ] Test that trace capture works as expected.
* [ ] Test that side effects and artifacts are collected correctly.
* [ ] Test that cleanup behavior works correctly.

### Integration Tests

* [ ] Confirm the adapter is exported from `self/subcortex/coding-agents/src/index.ts`.
* [ ] Confirm the Qwen Code adapter follows the same usage pattern as the Claude and Codex adapters.

### Manual Testing

Manual testing has not been completed yet. I will update this section after I set up the repository locally and understand how the existing adapter tests are run.

---

## Implementation Notes

### Week 1 Progress

I completed Phase I setup by creating my GitHub and Slack accounts, selecting this GitHub issue, commenting on the issue, and marking the issue in the course spreadsheet. I also created this Contribution README to document my understanding of the issue and my plan for working on it.

### Code Changes

* **Files modified:** None yet.
* **Key commits:** None yet.
* **Approach decisions:** I am starting by studying the existing Claude and Codex adapters so that the Qwen Code adapter follows the project’s current design instead of introducing a new pattern.

---

## Pull Request

**PR Link:** Not submitted yet.

**PR Description:** To be added after implementation.

**Maintainer Feedback:**

* No maintainer feedback yet.

**Status:** Not started.

---

## Learnings & Reflections

### Technical Skills Gained

So far, I have practiced reading an open-source issue, identifying the expected files involved, and translating the issue requirements into an implementation plan. I also learned more about how adapter-based designs allow one project to support multiple tools through a shared interface.

### Challenges Overcome

The main challenge so far was understanding the difference between the Contribution README repository and the forked project repository. The Contribution README repository documents my process, while the forked `nous-core` repository is where I will eventually make code changes.

### What I'd Do Differently Next Time

Next time, I would review the issue, affected files, and acceptance criteria earlier so that I can create a more specific README from the beginning.

---

## Resources Used

* Project repository: https://github.com/orthogonalhq/nous-core
* Chosen GitHub issue: https://github.com/orthogonalhq/nous-core/issues/296
* Qwen Code project: https://github.com/QwenLM/qwen-code
