# Contribution 1: Adapter: Qwen Code

**Contribution Number:** 2

**Student:** Nikhil Koganti

**Issue:** https://github.com/orthogonalhq/nous-core/issues/296

**Status:** Phase II Complete

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

### Steps to Reproduce
1. Clone the repository fork and switch to the `dev` branch:
   ```bash
   git clone https://github.com/Nikhil-Koganti/nous-core-open-source.git
   cd nous-core-open-source
   git checkout dev
   ```
2. Install dependencies:
   ```bash
   pnpm install
   ```
3. Build the project and run existing tests to establish a baseline:
   ```bash
   pnpm build
   pnpm test
   ```

---

### Reproduction Evidence
* **Fork Branch URL**: [Nikhil-Koganti/nous-core-open-source (dev branch)](https://github.com/Nikhil-Koganti/nous-core-open-source/tree/dev)

---

### Implementation Plan
* **Create Adapter**: Implement the `AgentAdapter` interface in a new file [qwen-code-adapter.ts](file:///c:/Users/Nikhil/Documents/nous-open-source/self/subcortex/coding-agents/src/qwen-code-adapter.ts) matching the contract defined in [adapter.ts](file:///c:/Users/Nikhil/Documents/nous-open-source/self/shared/src/types/adapter.ts). The adapter will spawn the Qwen Code terminal CLI process, translate inputs/outputs, and validate them against the Zod schemas from `@nous/shared`.
* **Export Adapter**: Register and export the `QwenCodeAdapter` from [index.ts](file:///c:/Users/Nikhil/Documents/nous-open-source/self/subcortex/coding-agents/src/index.ts).
* **Add Conformance Tests**: Create unit/conformance tests in `self/subcortex/coding-agents/src/__tests__/qwen-code-adapter.test.ts` matching the lifecycle patterns of other adapters.
* **Verify**: Run `pnpm typecheck`, `pnpm lint`, and `pnpm test` locally to verify that all schemas and type checks pass.

### Environment Setup

I have forked and setup the full repo including using antigravity for ide and agentic reviewing of the code.

## Solution Approach

### Initial Analysis

My initial understanding is that this issue is mainly an adapter implementation task. The project already has similar adapters for Claude and Codex, so the Qwen Code adapter should likely follow the same structure while changing the command execution details for Qwen Code.

The root cause is not a bug in the existing adapters. Instead, the missing functionality is that Qwen Code has not yet been connected to the project’s shared `AgentAdapter` interface.

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
