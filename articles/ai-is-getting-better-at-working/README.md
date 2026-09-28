# AI Is Getting Better at Working. The Engineering Problem Is Changing Too.

*Some observations on compositional AI systems, execution provenance, and the next set of problems in AI-assisted engineering.*

*Published: September 28, 2026*

Over the past few months, I have been following developments in coding agents, model releases, AI engineering systems, and verification research. Many of these developments appear unrelated when viewed individually. Some teams are building more capable coding agents. Others are developing smaller, more specialized models. Some are improving computer use, while others are exploring collaboration between large and small models or investigating how AI-generated work can be verified.

Taken together, however, these developments seem to point toward a broader shift. **AI is not only getting better at generating outputs. AI systems are getting better at doing work. At the same time, “AI” increasingly looks less like a single model and more like a system composed of models, tools, agents, execution environments, and verification mechanisms.** If this direction continues, the engineering problems around AI-assisted work may change with it.

## From Generating Results to Performing Work

For a long time, evaluating an AI model largely meant evaluating the result it produced. Could it answer the question? Could it write the code? Could it understand a repository? Could it fix a bug?

Modern agent systems are beginning to operate across much longer work sequences. An agent may inspect a repository, develop a plan, invoke tools, modify code, run tests, diagnose failures, iterate on its changes, and finally summarize what it did.

OpenAI's Agents API reflects this shift. Context management, tool use, subagent coordination, long-running execution, file handling, code execution, and intermediate state are increasingly responsibilities of the agent harness surrounding the model. Whether an agent can complete useful work therefore depends on more than model intelligence alone. The model remains important, but it is becoming one component inside a larger execution system.

Microsoft Research's MagenticLite makes this system-oriented direction even more explicit. Rather than presenting a single stronger model, it combines different capabilities: MagenticBrain focuses on planning, delegation, coding, and orchestration; Fara1.5 specializes in computer use; and the surrounding harness is designed to make those components work together.

This makes a previously simple question more complicated: **Are we evaluating a model, or are we evaluating an AI system?** Increasingly, those are not the same thing.

## AI Capability Is Becoming More Compositional

For the past several years, one of the dominant narratives in AI has been straightforward: build increasingly capable general-purpose models and let them handle an expanding range of tasks. That direction is still very much alive, but another pattern is becoming increasingly visible: **specialization is reappearing inside larger AI systems.**

PP-OCRv5 is a useful example. The CVPR 2026 paper describes a specialized OCR model with roughly five million parameters that can compete with many billion-parameter vision-language models on standard OCR benchmarks, while providing more precise text localization and reducing hallucination. The interesting conclusion is not that small OCR models will replace general-purpose vision-language models. It is that not every task inside a system necessarily requires the largest or most general model available.

Similar ideas are appearing elsewhere. Vertical Routing studies how large and small language models can collaborate across different stages of the same task, assigning critical subtasks to the larger model while routing other stages to a smaller one. In the evaluated settings, this decomposition reduced large-model usage while maintaining or improving aggregate performance. Microsoft's MagenticBrain and Fara1.5 illustrate a different form of specialization: one component is oriented toward orchestration, while another focuses specifically on computer interaction.

TypeSafe AI's recently introduced Jev offers another architecture signal worth watching. Jev is not positioned primarily as a general conversational model. Instead, it produces typed probabilistic judgments that software can consume directly, such as classifications, routing decisions, scores, or other bounded decisions. This should be treated cautiously: Jev is currently better understood as a product and architecture signal than as evidence equivalent to peer-reviewed research. Still, it illustrates an interesting possibility. Specialization may not only be organized around domains such as OCR or coding; it may also be organized around roles within an AI system, such as perception, reasoning, planning, routing, computer use, decision-making, and verification.

The future AI system may therefore look less like one increasingly powerful model doing everything and more like **Generalist + Specialists + Tools + Routing + Harness**. The more interesting question may not be whether small models will replace large ones, but whether **AI capability itself is becoming increasingly compositional**.

## The Engineering Unit May Be Shifting from the Model to the System

Once capability is distributed across multiple components, discussing model capability alone begins to leave out important parts of the picture. A frontier model may have extremely strong reasoning capabilities, but the overall system can still fail if context management is poor, tool integration is unreliable, execution state is lost, routing selects the wrong component, or a verifier checks the wrong artifact.

The reverse can also be true. A smaller model assigned to a clearly bounded specialist role, with stable inputs and well-defined tools, may become a highly effective component of the larger system.

This leads to an increasingly important distinction: **Model competence ≠ System competence.** This does not make benchmarks irrelevant, nor does it make model intelligence less important. It simply means that as AI begins to perform real work, the unit we need to observe is becoming larger.

The question used to be primarily, “Which model is better?” Increasingly, we may also need to ask, **“Which system can reliably complete the work?”**

