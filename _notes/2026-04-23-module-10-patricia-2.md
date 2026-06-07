---
layout: note
title: "Meta-Evaluation"
course: "AI Evaluation: Capabilities & Safety"
course_id: eval
module: "10"
module_title: "Governance, Policy and Regulation"
lecturer: "Patricia Paskov"
lecturer_affiliation: "RAND Corporation & Oxford Martin AI Governance Initiative"
date: 2026-04-23
---


## Overview

If frontier safety frameworks, regulators, insurers, and enterprise buyers all rely on AI evaluations to make consequential decisions, then a prior question matters more than any single benchmark: who is making sure the evaluations are good enough? This lecture works outward through four concentric layers: running a rigorous evaluation, synthesizing evidence across many of them, expanding evaluation capacity in the ecosystem, and enabling genuinely independent third-party evaluation.

This lecture was taught by Patricia Paskov from RAND and the Oxford Martin AI Governance Initiative.

---

## Key Takeaways

1. The credibility of AI evaluations depends not only on methodology but on the institutions, incentives, access regimes, and broader ecosystem that surround them. A scientifically strong evaluation embedded in an institutionally weak system is insufficient.
2. A state-of-the-art evaluation, by the EU codes of practice criteria, must be internally valid, externally valid, and reproducible, and existing evidence (an agent scoring 100% on SWE-Lancer without completing any tasks, 76-point swings from prompt formatting, median human-baseline sample sizes of eight) shows the field is not yet there.
3. ISO's standard three-year development cycle does not match a frontier where 50%-completion task length has gone from four minutes to nearly six hours in three years, so best-practice hubs need faster venues. Candidates include the AI Evaluators Forum, the Evals Coalition's consensus process, and AI insurers.
4. Proportionality offers a usable test for how much evaluation is enough: suitability (is the evaluation effective evidence for the claim), necessity (is there a lighter alternative of equal effectiveness), and whether the value of the evidence justifies the burden of access, compute, and IP exposure.
5. Third-party evaluation today is constrained on four dimensions (access, mostly black-box, time, often days rather than weeks, autonomy, since developers can reshape findings, and transparency, given NDAs and sparse public reporting). A healthier ecosystem requires improvement on all four.

**Concepts**
- **Meta-evaluation**: Judging the conditions (methodological, institutional, and ecosystem-level) under which evaluation evidence can responsibly govern decisions.
- **Internal validity**: Results reflect the truth in the evaluation setting and are not artifacts of methodological shortcuts.
- **External validity**: Results generalize as a proxy for model behavior in deployment contexts outside the evaluation environment.
- **Reproducibility**: A third party with the same code, data, and conditions can reproduce the result.
- **Proportionality**: A three-part test (suitability, necessity, burden-justification) for whether a given evaluation is the right level of effort for a given claim.
- **Ladder of evaluations**: A progressive framework that starts with light-touch probes and escalates to costly methods (control trials, uplift studies) only when warranted.
- **Independent verification organization (IVO)**: A government-licensed third party that certifies frontier developers within a regulatory market.
- **Post-deployment monitoring**: Structured collection of real-world capability and harm data, used both to validate pre-deployment evaluations and to surface novel risks.

---

## Detailed Notes

### Why meta-evaluation matters now

Consequential decisions are already running on evaluation evidence. A frontier developer decides whether a model crosses a critical capability threshold before deployment. A regulator assesses compliance with the EU AI Act. An enterprise buyer decides whether to procure a service. An insurer prices AI risk and writes coverage. In each of these, an evaluation result is doing the work of justifying a real-world action.

The motivating chain is simple. Consequential decisions rely on evaluations. Evaluations, by the field's own assessment, are not yet up to the task. A scientifically strong evaluation embedded in an institutionally weak system still fails. Therefore the answer has to be both scientific and institutional. The rest of the lecture works outward through four layers: the science of running one evaluation well, synthesis across many, capacity in the ecosystem, and independent third-party assessment.

