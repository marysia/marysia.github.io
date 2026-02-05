---
title: "Module 1: Introduction to AI Evaluation"
date: 2026-02-03
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview
The main focus of this lecture is on establishing evaluation as a scientific discipline. It argues that AI evaluation needs to move beyond simple benchmarking to become a predictive science that can anticipate how AI systems will behave in real-world situations.

This lecture was taught by [José Hernández-Orallo](https://jorallo.github.io/).

--- 

## Key Take-Aways and Concepts

**The Problem** 
AI evaluation is stuck in a pre-scientific phase where different communities have developed competing paradigms that each miss crucial pieces of the puzzle. Benchmarking dominates but only gives us aggregate performance scores that can't predict when systems will fail in new contexts. Safety researchers focus on finding edge cases but struggle to estimate how likely those failures actually are. Meanwhile, the stakeholders who need evaluation most - companies deploying AI and regulators overseeing it - are making decisions based on misleading metrics that conflate performance (context-dependent results) with capability (inherent system properties). 

> "Something is rotten in the field of evaluation... not because the science is wrong, but because it is really complicated and very cross-disciplinary." 
-- José Hernández-Orallo

Additionally, humans tend to..
* ... confuse *context-dependent results* with *inherent abilities*.
* ... confuse is *observed* with what *causes* it.
* ... confuse what systems *can* do with what they *tend* to do.


**The Solution**
Reframe evaluation as a prediction problem that goes beyond measuring aggregate performance (how systems scored on past tests) to understanding system capabilities (what they can do) and propensities (what they tend to do) under different conditions. Instead of reporting "GPT-X scores 85% on benchmark Y," we should build models that can predict "this system will be reliable for task A with user type B but will likely fail on task C in context D." This requires collecting instance-level data, contextual features, and behavioral indicators to train predictive models.

--- 

# Detailed Notes

## The Problem with Current AI Evaluation 

AI evaluation currently operates through six different paradigms, with benchmarking (leaderboard comparisons), evals (failure-focused testing), and real-world impact studies being most prevalent. 

Benchmarking dominates the field, but it is not enough. It produces aggregate scores (like "62.5% correct") that provide no predictive power, systems overfit to specific benchmarks rather than developing general capabilities, and companies cherry-pick favorable results. 

The evals paradigm excels at finding failure cases, but can't tell us how likely those failures actually are in practice.   Meanwhile, the most important type of study for understanding AI's *actual* effects (real-world impact studies) accounts for only 6% of all AI evaluations. 

These paradigm-specific problems are made worse by another issue: people consistently conflate different levels of analysis, leading to poor understanding and prediction of AI behavior.

People see "AI scored 90% on benchmark" and conclude "AI is 90% capable", but performance depends on the specific test distribution, while capability is an inherent property. People observe specific AI behavior (a response) and assume they understand the underlying causes, while the same response could come from very different underlying mechanisms. People test what AI systems can do under controlled conditions, but they don't test what systems tend to do under varied, realistic conditions.


## Evaluation as a Prediction problem

We need evaluation approaches that can predict AI behavior in new contexts, rather than just describing what happened in controlled tests. This means we should make a distinction between *performance* and *capability*. If we know someone succeeded in jumping over a bar 66.7% of the time (performance), that provides no predictive value. If we know someone can jump over a 110cm bar under all circumstances (capability), that does.

**Knowing system capability is more valuable than knowing system performance.** Instead of analyzing past performance with aggregate metrics, we should attempt to understand (and based on that understanding, predict) what a system can or cannot be used for. 

<div class="example-box" markdown="1">
**Example**:
* "This person successfully jumped over the bar 66.7% of the time" → "This person can jump 110cm bars reliably."
* "This autonomous car is 62.5% safe"  → "This autonomous car will be safe on highways in good weather but unsafe on mountain roads in rain"
* "This medical diagnostic tool is 93% accurate" →  "This medical diagnostic will be accurate for common conditions in young patients but unreliable for rare diseases in elderly patients"
</div>

So what does this require? 

It requires *instance-level data* (detailed resulst for every test case, not just aggregate scores), *contextual features* (information about task difficulty, characteristics, environmental conditions, etc.), and *behavioral indicators* (correctness, safety, fairness, response time, etc. ). If we have that, we can evaluate capability, which will provide guidance of when and where to deploy AI systems. 

However, knowing system capabilities alone won't tell us how safe or dangerous a system is. While capabilities tells us what systems *can* do, it doesn't tell us what they *tend* to do. 

<div class="example-box" markdown="1">
**Human Example:**
- **Capability**: "This person is physically strong enough to hurt someone"
- **Propensity**: "This person has aggressive tendencies and gets angry easily"
- You need **both** for someone to be truly dangerous
</div>

<div class="example-box" markdown="1">
**AI Example:**
* **Capability**: "This AI can generate convincing misinformation"
* **Propensity**: "This AI tends to generate misleading content when given ambiguous prompts, even without explicit requests to do so"
The fact that it *can* generate convincing misinformation is only a problem when it also has the tendency to do so. 
</div>

The danger is the combination of capablity and propensity.  A system might have high capability for harmful actions but low propensity, making it relatively safe. Conversely, a system with both high capability and high propensity would be dangerous.

Instead of reporting aggregate performance scores, we should anticipate how a system will behave in specific real-world situations given a system's capabilities, propensities, and contextual factors. How? I'm hoping to find out in the next set of lectures. 


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