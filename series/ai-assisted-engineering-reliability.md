# AI-Assisted Engineering Reliability

This series follows one question from several angles: how can AI-assisted engineering remain reviewable and trustworthy when generated output is not the same thing as workflow completion?

## Narrative progression

1. **A01 — Prompt Is Not a Workflow Engine** (2026-05-13) establishes the boundary between probabilistic generation and deterministic workflow governance.
2. **A02 — Producing Work Is Not Completing Workflow** (2026-05-23) separates data-plane output from control-plane completion and approval.
3. **A03 — AI Can Produce Work, But What Counts as Evidence?** (2026-05-30) distinguishes an artifact from evidence and asks for a trace from intent through completion.
4. **A04 — The Most Overlooked Problem in AI-Assisted Engineering** (2026-06-06) narrows the claim to evidence gaps and validation scope rather than code quality or productivity.
5. **A05 — Completion Needs Evidence, Not Confidence** (2026-06-14) shows why apparently complete work can still need recovery, metadata, or evidence repair.
6. **A06 — A Healthy Workflow Sometimes Says No** (2026-06-27) treats rejection, repair, renewed evidence, and admitted completion as an observable workflow sequence.
7. **A07 — AI-Assisted Engineering Needs More Than Long Context** (2026-07-24) asks how an AI system can understand the engineering knowledge a project accumulated across sessions, revisions, and evidence. — Ruei Ming Deng

The sequence is intentionally bounded. These articles do not claim that AIWF proves better code, higher productivity, or universal defect reduction. They describe a repository-backed argument for making completion claims more explicit and evidence-aware.

The machine-readable series index and publication timeline are maintained in [`docs/article_index.yaml`](../docs/article_index.yaml) and [`docs/publishing_log.md`](../docs/publishing_log.md).

## Related work

Research references are collected in [`references/papers.md`](../references/papers.md). They provide adjacent or parallel context and should not be read as retroactive proof of the series' claims.