The frame for the lecture is dangerous capability evaluations of frontier AI systems. Some lessons generalize to other contexts. The specifics are calibrated to that setting.

### Layer 1: Running a rigorous evaluation

The EU AI Act codes of practice define a state-of-the-art evaluation along three axes. Internal validity: the result represents the truth in the setting, free of methodological shortcuts. External validity: the result can stand in as a proxy for behavior outside the evaluation environment. Reproducibility: running the same evaluation again yields roughly the same result.

By every one of these criteria the current field has gaps. One agentic benchmark study showed an agent scoring 100% on SWE-Lancer without completing any of the actual tasks. Subtle changes in prompt formatting have been shown to swing performance by up to 76 accuracy points on open-source LLMs. Overfitting, reward hacking, and sandbagging undercut external validity. Human baselines, frequently used to contextualize AI evaluation results, often rest on tiny samples. One survey found a median of eight participants.

<div class="example-box" markdown="1">
**Concrete guidance documents already exist**
The BetterBench paper walks through the full life cycle of a benchmark (design, implementation, documentation, maintenance, retirement) and lays out best-practice guidance at each stage. NIST AI 800-2 specifies practices for automated benchmark evaluations of language models. The Agentic Benchmark Checklist (ABC) and STREAM provide reporting checklists. A RAND working paper offers preliminary recommendations for rigorous model evaluations, and a separate piece walks through agentic biological evaluations stage by stage. The raw material for institutionalizing rigor is on the page. The open question is where it lives.
</div>

The institutional question is which body should host and operationalize these best practices. Four properties matter: access to expertise, independence from developers, integration with how evaluations actually get run, and the ability to keep pace with the frontier.

ISO is the natural first thought, but ISO standards take roughly three years from first proposal to publication. Over the most recent three-year window, the METR time-horizon plot shows the task length at which frontier LLMs succeed 50% of the time growing from about four minutes to about five hours and forty-two minutes. A standard-setting cadence that lags the underlying object by that much will not bind.

Other candidates have their own trade-offs. The AI Evaluators Forum, launched in December 2025 as a body of non-profit evaluators, has already issued a standard on operating conditions for evaluations and is a plausible home. Regulators have authority but move slowly and struggle to recruit the necessary expertise. The Frontier Model Forum has expertise but is funded by the developers it would assess and so fails the independence test. AI insurers may emerge as a forcing function, since pricing AI risk requires defensible evaluation methodology. The Evals Coalition's Evals Consensus, a Delphi process that aggregates community input on best practices, is another live venue.

### Layer 2: Synthesizing evidence across evaluations

