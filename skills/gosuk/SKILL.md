---
name: gosuk
description: Verification-first implementation skill for Codex. Use when the user invokes `/gosuk`, mentions `gosuk`, or wants a fix or feature implemented only after a deterministic verification path is defined and run before any code changes.
---

# Gosuk

Use this skill for implementation work when verification must be proven first.

## Core Rule

Do not start editing code until the verification path exists, is deterministic, and can be run immediately.

- For a bug: reproduce the current failure first.
- For a feature: create or adapt a focused verifier that fails because the behavior is missing.
- If the verifier is flaky, manual-only, or too broad, tighten the verifier before implementation.

## Deterministic Verification Standard

A verifier is acceptable only when all of these are true:

- It has a concrete local command.
- Its pass/fail condition is explicit.
- It avoids manual clicking, race-prone timing, and uncontrolled live data.
- It is scoped tightly enough to explain why it failed.

Prefer verification in this order:

1. Existing focused automated test or script
2. New focused test or harness that reproduces the gap
3. Playwright, RTL, or integration coverage with controlled fixtures
4. Stable local command with explicit assertions or log checks

Do not rely on a manual smoke test as the main proof.

## Visible Proof Requirement

If the task changes something user-visible or artifact-producing, do not stop at a passing test unless artifact capture is genuinely blocked.

- UI changes: try to produce a screenshot, rendered page, component capture, or browser proof.
- File or artifact generation: try to produce the actual output file, preview, export, or rendered result.
- API or data-shape changes with a concrete payload: try to show the response, diff, or generated artifact that demonstrates the new behavior.
- Treat the artifact as supporting proof in addition to the deterministic verifier, not a replacement for it.
- If artifact capture is blocked by the environment, say exactly what blocked it and do not over-claim end-to-end proof.

## Workflow

1. Inspect the task and the likely touched surface.
   Pick the smallest deterministic verifier before planning the code change. If the task has a visible result, also decide what artifact or UI proof you expect to produce at the end.
2. Make the verifier real before implementation.
   - Bugs: prove the current failure first.
   - Features: write or adapt a focused failing test or harness first.
   - If the verifier does not run cleanly yet, fix the verifier path before touching product code.
3. Run the verifier before editing.
   Record the exact command and the result. Do not start implementation until this step is complete.
4. Implement the change normally.
   Keep scope tight and follow repo rules for the active codebase.
5. Re-run the exact same verifier until it passes.
   Keep the before and after proof on the same command whenever possible.
6. Produce visible proof when the change has a visible or generated result.
   Capture the smallest concrete artifact that demonstrates the implemented behavior: screenshot, HTML preview, downloaded file, rendered component, API payload, or similar.
7. Run the minimal guardrails implied by the change.
   - Backend or service logic: focused test, then targeted typecheck or lint if needed
   - Frontend route or component: focused test or Playwright, then relevant typecheck or lint
   - Shared package changes: regenerate or sync any derived artifacts before finishing
   - Contract or schema changes: regenerate and validate the affected generated artifacts
8. Close out with proof.
   Report the pre-change verifier, the implementation result artifact when applicable, the post-change verifier, extra guardrails, and any remaining determinism caveat.
9. If deterministic verification cannot be established, stop and say so explicitly.
   Do not implement against a manual-only or flaky verification story unless the user overrides it.

## Verification Selection Hints

- API or backend behavior: prefer service or endpoint integration coverage over broad suites.
- Route or UI behavior: prefer the smallest route or component test; use Playwright for real browser flows.
- Cross-service bugs: prove the issue at the boundary where it appears, then add narrower coverage in the touched service if needed.
- Nondeterministic bugs: add fixtures, freeze inputs, stub time or network, or reduce the repro until it is stable.
- Production-only reports: build a local or CI-reproducible harness before changing logic whenever possible.

## Output Contract

Keep status updates and final summaries grounded in verification:

- `Preflight verifier`
- `Implementation`
- `Implementation result`
- `Post-change verifier`
- `Extra guardrails`

For `Implementation result`:

- Include the concrete artifact path, screenshot, payload, preview, or rendered output when the task has a visible result.
- If you could not capture the artifact, say why not in one sentence.
