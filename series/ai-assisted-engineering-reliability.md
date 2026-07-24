# AI-Assisted Engineering Reliability

This series follows one question from several angles: how can AI-assisted engineering remain reviewable and trustworthy when generated output is not the same thing as workflow completion?

## Narrative progression

1. **Prompt Is Not a Workflow Engine** establishes the boundary between probabilistic generation and deterministic workflow governance.
2. **Producing Work Is Not Completing Workflow** separates data-plane output from control-plane completion and approval.
3. **AI Can Produce Work, But What Counts as Evidence?** distinguishes an artifact from evidence and asks for a trace from intent through completion.
4. **The most overlooked problem in AI-assisted engineering** narrows the claim to evidence gaps and validation scope rather than code quality or productivity.
5. **Completion Needs Evidence, Not Confidence** shows why apparently complete work can still need recovery, metadata, or evidence repair.
6. **A Healthy Workflow Sometimes Says No** treats rejection, repair, renewed evidence, and admitted completion as an observable workflow sequence.
7. **AI-Assisted Engineering Needs More Than Long Context** asks how an AI system can understand the engineering knowledge a project accumulated across sessions, revisions, and evidence. — Ruei Ming Deng, July 23, 2026

The sequence is intentionally bounded. These articles do not claim that AIWF proves better code, higher productivity, or universal defect reduction. They describe a repository-backed argument for making completion claims more explicit and evidence-aware.

## Related work

Research references are collected in [`references/papers.md`](../references/papers.md). They provide adjacent or parallel context and should not be read as retroactive proof of the series' claims.