One rigorous evaluation, even done well, is not a body of evidence. A frontier safety framework with a qualitative critical capability threshold (for example, OpenAI's preparedness framework threshold for a model enabling an expert to develop a highly dangerous novel threat vector) cannot be assessed by a single test.

<div class="example-box" markdown="1">
**The threat model for building a bioweapon**
A RAND threat model decomposes the path from initial intention to release into multiple stages, including an iterative design-build-test-learn cycle. Each stage admits many ways the threat can play out and many ways an AI system might provide uplift to a human actor. One published evaluation in this area focused only on the design-build pathway and required substantial time, manpower, and methodology to produce. Evaluating the full threat model end-to-end is a far heavier undertaking, which is why synthesis across many evaluations matters more than perfecting any single one.
</div>

Two questions follow. The first is scientific: how do results combine into trends across evaluations and models? Shared platforms help: the Inspect framework standardizes how evaluations are run and creates knowledge spillovers between teams. The Evals Coalition's Every Eval Ever project provides a taxonomy and structured storage for evaluation results, enabling meta-analysis. Shared task libraries and shared documentation conventions deliver economies of scale. Better documentation (pre-analysis plans, reporting checklists, model and system cards) makes it clearer what any one evaluation does and does not represent.

The second question is pragmatic: how much evidence does a decision-maker need before acting? A *ladder of evaluations* matches depth to stakes. Begin with light-touch probes. Escalate to control trials or human uplift studies only when warranted. A recent *Science* paper, born of an EU AI Office workshop, operationalizes this through three proportionality tests. **Suitability**: does the evaluation provide effective information about the claim (realistic, sensitive, specific, rigorous)? **Necessity**: is there a lighter alternative of equal effectiveness? **Burden justification**: does the evidence produced justify the time, compute, access, and IP exposure required to produce it?

What "effective" means in the suitability test is the part the field has not nailed down. The honest direction is to validate pre-deployment evaluations against post-deployment outcomes and to build feedback loops between the two, rather than assert effectiveness by stipulation.

<div class="example-box" markdown="1">
**Operationalizing a manipulation threshold**
The Google DeepMind frontier safety framework defines a critical capability level for harmful manipulation: the model can systematically and substantially change beliefs and behavior in identified high-stakes contexts over the course of interactions, resulting in additional expected harm at severe scale. Applying proportionality here means starting with proxy benchmarks and simulations, escalating to human-subjects studies only when initial signals warrant it, and recognizing that a DeepMind paper on cross-cultural variation in harmful manipulation finds the construct is highly context-dependent: manipulation that is meaningful in one geography or domain may not transfer to another. The number of conditions under which the model must be tested expands accordingly, and the technical evaluation becomes a social-science evaluation as much as an ML one.
</div>

The proportionality test also has a "for whom" dimension. Burdens on a frontier developer (compute, time, regulatory compliance cost) are visible. Harms to individual users are diffuse and largely unobserved. A framework that balances burden against value has to specify which side of the ledger each is being measured against.

### Layer 3: Expanding evaluation capacity

Frontier developers cannot grade their own homework, both for incentive reasons and because some laws now forbid it. The EU AI Act mandates third-party evaluations. SB 53 and the RAISE Act push in the same direction through transparency and reporting requirements rather than direct mandate. The surface area is large: six risk domains appear across the twelve current frontier safety frameworks, and the rate of model capability change continues to outrun evaluation supply. Auditors need deep expertise in both AI and the relevant risk domain (bioinformatics, lab biology, cyber operations, persuasion research), which makes the talent pipeline a serious bottleneck.

The credit-rating-agency failure of 2008 is the cautionary analogy. A small number of dominant raters, with limited independence and limited diversity in approach, produced a fragile evidence base. The same risk applies to AI evaluation if the ecosystem stays concentrated. Diversity and competition in auditors improves integrity, generates knowledge spillovers, and reduces blind spots.

<div class="example-box" markdown="1">
**Regulatory markets and independent verification organizations**
Gillian Hadfield and Jack Clark's 2023 proposal for regulatory markets (building on Hadfield's earlier work) imagines legislation that licenses independent verification organizations (IVOs) to certify whether frontier developers meet specified safety conditions. The IVOs compete on quality of assessment within a regulated market structure. FATLM is one organization actively developing this model. The aim is to combine the discipline of competition with the legitimacy of government oversight, without putting the technical assessment work inside the regulator itself.
</div>

Several levers can expand capacity. Philanthropy and venture capital can fund new evaluation organizations, methodological research, and talent development through fellowship programs. Insurance, regulation, and enterprise procurement create market demand for evaluations on the developer side. Government can use the regulatory-markets approach to license IVOs. Public-interest AI safety research is currently underfunded relative to the size of the problem.

Some risks are systematically neglected because no individual developer has the incentive to evaluate for them. Multi-agent risk is the clearest example: chains of liability in the AI Act and the codes of practice attach to individual models or agents, not to the emergent behavior of agents interacting in an economy. No developer is naturally positioned to evaluate that, and the market alone will not produce the evidence.

