# Adapter: Qwen Code

**Contribution Number:** 1

**Student:** Nikhil Koganti

**Issue:** https://github.com/orthogonalhq/nous-core/issues/296

**Status:** Phase IV Complete

---

## Why I Chose This Issue

I chose this issue because it focuses on implementing a new `AgentAdapter` for Qwen Code in the `nous-core` project. The issue is asking for support for Qwen Code, a terminal-based coding agent, by creating a new adapter file that follows the same structure as the existing Claude and Codex adapters.

This issue matters because adapters allow the project to support different coding agents through a shared interface. I chose it because it gives me a chance to work with TypeScript, adapter patterns, command-line tool integration, schema validation, and tests while still having existing reference files to guide my implementation.

---
## Phase IV - Pull Request Submitted

**Update(July 1st):** The author reviewed the PR and requested changes, primarily around the provider's runtime safety posture (default auto-approved execution, headless one-shot invocation, cancellation/timeout handling, Windows shell execution, and environment inheritance). I have currently pushed my fixes to the same PR and also resolved all the merge conflicts.Currently waiting for author review. 

**PR Link:**: https://github.com/orthogonalhq/nous-core/pull/416

**Summary of Contribution:**
Added Qwen Code as a CLI-backed provider leaf under `self/subcortex/providers/src/providers/qwen-code/`, following the updated #296 contract. Built on `ProviderDefinitionLeaf` with the provider ID derived from `vendorKey` and `executionCapabilityProfile: one_shot_command`; regenerated the provider catalogs and added unit/integration tests (13 leaf tests + catalog assertions). Opened against the integration branch `feat/contributor-friendly-inference-provider-surface`. Closes #296.

**Feedback / Next Steps:**
No review comments yet. The only prior guidance was the maintainer's 2026-06-18 issue update redirecting CLI integrations from the old `AgentAdapter` path to a provider leaf, which this implementation follows. Next step is to respond to reviewer feedback and iterate as needed; verification is green locally (376 provider tests passed / 2 live-only skipped, lint clean, typecheck exit 0, catalogs no-diff).


**Status:** Changes requested and iterating on author feedback

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
* **Dev Branch URL**: [Nikhil-Koganti/nous-core-open-source (dev branch)](https://github.com/Nikhil-Koganti/nous-core-open-source/tree/dev)
* **Qwen Code Branch URL**: [Nikhil-Koganti/nous-core-open-source (qwen-code-provider branch)](https://github.com/Nikhil-Koganti/nous-core-open-source/tree/qwen-code-provider)

---

### Implementation Plan
* **Create Adapter**: Implement the `AgentAdapter` interface in a new file [qwen-code-adapter.ts](file:///c:/Users/Nikhil/Documents/nous-open-source/self/subcortex/coding-agents/src/qwen-code-adapter.ts) matching the contract defined in [adapter.ts](file:///c:/Users/Nikhil/Documents/nous-open-source/self/shared/src/types/adapter.ts). The adapter will spawn the Qwen Code terminal CLI process, translate inputs/outputs, and validate them against the Zod schemas from `@nous/shared`.
* **Export Adapter**: Register and export the `QwenCodeAdapter` from [index.ts](file:///c:/Users/Nikhil/Documents/nous-open-source/self/subcortex/coding-agents/src/index.ts).
* **Add Conformance Tests**: Create unit/conformance tests in `self/subcortex/coding-agents/src/__tests__/qwen-code-adapter.test.ts` matching the lifecycle patterns of other adapters.
* **Verify**: Run `pnpm typecheck`, `pnpm lint`, and `pnpm test` locally to verify that all schemas and type checks pass.

### Phase III Progress

I implemented the Qwen Code provider using the updated provider-adapter integration branch requirements. The original issue described an `AgentAdapter` implementation under `self/subcortex/coding-agents`, but the maintainer clarified that CLI-backed integrations should now be implemented as provider leaves under `self/subcortex/providers/src/providers/<vendor>/`.

I created a new `qwen-code` provider leaf following the existing `codex-cli` provider structure. The implementation includes provider definition metadata, adapter wiring, CLI execution handling, exports, generated provider registry updates, and unit/integration test coverage.

### Code Changes

* **Development branch:** https://github.com/Nikhil-Koganti/nous-core-open-source/tree/qwen-code-provider
* **Main commit:** https://github.com/Nikhil-Koganti/nous-core-open-source/commit/9259bcfe

Files added or updated include:

* `self/subcortex/providers/src/providers/qwen-code/adapter.ts`
* `self/subcortex/providers/src/providers/qwen-code/definition.ts`
* `self/subcortex/providers/src/providers/qwen-code/implementation.ts`
* `self/subcortex/providers/src/providers/qwen-code/index.ts`
* `self/subcortex/providers/src/providers/qwen-code/provider.ts`
* `self/subcortex/providers/src/__tests__/providers/qwen-code.test.ts`
* provider registry/codegen-related files for adapter and definition registration

Key implementation decisions:

* Implemented Qwen Code as a CLI-backed provider leaf, not as the old `AgentAdapter`.
* Used `ProviderDefinitionLeaf`.
* Used `vendorKey: 'qwen-code'`.
* Did not hand-author `wellKnownProviderId`, because provider IDs are now derived from `vendorKey`.
* Declared `executionCapabilityProfile` for the CLI provider.
* Added tests for provider behavior and registry integration.

* Project repository: https://github.com/orthogonalhq/nous-core
* Chosen GitHub issue: https://github.com/orthogonalhq/nous-core/issues/296
* Qwen Code project: https://github.com/QwenLM/qwen-code

* [ ] Run the full project build after dependencies are installed.
* [ ] Run the full provider package test suite.
* [ ] Open a draft PR against the maintainer’s requested integration branch.
