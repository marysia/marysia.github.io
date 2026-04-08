---
title: "Module 6: Measurement Theory for AI Evaluation"
date: 2026-03-18
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

This lecture covers how measurement theory from psychometrics provides frameworks for building more reliable evaluation systems that can support the kinds of claims we now need to make about AI systems.

This lecture was taught by Sanmi Koyejo.


## Key Takeaways

1. Benchmarks worked well for research competition but break down when used for deployment, policy, and safety decisions because the stakes, scale, and gaming incentives have fundamentally changed.
2. Validity failures explain why lab performance doesn't predict real-world behavior: construct validity (measuring what you claim), external validity (lab-to-deployment gaps), and consequential validity (gaming effects).
3. Statistical models like Item Response Theory can separate model capability from question difficulty, reducing evaluation costs while providing more reliable capability estimates.
4. Evaluation now costs up to 25% of total model development expenses at major labs, making efficient measurement methods an economic necessity rather than academic luxury.
5. Metric choice determines whether model abilities appear "emergent" or predictable, with probability-based metrics revealing smooth scaling patterns that exact match metrics obscure.
6. Post-deployment monitoring is essential because lab evaluations cannot capture all real-world failure modes, requiring continuous measurement systems rather than one-time testing.


---

## Detailed Notes


<div class="example-box" markdown="1">
**Scale Transformation** <br>
Research showed that simple character substitutions (changing 'a' to '@', random capitalization) could extract copyrighted books like Harry Potter and 1984 from production language models. Even with a jailbreak success rate as low as 0.001% in controlled lab testing, deployment at internet scale with millions of users transforms rare events into thousands of daily occurrences. The benchmark evaluation didn't predict this deployment reality because it failed to account for the scale transformation from lab to production use.
</div>


###  The Validity Framework

The current challenges in AI evaluation closely parallel the crisis that psychometrics faced in the 1950s when intelligence tests moved from research laboratories to life-altering decisions about military service and educational opportunities. Different IQ tests gave contradictory rankings of the same individuals, tests were optimized to look impressive rather than measure specific constructs, and the field struggled with questions of reliability and generalization. The psychometrics community's solution was to shift focus from validating tests to validating the inferences and claims made from test results.

This framework distinguishes between directly observable criteria (like accuracy on a specific dataset) and inferred constructs (like reasoning ability or safety) that must be built up through multiple lines of evidence. Validity is not a property of a test or benchmark but rather a property of the claims we want to make based on test results.

**Construct Validity** addresses whether an evaluation actually measures what it claims to measure. The GPQA benchmark illustrates this challenge well - it consists of graduate-level multiple choice questions in biology, chemistry, and physics that were designed to be "Google-proof" (laypeople with internet access couldn't answer them correctly). The directly observable criterion is clear: models can answer these specific multiple choice questions at some measurable rate. However, the construct claim that high performance indicates "graduate-level reasoning ability" requires additional evidence because multiple choice formats can be gamed, it's difficult to distinguish memorization from genuine reasoning, and the scope is limited to three scientific domains rather than general reasoning.

<div class="example-box" markdown="1">
**The GPQA Construct Gap** <br>
A model scoring 78% on GPQA demonstrates it can answer graduate-level multiple choice questions in three scientific domains. Claiming this shows "graduate-level reasoning ability" requires additional evidence about open-ended reasoning, problem-solving in novel domains, and the ability to explain reasoning steps - none of which multiple choice questions can assess.
</div>

**External Validity** captures the gap between controlled lab settings and messy real-world deployment conditions. Most benchmarks use single-turn question-and-answer formats, but deployment involves multi-turn conversations that introduce new failure modes. Lab evaluations happen at small scale with clean inputs, while deployment happens at internet scale with adversarial users, distribution shifts, and contexts the model has never encountered. Language differences, cultural variations, and the integration challenges of embedding AI systems into larger workflows all create external validity gaps that lab benchmarks cannot capture.

**Consequential Validity** focuses on how the measurement process itself changes behavior, particularly through gaming and optimization pressure. Goodhart's Law suggests that when a measure becomes a target, it ceases to be a good measure, and this effect is amplified in modern AI development where enormous resources are at stake. Models are increasingly optimized directly or indirectly on benchmark performance, benchmark datasets may be included in training data (intentionally or accidentally), and the competitive pressure to achieve high scores can overwhelm the goal of building genuinely capable systems.

### Item Response Theory

Current evaluation practices waste enormous resources because they treat every question as equally informative and every model response as equally meaningful. Major AI labs report that evaluation now consumes up to 25% of their total model development costs, driven by the need to test across thousands of benchmarks with hundreds of thousands of items each. Simple averaging approaches cannot distinguish whether a model failed because the question was inherently difficult or because the model lacks the relevant capability, leading to both inefficient resource allocation and unreliable capability estimates.

**Item Response Theory** provides a statistical framework for modeling the probability that a model answers a question correctly based on three key parameters: model capability (θ), item difficulty (β), and item discrimination (how well the question separates capable from incapable models). When model capability is high and item difficulty is low, the success probability approaches one, but when difficulty exceeds capability, success probability drops sharply. This mathematical relationship allows researchers to separate model properties from question properties in ways that simple averaging cannot achieve.

<div class="example-box" markdown="1">
**Non-Overlapping Test Sets** <br>
When Model A is evaluated on easy questions and Model B on hard questions, simple averaging might show identical 70% performance, suggesting equal capability. Item Response Theory accounts for difficulty differences and correctly identifies which model is actually more capable. This scenario occurs frequently in practice when comparing models across different benchmarks or when using adaptive testing approaches.
</div>

Based on this, we can use adaptive testing techniques that select questions based on model responses can reduce the number of evaluation items needed while maintaining the same quality of capability estimates. It also enables prediction of model performance on unseen questions, provides explanations for why certain items are difficult, and supports more reliable model comparisons even when test sets don't overlap perfectly.

Latent factor models extend this approach by decomposing model behavior across multiple benchmarks into a small number of underlying factors that capture different aspects of capability. Research typically finds that two or three factors explain most of the variation in model performance across diverse benchmarks, with the first factor capturing general capability (separating larger from smaller models) and subsequent factors capturing more specific capabilities like mathematical reasoning or code generation. This decomposition helps identify what constructs actually drive model behavior rather than assuming that benchmark labels accurately reflect the capabilities being measured.

<div class="example-box" markdown="1">
**Evidence Scaffold Example** <br>
To support a claim about "mathematical reasoning ability," combine: (1) performance on formal proof tasks, (2) ability to explain solution steps in natural language, (3) transfer to novel mathematical domains not seen in training, (4) robustness to different problem presentations, and (5) consistency across multiple solution attempts. No single benchmark can support this claim alone.
</div>

### Building Robust Evaluation Systems

The field needs to move beyond the leaderboard culture that encourages gaming and optimization for specific benchmarks toward evaluation systems designed to support the kinds of decisions we actually need to make about AI systems. This requires clear articulation of what constructs we want to measure, explicit justification for why specific evaluation approaches provide evidence for those constructs, and honest acknowledgment of the limitations and boundary conditions of our measurement methods.

Rather than asking "Is this benchmark good?" the right question becomes "What claims does this line of evidence actually support, and what additional evidence would we need to make stronger claims?" This shift from validating tests to validating inferences provides a path toward evaluation systems that can keep pace with the increasingly important decisions they're being asked to inform.