### Layer 4: Enabling independent evaluation

Two recent pieces sketch what robust independent evaluation requires. Frontier AI Auditing, from an organization launched by Miles Brundage, lays out rigorous third-party assessment of safety and security practices. The AI Evaluators Forum has published minimum operating conditions for independent third-party evaluations. Both converge on four dimensions: access, independence, autonomy, and transparency.

**Access today is constrained**. Most external evaluators get black-box access only. Evaluation windows run on compressed pre-release timelines, often days rather than weeks. The codes of practice mandate at least twenty business days for substantially novel systems, which is the floor, not the ceiling. Access should be proportionate to the assurance level being sought. The codes of practice appendix lays out a typology of access levels matched to evaluation depth.

<div class="example-box" markdown="1">
**Apollo's note in the Claude Opus 4.6 system card**
Apollo's contribution to the Claude Opus 4.6 system card noted that developing the relevant experiments would have taken a significant amount of time, and that they therefore declined to provide a formal assessment of the model at that stage. The transparency of the statement is unusual and welcome. Many third-party evaluations operate under NDAs that would prohibit even that level of disclosure. The substantive problem it documents, though, is the standard one: insufficient time to run the evaluation the question deserved.
</div>

**Independence has no established standard.** Best practices for how third-party auditing organizations should be structured, manage conflicts of interest, and avoid direct financial stakes in the developers they assess have not been written down. Financial auditing has worked these questions out over decades. AI evaluation has not. Payment that depends on audit results, and undisclosed conflicts of interest, are both live concerns.

**Autonomy is weak**. Providers can currently redact or reshape findings before they reach the public. An evaluator should control the scoping of the work, the methodology, and how results are presented. Today none of those are guaranteed.

**Transparency is sparse**. Public reporting of methods and operating conditions is rare. A deep read of even a recent Gemini frontier safety report shows that the methodology behind evaluation claims is often impossible to assess from the outside. Information hazards are real (biorisk and cyber-risk evaluations cannot be fully disclosed), but tiered or secure information-disclosure mechanisms can let qualified parties verify results without broadcasting hazards.

Threat modeling for novel risks under uncertainty is a related open challenge. Two directions help. Post-deployment data surfaces emerging threats that pre-deployment evaluations would not have anticipated, feeding back into new evaluation design. AI-assisted exploration of a wide action space (using AI to probe for novel attack vectors against AI) can scale threat enumeration beyond what a fixed expert panel can produce. Neither replaces the need for deep domain expertise. A bioinformatician and a lab biologist generate different threat models from the same evidence. Convergence between policy and bench scientists is still scarce. The current EU AI Office work with Secure Bio, SaferAI, and similar groups is one of the few places that convergence is being practiced.

---

## Conclusion

The credibility of an evaluation is not a property of the evaluation alone. It is a property of the conditions under which the evaluation was produced: the methodology, the time and access granted, the independence of the team, the documentation that survives, and the synthesis with other evidence that surrounds it. Treating any single number as a verdict ignores most of what determines whether the number means anything.

Practical guidance follows directly. When running an evaluation, document not only the result but the limitations, so a downstream reader can tell what the evidence supports and what it does not. When reading a safety claim, ask which of the four layers it depends on: was the underlying evaluation rigorous, was the synthesis across evaluations adequate, was the team independent, and did they have the access and time to do the work. When designing institutions, build for proportionality and for synthesis, not for any single benchmark. Treat post-deployment monitoring as part of the evaluation system, not as a separate after-the-fact activity.

The ecosystem is still thin. Few third-party evaluators have the deep domain expertise that bio, cyber, and manipulation risk each demand. Best practices exist on paper but lack a settled institutional home. Independence standards for evaluators do not yet exist in the form financial auditing has had for decades. Each of these gaps is a place where the evidence base for high-stakes AI decisions is weaker than the decisions require, and each is a place where deliberate institutional work can close the gap.