And once the engineering unit expands from the model to the system, a new set of problems appears.

## When Execution Becomes Distributed, We Need to Know What Actually Happened

Consider a simple AI system:

`Prompt → Model → Output`

Even when the result is wrong, the space of possible explanations remains relatively small.

Now consider a more composed system:

`Generalist → Router → Specialist → Tool → Verifier → Human / Policy → Action`

If the final result is wrong, where did the failure enter the system? Was the initial observation incorrect? Did the generalist misunderstand it? Did the router select the wrong specialist? Did a tool return stale information? Did the verifier actually run, and did it verify the correct artifact? Or were all the upstream judgments reasonable, only for the final policy to translate them into the wrong action?

This turns execution provenance into a practical engineering problem.

A recent study, *Plans They Abandon, Reports They Author*, analyzed 5,851 real developer sessions containing 355,942 tool calls. The researchers found that agent self-reports mentioned roughly one out of every eleven actual actions, and that a reader relying only on those reports could reconstruct only about one fifth of the underlying execution log.

The study examines coding-agent reports rather than multi-model architectures specifically, so it should not be used to claim that composed AI systems necessarily have a provenance problem. What it does demonstrate is a problem that already exists: **execution narrative is not the same as execution record.**

An agent may finish by saying, “I checked the repository, fixed the problem, and ran the tests.” That is a narrative. The actual execution history may contain dozens or hundreds of observations, tool calls, failed attempts, retries, and intermediate decisions. As more components participate in the same piece of work, the distinction becomes increasingly important.

## Execution Records May Become an Important Engineering Asset

If multiple AI components contribute to the same task, we may eventually need to preserve more than the final output. We may need to know which component performed an action, what input or system state it observed, what observation or judgment it produced, which tool call actually executed, which artifact was modified, which candidate was verified, what verification actually ran, which decision changed system state, and which policy or authority allowed execution to continue.

That is more than conventional debug logging. It is closer to **an evidence trail that allows us to reconstruct how AI work actually happened**.

Such records matter for debugging, evaluation, and failure analysis. If system capability emerges from the combination of generalists, specialists, tools, and verifiers, then without execution provenance it may be difficult to determine which component actually produced an improvement. When the system fails, it may be equally difficult to determine whether the right fix belongs in the model, prompt, router, tool, context management, verification mechanism, or surrounding system.

Execution records may therefore become valuable for more than auditing. They may become an important source of data for understanding and improving AI systems themselves.

## More Verification Does Not Automatically Mean More Trust

One natural response to increasingly complex AI systems is to add more verification. That is an important direction, but verification has boundaries of its own.

SWE-Proof studied 500 real-world software engineering issues. The researchers found that formal verification could still identify counterexamples in roughly one quarter to one half of patches that had already passed held-out tests. This suggests that stronger verification can expose problems that conventional testing misses.

The same study, however, also revealed a deeper limitation. When models had to generate their own formal specifications, the benefits of verification were substantially constrained. Only 62% of those specifications passed the researchers' audit, with specification fidelity emerging as an important issue: a model might correctly constrain part of the required behavior while leaving other requirements unspecified.

This exposes an important distinction: **Verification strength ≠ Specification fidelity.** A verifier can rigorously prove that an implementation satisfies a specification, but if the specification omits an important requirement, the proof still does not establish that the system implemented what was actually intended.

Adding a verifier to a composed AI system therefore does not automatically solve the trust problem. A verifier provides another source of evidence. We still need to ask what was verified, against which specification, for which artifact, and what that evidence actually establishes.

## Judgment, Policy, and Action May Need to Be Separated

As specialized models take on different roles, another boundary becomes more visible: a model may be very good at making a judgment without necessarily being the component that should have authority to act on it.

Suppose a decision-oriented model estimates that an operation is safe with a probability of 0.94. That result is a judgment. The surrounding system must still determine whether 0.94 is sufficient, whether values between 0.90 and 0.95 require additional verification, when execution must stop, and which operations require human approval regardless of confidence.

There are therefore at least three different responsibilities involved: **Judgment, Decision Policy, and Execution Authority.** They are not interchangeable.

Coinbase's Autopilot quality system provides one production engineering example of this separation. It combines agentic and deterministic components: agents are used where language reasoning and exploration are useful, while deterministic services provide repeatable execution. The system also uses LLM-based scoring, while explicitly acknowledging that model judgments can be wrong. Those scores become inputs to human review and release gating rather than allowing the model to decide release on its own.

The interesting point is not that one particular implementation is universally correct. It is the broader engineering question: **probabilistic intelligence can participate in a decision, but whether it should own final authority is a separate design choice.** As specialist models become more common, this distinction may become increasingly important.

## Human Review Also Needs to Find a New Place

There is another practical constraint. If AI systems become capable of producing ten times as much engineering output, the obvious answer cannot always be to ask humans to review ten times as many artifacts. Human attention is itself a limited resource.

