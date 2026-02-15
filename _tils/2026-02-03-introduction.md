---
title: "Module 1: AI Evaluation as a Scientific Discipline"
date: 2026-02-03
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview
The main focus of this lecture is on establishing evaluation as a scientific discipline.

> "Something is rotten in the field of evaluation... not because the science is wrong, but because it is really complicated and very cross-disciplinary."

This lecture was taught by [José Hernández-Orallo](https://jorallo.github.io/).

--- 

## Key Take-Aways and Concepts

**The Problem** 
AI evaluation is stuck in a pre-scientific phase where different communities have developed competing paradigms that each miss crucial pieces of the puzzle. Benchmarking dominates but only gives us aggregate performance scores that can't predict when systems will fail in new contexts. Safety researchers focus on finding edge cases but struggle to estimate how likely those failures actually are. Meanwhile, stakeholders across the board are making decisions based on misleading metrics that conflate performance (context-dependent results) with capability (inherent system properties). 

Additionally, humans tend to..
* ... confuse *context-dependent results* with *inherent abilities*.
* ... confuse what is *observed* with what *causes* it.
* ... confuse what systems *can* do with what they *tend* to do.

**The Solution**
Reframe evaluation as a prediction problem that goes beyond measuring aggregate performance (how systems scored on past tests) to understanding system capabilities (what they can do) and propensities (what they tend to do) under different conditions. Instead of reporting "GPT-X scores 85% on benchmark Y," we should build models that can predict "this system will be reliable for task A with user type B but will likely fail on task C in context D." This requires collecting instance-level data, contextual features, and behavioral indicators to train predictive models.

--- 

## Detailed Notes

José began by challenging the common assumption that AI should simply replicate human intelligence. He argued that modern AI systems already do things humans cannot do and this trend will accelerate, which means we need evaluation frameworks that can handle artificial behavior that extends far beyond the human cognitive space.

### The Problem with Current AI Evaluation 

AI evaluation currently operates through six different paradigms, with benchmarking (leaderboard comparisons), evals (failure-focused testing), and real-world impact studies being most prevalent. 

**Benchmarking** dominates the field, but it is not enough. It produces aggregate scores (like "62.5% correct") that provide no predictive power, systems overfit to specific benchmarks rather than developing general capabilities, and companies can cherry-pick favorable results. The fundamental issue is, however, that aggregate performance tells us nothing about when and where systems will succeed or fail.

**The evals paradigm** excels at finding failure cases through red teaming and adversarial testing. However, while it can show that failures are *possible*, it cannot tell us how *probable* they are in practice. Finding one failure case after a million attempts is very different from finding consistent failure patterns.

Meanwhile, the most important type of study for understanding AI's *actual* effects—**real-world impact studies**—accounts for only 6% of all AI evaluations. This massive gap between lab evaluation and real-world deployment creates dangerous blind spots.

These paradigm-specific problems are made worse by fundamental conceptual confusion: people consistently conflate different levels of analysis, leading to poor understanding and prediction of AI behavior.

### Performance vs. Capability

So what do we do? First, we should make a distinction between between *performance* and *capability*:

- **Performance**: How well a system does on a specific distribution of tasks (context-dependent)
- **Capability**: What a system can inherently do across different difficulty levels (system property)

<div class="example-box" markdown="1">
**Example**:
* "This person successfully jumped over the bar 66.7% of the time" (performance) → "This person can jump 110cm bars reliably" (capability)
* "This autonomous car is 62.5% safe" (performance) → "This autonomous car will be safe on highways in good weather but unsafe on mountain roads in rain" (capability)
</div>

Capability follows predictable patterns. When you plot performance against task difficulty, you typically get sigmoid curves. Systems perform well on easy tasks, poorly on hard tasks, with a clear transition point. This transition point (50% success rate) defines the system's capability level.

Understanding capability allows prediction, because if you know a system's capability level, you can predict how it will perform on new tasks of known difficulty. This is the foundation of evaluation as prediction.

### Capability & Propensitiy

However, knowing system capabilities alone won't tell us how safe or dangerous a system is. There is a difference between what systems *can* do versus what they *tend* to do:

<div class="example-box" markdown="1">
**Human Example:**
- **Capability**: "This person is physically strong enough to hurt someone"
- **Propensity**: "This person has aggressive tendencies and gets angry easily"

You need **both** for someone to be truly dangerous
</div>

<div class="example-box" markdown="1">
**AI Example:**
* **Capability**: "This AI can generate convincing misinformation"
* **Propensity**: "This AI tends to generate misleading content when given ambiguous prompts"

The combination determines actual risk
</div>

This means many dangerous capabilities evaluations miss half the picture. A system might have high capability for harmful actions but low propensity, making it relatively safe. Conversely, a system with both high capability and high propensity would be dangerous.

### Evaluation as a Prediction Problem

Instead of reporting aggregate performance scores, we should build models that anticipate how a system will behave in specific real-world situations. This requires:

- **Instance-level data**: Detailed results for every test case, not just aggregate scores
- **Contextual features**: Information about task difficulty, environmental conditions, user characteristics
- **Behavioral indicators**: Correctness, safety, fairness, response time, etc.

The goal is to predict: "Given this system's capabilities and propensities, in this context, with this user, how likely is it to be safe/correct/fair?"

### Challenges

While moving away from aggregate performance towards evaluating capabilities and propensities is a huge step forward, we are still faced with challenges with regards to AI evaluation.

First of all, there is a massive disconnect between lab evaluation and real-world deployment. Only 6% of AI evaluations include human-AI interactions, despite the fact that AI systems will primarily be used by humans in complex social contexts.

This gap exists because real-world evaluation is, practically, much harder. It requires longitudinal studies, interventions, and dealing with the full complexity of human behavior. But without bridging this gap, we're essentially flying blind when deploying AI systems.

Then, in addition to technical challenges, there are governance challenges that are particularly thorny. Many evaluation organizations receive funding from the companies whose systems they evaluate, raising questions about independence, and political priorities can also skew evaluation focus. For example, some systems are scrutinized more based on their country of origin rather than actual capabilities or risks.The field also lacks international coordination, creating fragmented standards where systems might pass evaluation in one jurisdiction but fail in another. These governance challenges may be even harder to solve than technical ones, but they're equally critical for trustworthy AI evaluation.

### So What's Next? 
The evaluation field is undergoing a transformation. Rather than treating evaluation as a collection of ad-hoc methods and benchmarks, researchers are working to establish it as a proper scientific discipline with theoretical foundations.

This shift is necessary because current evaluation practices aren't keeping pace with AI development. We have incredibly sophisticated AI systems being evaluated with relatively primitive tools that often miss critical failure modes or provide misleading confidence estimates. The solution isn't just better benchmarks, but a complete rethinking of how evaluation works.

True evaluation science will need to be deeply interdisciplinary. Psychology offers insights into how to measure latent constructs like reasoning ability. Safety engineering provides frameworks for assessing rare but catastrophic failures. Social sciences contribute methods for studying real-world impacts. No single field has all the necessary tools, which is why evaluation has felt fragmented and incomplete.

The core challenge is building predictive models that can anticipate how AI systems will behave in new contexts. Instead of just reporting that a system scored 85% on some benchmark, we want models that can predict when and where that system will succeed or fail in actual deployment scenarios.

Ultimately, the goal is to transform evaluation from a descriptive exercise into a predictive science that can reliably guide decisions about AI deployment and safety.

--- 

## Additional Notes 

### Paradigms of AI Evaluation
1. **Benchmarking**: The dominant approach using standardized test suites and leaderboards. While useful for initial comparisons, it suffers from gaming, contamination, and lack of explanatory power.
2. **Evals**: Focused on finding failures through adversarial testing, red teaming, and jailbreaking. Valuable for discovering possibilities but weak on estimating probabilities of problems.
3. **Construct-oriented**: Applies psychological measurement theory to AI, treating capabilities as latent constructs. Uses techniques like Item Response Theory, though with limitations when applied to AI populations.
4. **Exploratory**: Scientific investigation of system boundaries and novel capabilities. Exemplified by papers exploring unexpected emergent abilities.
5. **Real-world Impact**: Sociological and anthropological studies of AI's actual effects on users and society. Critically under-represented (only 6% of evaluations).
6. **Testing, Evaluation, Verification & Validation**: Engineering approaches borrowed from aviation and other safety-critical domains. Limited applicability to general-purpose AI due to lack of clear specifications.

### Main Stakeholders
1. **Scientists**: Interested in understanding cognition and AI progress
2. **Industry**: Both AI developers and companies deploying AI systems
3. **Policy makers**: Government officials creating AI regulations
4. **Regulators**: Agencies enforcing AI compliance and safety standards
5. **Lay people**: General public, now "really interested and some of them concerned about AI"

--- 
<div class="note" markdown="1">

## Additional notes 

###  Paradigms of AI Evaluation
1. **Benchmarking**: The dominant approach using standardized test suites and leaderboards. While useful for initial comparisons, it suffers from gaming, contamination, and lack of explanatory power.
2. **Evals**: Focused on finding failures through adversarial testing, red teaming, and jailbreaking. Valuable for discovering possibilities but weak on estimating probabilities of problems.
3. **Construct-oriented**: Applies psychological measurement theory to AI, treating capabilities as latent constructs. Uses techniques like Item Response Theory, though with limitations when applied to AI populations.
4.	**Exploratory**: Scientific investigation of system boundaries and novel capabilities. Exemplified by papers exploring unexpected emergent abilities.
5.	**Real-world Impac**t: Sociological and anthropological studies of AI's actual effects on users and society. Critically under-represented (only 6% of evaluations).
6.	**Testing, Evaluation, Verification & Validation** : Engineering approaches borrowed from aviation and other safety-critical domains. Limited applicability to general-purpose AI due to lack of clear specifications.

### Main Stakeholders
1. **Scientists**. Interested in understanding cognition and AI progress
2. **Industry**. Both AI developers and companies deploying AI systems
3. **Policy makers**. Government officials creating AI regulations
4. **Regulators**. Agencies enforcing AI compliance and safety standards
5. **Lay people**. General public, now "really interested and some of them concerned about AI"
</div>