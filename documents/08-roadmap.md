# Agentic Legal Assistant — Delivery Roadmap (v3)

> **Provenance note.** This roadmap was produced on **2026-09-29** from the same
> static analysis as the rest of `documents/` (see `00-index.md`). Backlog items are
> derived from the functional requirements in `02-functional-requirements.md`, the
> non-functional targets in `03-non-functional-requirements.md`, and the market
> findings in `01-market-analysis.md`. Timeline targets are `[TO BE VALIDATED]`
> where they depend on future estimates rather than shipped code.

## 1. Objective & horizon

Legal AI agent for drafting/analysis with Gemini 2.5 Flash. This roadmap plans the next **5–6 weeks** of incremental delivery in
lockstep with the SDLC phases and traceability rules in `07-sdlc-lifecycle.md`
(Requirements → Design → Implement → Verify → Release/Operate → Improve).

Current shipped state: no public deployment yet (`—` in the repo facts); the
notebook agent is committed and the v2 documentation set is complete. The build is
currently a single Colab notebook (`Agentic_legal&ResearchAssistant.ipynb`) with a
system-driven persona, multi-tool function calling, and an iterative response loop.

## 2. Product backlog

Prioritised with MoSCoW. Items are phrased as outcomes (not tasks) and map to FR/NFR ids.

| ID | Item (outcome) | Source | Priority |
| --- | --- | --- | --- |
| PBI-01 | The notebook runs end-to-end from a clean Colab environment with inputs and expected outputs documented | FR-4 | Must |
| PBI-02 | Every generative call either returns output conforming to a declared schema or fails with an explicit error — never partial or malformed output | FR-5 | Must |
| PBI-03 | Every generative call records model, prompt version, and token usage, so a run is auditable after the fact | FR-5 (inferred) | Must |
| PBI-04 | Missing or invalid configuration is rejected at startup with a clear, actionable error rather than a mid-run traceback | FR-8 | Must |
| PBI-05 | Provider outages and timeouts surface as a typed error with an explicit retry budget, never a raw stack trace | NFR-4.1 | Should |
| PBI-06 | Per-request cost ceiling and maximum output token budget are set and observable | NFR-4.2, NFR-4.3 | Should |
| PBI-07 | Prompt-injection exposure of the tool-calling loop is assessed and documented | NFR-4.4 | Should |
| PBI-08 | Automated tests cover the agent loop's termination and tool-dispatch logic | NFR-6.1 | Should |
| PBI-09 | CI runs lint plus the notebook smoke path on every push | NFR-6.3, NFR-6.2 | Should |
| PBI-10 | Groundings in tool output are cited, addressing the "tool results are unverified" limitation the README already states | market differentiator (§6) | Could |
| PBI-11 | Latency and cold-start figures for the agent loop are measured and recorded against NFR-1 targets | NFR-1.1, NFR-1.5 | Won't (this horizon) |

## 3. Sprint plan

**Sprint cadence:** 1 week = 1 sprint; stand-up daily (15 min), sprint review + retrospective at the end of each sprint.

| Sprint | Goal | PBI delivered | Done/exit criteria | Phase (SDLC) |
| --- | --- | --- | --- | --- |
| Sprint 1 | Make the notebook reproducible from a clean environment | PBI-01, PBI-04 | fresh Colab run completes all cells; setup steps match the README | Implement → Verify |
| Sprint 2 | Contract and audit the model's output | PBI-02, PBI-03 | declared output schema enforced; a sample run's token usage is retrievable from the log | Implement → Verify |
| Sprint 3 | Fail safely instead of crashing | PBI-05, PBI-06 | simulated provider failure returns a typed error; cost budget trips before the ceiling is exceeded | Verify |
| Sprint 4 | Test and pipeline the agent loop | PBI-08, PBI-09 | CI green on every push; loop-termination test fails if the exit condition is removed | Verify → Release |
| Sprint 5 | Harden the tool boundary | PBI-07 | injection scenarios exercised against the tool dispatcher; findings written into NFR-4.4 | Verify |
| Sprint 6 | Ship the citation layer and cut a release | PBI-10, backlog refinement | build/lint green, use-case walkthrough in `05-use-cases.md` updated, release cut | Release & Operate → Improve |

## 4. Ceremonies

- **Daily stand-up (15 min):** what shipped since yesterday, what's blocked, what's next — tied to the active sprint's PBI board.
- **Sprint review (30 min, end of sprint):** demo PBI outcomes against the sprint goal; update `05-use-cases.md` walkthrough where behavior changed.
- **Retrospective (30 min, end of sprint):** inspect + adapt; record one actionable improvement per sprint in git notes.
- **Backlog refinement (before sprint 1):** re-prioritise PBIs against latest market findings.

## 5. Burndown (planned)

Tracked as PBI points remaining per sprint. Planned trajectory below; the team records actuals at each sprint review. `[TO BE MEASURED]` until the first sprint completes.

| Sprint | Planned remaining points |
| --- | --- |
| Start | 13 |
| Sprint 1 | 11 |
| Sprint 2 | 9 |
| Sprint 3 | 6 |
| Sprint 4 | 4 |
| Sprint 5 | 2 |
| Sprint 6 (Done, 0) | 0 |

## 6. Rollout & deploy

- Build/deploy per `07-sdlc-lifecycle.md` §5 (release policy).
- Production: none yet (`—`). There is no Vercel configuration or container
  definition in this repo, so the release cut in Sprint 6 is a tagged, CI-verified
  notebook revision rather than a hosted deploy. Hosting model is deferred until
  PBI-10 is validated against the market.
- Health: a broken build blocks the next sprint's first commit; security findings are release blockers.

## 7. Risks

| Risk | Mitigation |
| --- | --- |
| Requirements drift vs. implemented code | PBI↔FR↔use-case traceability check per change (`07-sdlc-lifecycle.md` §3) |
| Unmeasured NFRs treated as done | `[TO BE MEASURED]` targets stay visible until instrumented |
| Burndown actuals fall off plan | Over-plan cut scope in the retrospective, not mid-sprint |
| Category commoditised by well-funded incumbents (Harvey, Lexis+ AI, CoCounsel) | Compete on the agent-loop and grounding story, not on price (`01-market-analysis.md` §7) |
| Claims outrun the code — a demo presented as a product | Keep README and `documents/` language aligned with the source tree; "not legal advice" caveat stays |
