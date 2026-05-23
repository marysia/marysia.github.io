---
title: "Module 10: AI Evaluations in Governance and Policy"
date: 2026-04-14
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

Evaluations and governance shape each other. Voluntary frontier safety frameworks tie evaluation results to deployment decisions inside labs, while a new wave of regulation (the EU AI Act's codes of practice, California's SB 53, and New York's RAISE Act) is starting to push back on what counts as a credible evaluation. This lecture maps that landscape and shows how the requirements interact in practice.

This lecture was taught by Patricia Paczkowski from RAND and the Oxford Martin AI Governance Initiative.

---

## Key Takeaways

1. Frontier safety frameworks operate on an *if-then* logic: a lab defines a capability threshold, runs evaluations against it, and pre-commits to safeguards or pauses if the threshold is crossed, but in current frameworks the thresholds are mostly qualitative, leaving wide discretion to the lab.
2. Voluntary self-governance is fragile under competitive pressure: Anthropic's RSP v3 dropped its preset capability levels and pause commitments on the explicit reasoning that unilateral hard commitments without industry-wide matching can be counterproductive.
3. US state laws (SB 53, RAISE) mandate *reporting* of evaluations rather than evaluations themselves, so the regulatory bite comes through transparency obligations and third-party audit rights, not through a list of required tests.
4. The EU codes of practice are the first instrument to specify methodological standards for frontier evaluations: internal validity, external validity, reproducibility, elicitation matching real threat actors, mitigation testing under adversarial pressure, and minimum qualifications for evaluation teams.
5. Pre-deployment evaluation is necessary but insufficient: the codes of practice also require incident reporting and post-market monitoring, recognizing that systemic risks like manipulation only become visible at scale and over time.

**Concepts**
- **Frontier safety framework (FSF) / Responsible scaling policy (RSP)**: A lab's voluntary commitment tying capability thresholds to safeguards, evaluation cadence, and deployment decisions.
- **Capability threshold (or critical capability level, CCL)**: A defined level of model capability above which a specific risk is deemed unacceptable without further mitigation.
- **Elicitation**: The work of surfacing a model's true capability (through scaffolding, fine-tuning, tool access, and adversarial prompting) rather than measuring its default behavior.
- **Model / system card**: Documentation released alongside a model that describes the system and the evaluations run on it.
- **Codes of practice**: The EU instrument operationalizing Article 55 of the AI Act for general-purpose AI models with systemic risk. Signing on creates a presumption of conformity.
- **Presumption of conformity**: A legal posture in which following a code is treated as compliance with the underlying obligation. Non-signatories must prove conformity by other means.
- **Third-party evaluator**: An external organization, independent of the developer, that runs or audits evaluations.
- **Post-market monitoring**: Structured collection of capability and harm data from deployed systems, used to update risk assessments after release.

---

## Detailed Notes

### Why governance and evaluations are entangled

Evaluations shape governance because they generate the evidence that policymakers and labs use to decide whether a model is safe to train further, to deploy, or to deploy to specific users. Governance also shapes evaluations: when a law or framework asks for a particular kind of evidence, evaluation practice rearranges itself to produce it. Understanding both directions matters even for technical researchers, because regulatory and institutional incentives increasingly determine which evaluations get funded, run, and trusted.

The current landscape has three interlocking pieces: *voluntary frontier safety frameworks* published by individual labs, *model and system cards* that document specific releases, and a growing *body of binding regulation*. Each piece references the others. A US state law typically requires the lab to maintain a safety framework and publish a transparency report. The EU codes of practice require both plus an evaluations regime with methodological standards.

This lecture focused on the slice of governance aimed at *frontier* models and *systemic* risks (Article 55 of the EU AI Act and its codes of practice, plus SB 53 and RAISE in the US). Other regimes covering high-risk applications (hiring, credit, medical) are out of scope.

### Voluntary governance: frontier safety frameworks

The first frontier safety framework was published by Anthropic in 2023, building on a proposal from METR earlier that year. The Bletchley and Seoul summits in 2023–24 pushed more labs to sign on, and by the Paris Summit in 2025 twelve labs had released frameworks. Names vary (Anthropic calls theirs a responsible scaling policy, OpenAI a preparedness framework), but the structure is consistent.

**The if-then structure.** A framework defines one or more capability thresholds for each risk category. It commits the lab to running evaluations against those thresholds at specified points (e.g., before deployment, on a cadence during training, after major capability gains). If a threshold is approached or crossed, the framework pre-commits the lab to a particular safeguard or to halting development or deployment.

**Risk coverage.** Across the twelve current frameworks, biological and chemical weapons appear in ten, offensive cyber operations in nine, and AI R&D, misalignment, and loss of control in most. A few cover autonomous replication and harmful manipulation.

<div class="example-box" markdown="1">
**OpenAI's preparedness framework, biological risk**
The framework defines a *high* and a *critical* capability threshold for biological uplift, paired with safeguard tiers. High triggers additional security controls. Critical triggers a commitment to halt further development. The thresholds themselves are written qualitatively (phrased in terms of the kind of uplift the model would provide to a relevant actor), which leaves substantial room for interpretation when the evaluations come back.
</div>

METR has catalogued nine common elements across these frameworks. Eight of twelve define capability thresholds. Seven specify elicitation techniques. Nine specify evaluation frequency. All twelve discuss accountability mechanisms including, in some cases, third-party evaluations. Halting and mitigation commitments vary much more.

**Anthropic's V3 paradigm shift.** In February 2026, Anthropic released V3 of its responsible scaling policy and removed its preset capability thresholds and pause commitments (it also dropped its ASL-5 weight-security commitment). The accompanying blog post and Holden Karnofsky's longer write-up framed this as a response to two problems. First, preset thresholds had proven far more ambiguous in practice than anticipated, and the science of evaluation was not mature enough to make them load-bearing for binary deploy/halt decisions. Second, unilateral commitments without competitor matching create a collective action problem: a lab that pauses on its own simply cedes ground to less cautious developers. V3 separates Anthropic's own planned mitigations from a more ambitious set of recommendations that would only apply if industry-wide adoption could be secured.

### Model and system cards

Model and system cards are not a governance framework on their own. They are the documentation layer where the commitments in a safety framework become visible to outsiders. A good card describes the system, the evaluations run, and how those evaluations relate to the lab's capability thresholds.

In practice, the quality varies. Cards frequently assert that a critical capability threshold was not reached without showing the underlying methodology in enough detail to verify the claim. Third-party evaluations, when mentioned, are often summarized as "corroborated our findings" without disclosing what those findings were or how the third party arrived at them.

<div class="example-box" markdown="1">
**Reading Gemini 3 Pro's frontier safety report on harmful manipulation**
The report defines a qualitative capability threshold for harmful manipulation and concludes it was not reached. The supporting evidence runs roughly two pages and includes an attempt to separate *propensity* (will the model try to manipulate) from *efficacy* (does it actually change beliefs), an internal–external evaluator split, and a human baseline. But the experimental design uses a between-subjects setup with flashcard-style stimuli rather than a within-subjects, multi-turn chatbot interaction, and manipulation is plausibly a longer-horizon effect that won't surface in short exposures. Quantitative grounding is sparse and the qualitative judgments of "low uncertainty" are not unpacked. The report ticks the safety-evaluation boxes but, on close reading, does not provide enough methodological detail to independently verify the claim.
</div>

The lesson here is general: when a card says a threshold was not reached, treat the claim as a hypothesis that needs visible methodology, not as a finding.

### Regulation: state laws

Three pieces of binding regulation now apply to frontier model providers. New York's RAISE Act and California's SB 53 are close cousins, and the EU AI Act's Article 55 plus its codes of practice form the most detailed regime.

The state laws apply to models trained at or above 10^26 FLOPs. The EU codes of practice apply one order of magnitude lower, at 10^25. The state laws are slightly more specific about what counts as a risk or harm threshold. The codes of practice leave that to the developer's safety framework.

**What the state laws actually require.** SB 53 mandates that a developer maintain a frontier safety framework with ten required elements and publish a transparency report covering evaluation results. Crucially, the law does not directly mandate that evaluations be run. It mandates that whatever evaluations *are* run be reported, and that the framework describe how they would be run. The intended effect is similar but the mechanism is reporting, not prescription. Third-party evaluation is treated the same way: the law requires the developer to disclose the extent to which third parties were used, not to use them.

RAISE is similar with one important difference: it does not require public release of evaluation results in the way SB 53 does. Instead it requires developers to retain results on hand for annual third-party audits.

Three of the ten required framework elements are evaluation-relevant: defining and assessing capability thresholds for catastrophic risk, using third parties to assess catastrophic risks and the effectiveness of mitigations, and (distinctively) assessing catastrophic risk from *internal* use of frontier models. The last element matters because almost every other piece of governance discussed here applies to systems put on the market. Internal deployments, including agentic systems used inside the developer, were largely unregulated until this.

### Regulation: the EU codes of practice

The EU AI Act's Article 55 mandates that providers of general-purpose AI models with systemic risk perform model evaluations. The article itself does not say what an adequate evaluation looks like. The codes of practice, finalized in 2025, fill that gap as a presumption of conformity: sign on and follow them, and you are presumed to comply with Article 55. Other paths to compliance exist, but require demonstrating equivalence.

The safety and security chapter is where the methodological substance lives. Measure 3.2 calls for "at least state-of-the-art" model evaluations and lists candidate methods (Q&A sets, task-based benchmarks, red teaming, adversarial testing, human uplift studies) without prescribing which to use. This is deliberate. Hard-coding methodology in a code that is hard to update would lock in today's evaluation techniques as tomorrow's compliance ceiling.

It also lays out five requirements for evaluations.

**Rigor.** Evaluations must be internally valid (no methodological shortcuts or biases inside the evaluation environment), externally valid (results must generalize to deployment conditions), and reproducible (a third party with the same code, data, and conditions should be able to reproduce the result). The reproducibility requirement is carefully worded around reproducing *results* with the same inputs, not re-running the entire experiment from scratch. That matters for evaluations that involve human participants, where exact replication of a sample is effectively impossible.

**Elicitation.** Evaluations must surface actual capability, not default behavior. Elicitation effort must be at least as capable as the threat actors implied by the risk scenario. That implies scaffolding, tool access, fine-tuning where appropriate, and active work to minimize sandbagging risk. A short, unscaffolded zero-shot probe is not an adequate elicitation against a sophisticated adversary.

**Mitigation testing.** Safeguards have to be tested under adversarial pressure: can they be circumvented, deactivated, or subverted? How does their effectiveness change over time and across downstream deployment contexts? Mitigations that depend on downstream actors (application developers, system integrators) are particularly hard to evaluate because the relevant adversarial pressure is generated outside the lab.

**Teams and resources.** Evaluation teams must have appropriate technical expertise (the code suggests indicators such as relevant PhDs, published work in the topic area) and must be given enough time, model access, information, and compute. This addresses a recurring failure mode: a lab decides to deploy, then asks the evaluation team for a verdict on a compressed timeline with limited access.

**External evaluators.** External evaluation is required by default. Exemptions must be justified: either the model is sufficiently safe on independent grounds, or no qualified external evaluators exist for the relevant risk domain. The code does not yet specify the *relationship* between internal and external evaluation teams. Financial auditing offers a useful analogy: that field has worked out detailed independence rules over decades. Frontier AI has not.

### Beyond pre-deployment

All three regulatory regimes require incident reporting within defined windows after a severe incident. The codes of practice go further with Measure 3.5, requiring *post-market monitoring*: structured collection of capability and harm information from deployed systems, plus guarantees of external evaluator access to the most capable production version of the model and to its chain of thought where applicable.

This matters because systemic risks like manipulation, dependence, or subtle epistemic harms only become measurable at scale. Pre-deployment evaluations can rule out the most acute hazards but cannot anticipate every interaction pattern, integration, or downstream application that will emerge in the wild. Real-world data is the only way to close the loop.

A useful concrete agenda for post-market monitoring of harmful manipulation: periodically sample real user interactions, look for shifts in belief, dependence, or affective dynamics across cohorts, and flag patterns for deeper investigation. The hardest open question is who decides when monitored signals cross from "interesting" to "unacceptable" (Measure 4.2 of the codes asks signatories to justify how they determine risk acceptance), but the threshold remains genuinely under-specified.

---

## Conclusion

The shape of frontier AI governance has changed quickly. In three years it has gone from a single voluntary policy to a network of voluntary frameworks, transparency-based US state laws, and a methodologically detailed EU regime. Evaluations sit at the center of all of it, because evaluations are what every part of the system uses to ground its claims.

The practical implication for anyone working on evaluations is that methodology now has a regulatory audience. The codes of practice will not, on their own, settle questions about how to elicit a capability, how to estimate reproducibility for a human study, or how to verify that a third-party evaluator is genuinely independent. Those questions are being delegated to the EU AI Office, to standards bodies, to insurance and audit markets, and to the technical community itself. Be skeptical of any safety claim that does not show its evaluation methodology in enough detail to be re-examined. When running an evaluation, document not just the result but the limitations, so that a future reader can tell what the evidence does and does not support.

Voluntary commitments are vulnerable to competitive pressure. The regulatory layer is the part of the system designed to resist that drift. Whether it can do so depends on whether the evaluations underneath it are good enough to carry the weight.