AdaCore's GNAT Foundry: Intersection explores a related problem in high-integrity software development. Its approach uses deterministic proof, testing, coverage, and traceability to establish machine-checkable evidence so that human attention can be concentrated where judgment is still required. High-integrity Ada/SPARK development differs substantially from ordinary software engineering, so this approach should not simply be generalized to every AI development environment. But the question it raises is useful: **Does the human-review surface really need to be as large as the AI-output surface?**

Perhaps not. Some questions may increasingly be answered through machine-verifiable evidence, while others remain difficult to automate: intent, ambiguous requirements, architectural trade-offs, acceptable risk, conflicting objectives, consequential actions, and final acceptance.

The more useful question may therefore be less “How do we keep humans in every loop?” and more **“Where does human judgment actually matter?”**

## From Model-Centric to System-Centric Engineering

Taken together, these developments do not yet describe a settled future architecture. The evidence comes from different sources—peer-reviewed research, empirical agent studies, production engineering systems, model releases, and product architectures—and those sources do not all carry the same evidentiary weight. It would therefore be premature to claim that AI will inevitably converge on one particular architecture.

Still, the developments point toward a direction worth watching. AI is expanding from **a stronger individual model** toward **a more complete working system**. Capability may come from a generalist or a specialist, from tools or routing, from an execution harness, or from verification and human judgment.

As a result, the core questions in AI-assisted engineering may also be expanding. We still need to ask, **“Can the AI do the work?”** But increasingly, we may also need to ask: **What system actually did the work? How was the work produced? What evidence was created along the way? Which parts were independently verified? Where did judgment come from? Who had authority to act? And what should we trust this system to do next?**

This is why I increasingly suspect that one of the more important changes ahead is not simply small models replacing large models, or multi-agent systems replacing single agents. The deeper shift may be that **AI engineering is moving from model-centric engineering toward system-centric engineering.**

When we interact with a single model, we primarily care whether it can produce the right answer. When we work with a system composed of models, tools, agents, verifiers, and people, the answer itself is no longer the whole engineering problem. We also need to understand how the result was produced, which evidence deserves confidence, which uncertainties remain, and ultimately what we are willing to let the system do next.

That may be one of the more important engineering problems emerging as AI gets better at doing the work.

## References

1. **OpenAI — [*Introducing the Agents API*](https://openai.com/index/introducing-the-agents-api/)**\
   Agent harness architecture, tool use, context management, and long-running execution.

2. **Microsoft Research — [*MagenticLite, MagenticBrain, Fara1.5: An agentic experience optimized for small models*](https://www.microsoft.com/en-us/research/blog/magenticlite-magenticbrain-fara1-5-an-agentic-experience-optimized-for-small-models/)**\
   An example of specialist models, orchestration, and harness design being developed as parts of a larger agent system.

3. **Cui et al. — [*PP-OCRv5: A Specialized 5M-Parameter Model Rivaling Billion-Parameter Vision-Language Models on OCR Tasks*](https://openaccess.thecvf.com/content/CVPR2026/html/Cui_PP-OCRv5_A_Specialized_5M-Parameter_Model_Rivaling_Billion-Parameter_Vision-Language_Models_on_CVPR_2026_paper.html), CVPR 2026**\
   Research evidence for specialized models.

4. **Shen et al. — [*Vertical Routing: A Cost-Efficient Collaboration Routing Framework*](https://aclanthology.org/2026.tacl-1.104/), TACL 2026**\
   Research on collaboration between large and small models across different task stages.

5. **TypeSafe AI — [*Introducing System One Models & Jev*](https://typesafe.ai/blog/introducing-system-one-models-and-jev)**\
   A recent product and architecture signal for decision-specialized models; treated here as an industry observation rather than independent research evidence.

6. **Kraishan & Jitkajornwanich — [*Plans They Abandon, Reports They Author: The Narrative Layer of Autonomous Agents*](https://arxiv.org/abs/2609.12205)**\
   An empirical study of the gap between agent self-reports and underlying execution histories.

7. **Ma et al. — [*SWE-Proof: Can Language Models Resolve Real-World Issues with Machine-Checked Proofs?*](https://arxiv.org/abs/2609.21190)**\
   Research on the potential and limitations of formal verification, including specification fidelity.

8. **Coinbase Engineering — [*Autopilot: Engineering an Agentic Quality Loop for Support Automation*](https://www.coinbase.com/blog/autopilot-engineering-an-agentic-quality-loop-for-support-automation)**\
   A production engineering example combining agentic judgment, deterministic execution, release gating, and human review.

9. **AdaCore — [*GNAT Foundry: Intersection — Demonstrating Trustworthy AI Development*](https://www.adacore.com/gnat-foundry-intersection)**\
   An engineering demonstration of deterministic evidence and focused human review in high-integrity software development.
