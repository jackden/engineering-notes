# A Trace Is Not a Diagnosis

*When a diagnosis becomes an input to the agent’s next run*

In agents that use reflection and memory, an explanation of a failure may do more than appear in a postmortem. It can become part of the context that shapes the next plan and the next execution. *Reflexion*, for example, converts task feedback into textual reflections and stores them in episodic memory for later attempts, without updating model weights. The authors report improvements on the tasks they evaluated. [R1]

That makes “a trace is not a diagnosis” more than a debugging reminder. If an insufficiently supported explanation is accepted as guidance simply because a task eventually passed, a decision about the current result has quietly become a decision about future behavior.

**A successful run can support a claim about what a particular implementation did under particular conditions. It does not, by itself, validate the accompanying explanation or establish where that explanation can be reused.**

Three decisions need to remain distinct: whether to accept the present result, whether to accept an explanation of that result, and whether to authorize that explanation to guide future action. They may draw on the same evidence, but they should not inherit one undifferentiated PASS.

In [*AI-Assisted Engineering Needs More Than Long Context*](https://jackden.github.io/engineering-notes/articles/ai-assisted-engineering-needs-more-than-long-context/), I discussed how engineering knowledge can be preserved, retrieved, and applied. [A1] This article examines an earlier boundary: before an execution record becomes reusable engineering knowledge, what has actually been established, and what remains a candidate explanation?

Here, *trace* means preserved task events, test artifacts, reviews, and corrections—not necessarily a full agent conversation. The private engineering cases below include failures, subsequent passes, and a formal correction. They show that some technical explanations need to be qualified or withdrawn. They do **not** show that a mistaken explanation entered an agent's memory and caused a later failure. [E4] The argument is therefore about how to accept and reuse experience, not a report of a completed memory-system effectiveness experiment.

## A Later Pass Is Not a General Repair Rule

In one RAID automation task, a test failed while preparing for a migration operation. Device output showed that a Consistency Check (CC) was still in progress. A subsequent change added a bounded, read-only wait, with target identity verified on every fresh query. Even after CC completed, the original migration prerequisites still had to be satisfied. The preserved later JUnit results recorded **four passing tests**. [E1]

That is a useful engineering outcome, but it does not isolate the contribution of the wait. The runs did not restore identical initial hardware states; the code and test inputs were not identical throughout; and there was no matched control run that simply repeated the test without the code change. Four passes cannot, on their own, establish waiting as a general remedy for similar failures. [E1]

Imagine summarizing the case as “when this error appears, wait and retry.” This is a hypothetical example of lossy summarization, not an actual statement produced by the historical agent. It discards the target identity, observed device state, wait budget, and obligation to recheck the original prerequisites.

**What is more useful to retain is the decision procedure that determines whether the action is appropriate.** In a future case, first verify that the current target and state fit the documented path. Only then consider a bounded wait, followed by another check of the original prerequisites. An identity mismatch, unknown state, or timeout does not become safe to ignore because a previous task succeeded.

A related attribution problem appears in research on multi-agent debugging. The 2026 preprint *Repair or Resample?* observes that a full rerun resamples upstream model decisions. A later success may therefore follow a different execution path rather than demonstrate the effect of a specific intervention. The authors use reconstruction of the observable pre-intervention execution, followed by renewed downstream execution, to control that ambiguity. [R2]

This does not imply that model resampling explains the RAID result. The common question across these distinct settings is narrower: **after the outcome improves, what changed, and which changes have actually been separated from one another?**

For an agent that distills experience automatically, the design implication is to preserve supported checks and applicability conditions when the evidence is not strong enough to support a general repair rule. The checks themselves also have a scope; a more cautious procedure should not be treated as universally valid merely because it is cautious.

## Compression Must Not Expand the Evidence

The same RAID task had a post-patch regression result of **439 PASS / 7 FAIL**. The complete original baseline output was not preserved. A later comparison substituted the pre-change Lifecycle module in memory while reusing the current dependencies and the same test nodes; the same seven tests failed. A pre-publication check confirmed the matching failure-node identities across the saved logs and identical abstract syntax trees for three relevant functions in the compared versions. [E2]

This supports a limited conclusion: **under that module-substitution comparison and those test conditions, the seven failures cannot simply be attributed to the Lifecycle patch.** It does not recreate the complete historical environment. Of the five items in the original tested-source inventory, only two exact contents have been recovered. The seven failures were not thereby fixed, accepted, or waived. [E2]

The comparison is a positive example of what preserved evidence can do. Even incomplete evidence can constrain an overconfident attribution. The remaining challenge is whether a later summary preserves that constraint.

A reusable note grounded in this record might say:

> With the current dependencies and the same test nodes, substituting the earlier Lifecycle module produced the same failures. The available comparison does not support attributing them directly to this module change. Whether the failures are acceptable still requires a separate decision.

That is proposed guidance, not a quotation from the historical record. It cannot be replaced with “these are old failures and can be ignored.” The replacement would both remove the comparison conditions and introduce an acceptance decision the evidence did not establish.

For an agent's memory, the tested component, dependency state, and comparison scope are not optional footnotes. They determine when an inference may apply. If the test nodes, dependencies, or relevant code change, the agent must reassess that applicability rather than inherit a baseline label because the error messages look familiar.

This suggests a straightforward constraint: **summarization, paraphrasing, repeated citation, or a final PASS must not, by themselves, increase the certainty of a claim or broaden its scope.**

New code comparisons, sound reasoning, or further validation may justify a stronger claim. But the additional reasoning and evidence must remain inspectable; more confident wording is not a substitute for them. A summary may be shorter, while the conditions that determine its use need to remain attached to the guidance. Detailed artifacts can be retrieved through references when necessary.

## When an Explanation Is Corrected, Its Guidance Needs Review

A separate, already-closed structural audit reported **seven YAML parsing failures among eleven inputs**. The original explanation grouped unquoted Chinese characters, `§`, and em dashes among the causes. A formal correction withdrew that broad attribution and instead identified specific issues such as a colon followed by a space, indentation, and unclosed code fences. The reported failure count and issue categories did not change. [E3]

Small, newly constructed examples also contradicted the suggestion that those characters alone cause YAML parsing errors. But the original eleven inputs were not all preserved, so those examples are not a case-by-case reproduction of the historical failures. Unclosed code fences may also affect content extraction, rather than being YAML syntax errors in their own right. [E3]

The supported finding is precise: the failure observation can remain on record even while its original explanation is withdrawn. Closing the task does not make the interpretation immune to correction.

This creates a separate maintenance question for systems that reuse experience. If an older explanation had already become project guidance, would a correction to the source record affect what an agent sees next? We did not observe that propagation in this case. The following is a proposed responsibility, not a measured failure or a validated fix.

**Preserving historical explanations and continuing to offer them as current guidance are different decisions.** The original account may remain useful for understanding what people believed at the time. A withdrawn attribution should not silently remain an operative rule.

A lightweight starting point is to keep a relationship between guidance and its source records. When a source explanation is materially corrected, identify potentially affected guidance for review. Existing review practices can then decide whether to retain, qualify, revise, or withdraw it. A correction does not invalidate every note from the same task; equally, an unchanged file does not guarantee that its explanation remains sound.

This need not require a knowledge graph or automatic understanding of every claim dependency. It requires an executable maintenance responsibility: **when a source interpretation changes, there must be a way to revisit guidance that depends on it.**

## Accepting the Work Is Not the Same as Accepting Its Explanation

None of this means a task must have a complete root-cause analysis before a useful technique can be retained. A procedure that has worked under known conditions may remain a provisional, revocable recommendation even when the full causal explanation is unresolved.

*DoVer* offers a useful methodological comparison. It tests failure hypotheses through targeted interventions and observes that distinct interventions can independently repair the same failed task. Identifying an effective remedy and proving a unique cause are different evaluation objectives. [R3] That research does not provide additional validation of the RAID case; it helps clarify the distinction.

The appropriate review burden should depend on what the experience will be used for and what could go wrong. A note intended to suggest a diagnostic check may preserve uncertainty. Guidance that becomes a default operation, changes a safety constraint, or supports high-risk execution needs explicit applicability conditions, stronger verification, and current authorization.

**A task can be completed within a defined scope while its causal explanation remains incomplete. A useful technique can be retained without being promoted immediately into a general rule.**

Promoting a historical record into standing guidance should be an **inspectable change**, not an automatic by-product of task completion. A small collection of reviewed guidance can be enough to start. Each entry can identify its evidence source, applicable conditions, unresolved interpretations, claims it does not support, and triggers for review. This does not call for approval of every temporary note or bulk generation of “lessons learned” for every task.

For AIWF, such guidance could be an optional evidence-handoff practice, not a new automatic rule-learning system. The workflow layer can preserve relationships among tasks, validation, reviews, and corrections. Engineers and domain tools remain responsible for evaluating interpretations, checking present conditions, and authorizing operations. Reusing guidance must not inherit the authority of a previous task or silently change closure requirements. [A1]

## Test Whether the Agent Uses Experience Appropriately

The value of these proposals should be tested through subsequent agent behavior, not just whether an agent retrieves a note or repeats it accurately. One starting point is a set of paired scenarios: one preserves the conditions under which a technique was applicable, and the other changes a decisive condition.

For the CC example, a test could check whether the agent proposes a bounded wait when target identity and device state are verified, but stops applying that suggestion when identity does not match or state is unknown. Early evaluation can use offline records or simulated tools rather than live hardware.

Evaluation should separate *distillation* from *use*. Did a summary lose conditions, inflate a claim, or omit a correction? Given an already reviewed note, can the agent determine whether it applies now? Supplying the note directly can test use, but not whether retrieval would have surfaced the right source; retrieval needs its own checks. Success criteria should be based on the scenario and source evidence, not on whether the agent reproduces the wording of the note.

Comparisons should, as far as practical, hold the model, tool permissions, and evidence access constant, while recording context length, execution cost, and maintenance effort. Scoring should cover valid actions, mistaken carryover, necessary rechecks, and excessive refusal. A system that delegates every case to a human has not necessarily become better at using experience. Variations repeatedly used during development should not later count as independent evidence of generalization.

These are proposed tests, not completed experiments. A small, evidence-grounded evaluation may be sufficient to decide whether structured guidance reduces actual misjudgments. If it does not, a manually maintained guidance layer may be the right stopping point.

## From Inspectable History to Conditional Engineering Guidance

The private records support three modest findings: technical explanations can need narrowing or withdrawal; limited comparisons can constrain attribution; and a later PASS does not certify every conclusion attached to a task. Published agent research establishes a technical path by which reflection and feedback can enter a later attempt. [R1] Taken together, they motivate a design requirement: **preserve execution history, but also examine what is distilled from that history and under what conditions it is allowed to influence future work.**

An execution trace, a diagnosis, and reusable guidance are not interchangeable artifacts. A trace supplies observations. A diagnosis needs support. Guidance must also describe its scope and the conditions under which it should be revisited. None of those boundaries should disappear merely because the preceding task ended successfully.

**Task acceptance asks whether this work met its requirements. Experience reuse asks why the same approach would still be justified next time.**

Checking what an agent accomplished is necessary. Checking what it is prepared to accept from that accomplishment as future knowledge deserves equal attention. Reusable engineering experience is not just an answer that once worked; it includes the reasons to apply it, to recheck it, and to stop relying on it.

---

### Evidence and Scope

This is an engineering design argument grounded in private AI-assisted engineering records and public research, not an evaluation of an agent-memory system. The cases were purposefully selected, and the two RAID examples come from the same task rather than independent samples. A November 12, 2026 pre-publication review examined preserved artifacts, logs, and source code, along with limited mechanism checks. It did not rerun the historical application, pytest suite, or physical-device tests. [E4]

The records do not establish that mistaken explanations propagated through an agent's memory or caused subsequent failures. This article makes no claims about a model's internal beliefs, relative model performance, or AIWF-driven defect reductions. The lossy summaries, suggested guidance, and paired evaluation tasks are design examples, not quotations from the historical agent. Guidance review, correction linkage, and behavioral evaluation remain proposals, not released capabilities or demonstrated benefits.

### Sources and Evidence Notes

**[R1]** Noah Shinn, Federico Cassano, Ashwin Gopinath, Karthik Narasimhan, Shunyu Yao. [*Reflexion: Language Agents with Verbal Reinforcement Learning*](https://proceedings.neurips.cc/paper_files/paper/2023/hash/1b44b878bb782e6954cd888628510e90-Abstract-Conference.html). NeurIPS 2023. DOI: 10.52202/075280-0377. Cited for verbal reflection, episodic memory, and subsequent attempts, not for transferable benchmark gains.

**[R2]** Zhongwen Luan, Xiaoyu Zhang, Ming Hu, Yue Yang, Jiongchi Yu, Xiaohong Chen. [*Repair or Resample? Rethinking Failure Debugging in LLM Multi-Agent Systems*](https://arxiv.org/abs/2608.25920). arXiv:2608.25920 (2026 preprint). Cited for the distinction between upstream resampling and controlled pre-intervention replay with downstream continuation; not for hardware-state equivalence.

**[R3]** Ming-Jie Ma et al. [*DoVer: Intervention-Driven Auto Debugging for LLM Multi-Agent Systems*](https://www.microsoft.com/en-us/research/publication/dover-intervention-driven-auto-debugging-for-llm-multi-agent-systems/). ICLR 2026 (per Microsoft Research); arXiv:2512.06749. Cited for intervention-based hypothesis testing and multiple effective repair points, not for gains in the engineering cases here.

**[A1]** Ruei Ming Deng. [*AI-Assisted Engineering Needs More Than Long Context*](https://jackden.github.io/engineering-notes/articles/ai-assisted-engineering-needs-more-than-long-context/). Engineering Notes, July 23, 2026. Prior design argument on preserving, retrieving, applying, and governing engineering understanding; not new experimental evidence in this article.

**[E1]** Private case C05: CC state in a RAID automation task; bounded read-only wait and repeated target-identity checks; archived JUnit and case-final records. The four subsequent passes are not four independent repetitions and do not establish completion of the entire task or isolate the wait's effect.

**[E2]** Private case C04: regression failures, old-module comparison, failure-node identity checks across three saved logs, and AST comparison of three relevant functions. The full original baseline output is missing; the comparison reused current dependencies; exact content was recovered for only two of five originally inventoried source items.

**[E3]** Private case C03: correction to a closed structural audit and newly constructed minimal YAML examples. Not all eleven original inputs remain available. The new examples test specific mechanisms, not every historical root cause.

**[E4]** *A Trace Is Not a Diagnosis: Expanded Literature and Original Engineering Case Review*, private research package dated November 12, 2026, with source excerpts and verification notes. Original archives are not public. Internal paths, device identities, and product-sensitive details are deliberately omitted; this is traceable private case analysis, not an independently reproducible public benchmark.
