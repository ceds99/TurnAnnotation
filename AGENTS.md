# Project guidance

Read README.md, docs/decisions.md and the relevant design document before implementation.

- User-approved scope is authoritative. Annotation history is for human review, not persistent model memory.
- Assembled feedback is ordinary user-message text. Do not promote it to system/developer instructions, tool results, or hidden additionalContext.
- Preserve the user's comment and freeform text. Do not use an LLM to rewrite feedback in v1.
- Do not add model calls, telemetry, automatic resends, resolution workflows, or UI scraping to the product without a scoped design change.
- Native integration is the target. A side panel alone does not prove native text-selection integration.
- Keep compiler, local history, host adapter, and evaluation code separable.
- Mark proposals, documented host capabilities, locally verified capabilities, and experimental results distinctly.
- No quality or token-saving claims without reproducible measurements and uncertainty estimates.
- Do not publish local conversations or raw run logs. local/ and evals/runs/ are ignored intentionally.
- Implement only the requested scope. Current repository contains specifications and examples, not a working plugin or evaluation runner.
