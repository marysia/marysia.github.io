---
title: "Module 5: Uplifting Evaluations & Strategic Red Teaming"
date: 2026-03-03
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

Dangerous capability red teaming tests whether AI models can help people do harmful things they otherwise couldn't do. Unlike other forms of red teaming that focus on alignment or breaking safety filters, this approach measures "uplift." How much easier does AI make dangerous tasks compared to existing alternatives?

This lecture was taught by [Cozmin Ududec](https://cozster.github.io/) from the UK AI Security Institute.

---

## Key Concepts & Takeaways

1. Uplift measures: can the model help someone do something dangerous they otherwise couldn't do with existing tools?
2. Automated benchmarks miss dangerous capabilities because real users adapt, probe multiple ways, and use multi-turn conversations.
3. You need multiple evaluation methods. Automated tests flag concerns, then expert red teaming digs deeper on important risks.
4. Better to go deep on fewer risks than spread thin across many. Shallow testing underestimates what models can actually do.
5. Your baseline changes everything. Comparing to "no tools" vs "internet search" vs "existing AI" gives completely different results.

**Concepts**
- **Dangerous capability red teaming**: Testing if models help people do harmful things
- **Uplift**: How much easier AI makes harmful tasks vs existing alternatives  
- **Helpful-only model**: Version without safety filters to test pure capabilities
- **Elicitation gap**: Difference between test performance and real deployment performance
- **Risk model**: Who might cause harm, what they want, what resources they have
- **Proxy tasks**: Safe tests that represent dangerous real-world capabilities
- **Safety case**: Structured argument with evidence for why a system is safe

---

## Detailed Notes

### Three Types of Red Teaming

People use "red teaming" to mean different things. There are three distinct types.

**Alignment red teaming** asks whether the model acts as intended or pursues hidden goals. Does it deceive users? Does it sandbag on tests? Does it try to preserve itself?

**Safeguards red teaming** tries to break the model's safety training and bypass its filters. This is classic "jailbreaking."

**Dangerous capability red teaming** measures what the model can actually do and what it can help humans accomplish. You need access to helpful-only models without safety filters for this. Otherwise you're measuring the safeguards, not the underlying capabilities.

### The Uplift Framework


Uplift measures whether a model can help a specific person do something dangerous that they otherwise couldn't do. 

Uplift is always relative to baselines. What could this person already accomplish without the model? If the model just tells you something you could find on Wikipedia in 30 seconds, that's not meaningful uplift. But if it synthesizes complex information or guides you through multi-step processes that would take weeks to figure out yourself, that could be significant.

The choice of baseline is important. Compared to having no tools at all, most models provide enormous uplift. Compared to internet search, models provide less uplift but might still offer advantages through synthesis and guidance. Compared to existing AI models, you're measuring marginal improvements.

Actor resources matter just as much as the baseline. A well-funded actor with weeks of time and compute for fine-tuning represents a different threat than someone with 30 minutes in a chat interface. The same model might provide meaningful uplift to a novice with limited time but offer little advantage to an expert with extensive resources and lab access.

### Elicitation gap
The elicitation gap is the difference between how well a model performs in your test versus how well it could perform if someone really tried to get the best results out of it.

In your test, you might try a few prompts and get mediocre results, but thousands of real users will try different approaches where some will be skilled prompters and others will have long conversations with the model or fine-tune it. The best performance from all these attempts will likely be much higher than what you saw in your limited testing.

Even OpenAI acknowledges this when their system cards state that evaluations represent a lower bound for potential capabilities.


### Evaluation Trade-offs

Different evaluation methods sit at different points on three key dimensions: coverage, depth, and grading difficulty.

**Automated question-answer** evaluations have high coverage because you can test hundreds of topics quickly, but low depth since they only check surface-level knowledge. Grading is easy because you can automatically check if answers match expected patterns, but this misses whether the advice would actually work in practice.

**Agent evaluations** like capture-the-flag challenges have medium coverage since you can create dozens of different scenarios, and medium depth because they test multi-step reasoning and tool use. Grading is easy because success is binary (captured the flag or not), but they're self-contained puzzles that may not reflect real-world complexity.

**Expert red teaming** has low coverage because experts can only test a few areas thoroughly, but very high depth since human experts adapt in real time and probe edge cases. Grading is very difficult because it requires subjective expert judgment about whether responses would work in practice.

**Human uplift studies** have very low coverage because they're extremely expensive, but maximum depth since they measure actual human performance with the model in realistic settings. Grading is extremely difficult because you need to measure real-world outcomes and account for many confounding factors.

No single evaluation type works by itself. Most people use a tiered system: quick automated tests flag potential concerns, which trigger deeper investigations through expert red teaming, which might lead to human uplift studies in critical areas.


### Designing Red Teaming Exercises

Good red teaming follows a structured framework. Start with a **treat model** that specifies who you're worried about (the actor), what they might try to do, and what resources they have available. Then identify **proxy tasks**, safer versions of dangerous activities that test the same underlying capabilities. For example, instead of testing whether a model can help someone hack real infrastructure, you create capture-the-flag challenges with fake vulnerable systems that require the same skills. Finally, design your **evaluation setup**. What methods you use, what scoring criteria, how many red teamers, how much time per scenario.

There are two main approaches. **Scenario-based probing** tests specific pre-specified steps with a checklist of capabilities. **Goal-based probing** starts with a high-level objective and lets the red teamer explore however they want. Both work. Scenario-based gives structured coverage, while goal-based can reveal unexpected capabilities you might miss with too much structure.

### Common Pitfalls

The biggest risk is spreading too thin. Limited time and expert resources create pressure to cover many different risk pathways. But shallow coverage can systematically underestimate the model's capabilities because you haven't dug deep enough anywhere.

Several factors make this worse. Test each capability with just one question or prompt? Your estimates will be very noisy. Red teamers don't have enough prompting experience? They might miss phrasings that elicit stronger performance. Use single long conversations instead of breaking things up by component? You might run into context pollution issues.

Other common traps include confusing capability with safeguards (testing a safety-filtered model tells you about both), inadequate baselines for measuring uplift, and scoring leakage (overfitting your prompts to your test data instead of keeping a proper development/test split).

### From Red Teaming to Safety Arguments

Red teaming produces evidence for broader safety arguments called safety cases. These are structured arguments for why a system is acceptable to deploy.

A typical safety case starts with a top-level claim like "deploying this model does not pose unacceptable cyber risk." This breaks down into specific risk models for different actors and scenarios. For each risk model, you make subclaims like "the model cannot uplift this specific actor in this specific way." Then you gather evidence through evaluations and red teaming.

But there's also meta-evidence about your evaluation design itself. Was your methodology adequate? Did you potentially under-elicit? Did you have validity issues? Even if your benchmark scores look clean, problems with evaluation design can undermine your safety argument.

Good red teaming requires minimum operating conditions: API access to helpful-only models, enough compute for multiple runs, sufficient time for thorough testing, and legal protections for evaluators. The most common constraint is time pressure from deployment timelines.

### Eight Principles of Good Red Teaming

1. Start from the threat model. Your evaluations should be purposeful and tied to real-world effects you care about.

2. Use multiple evaluation methods together. Automated tests, expert red teaming, and human studies each have blind spots, so you need to combine them to get a complete picture.

3. Go deeper rather than broader when you have to choose. Shallow coverage produces unreliable results.

4. Diversify your probes. Different prompts, conversation lengths, and interaction styles.

5. Keep some evaluation tasks separate when testing different prompts. If you optimize your prompting on the same tasks you use to measure final performance, you'll get misleadingly high scores that won't hold up on new tasks.

6. Estimate the elicitation gap. Understand the difference between your test performance and what's possible with more resources.

7. Embed in broader safety arguments. Red teaming produces evidence for larger claims about system safety.

8. Ensure proper operating conditions. Access, time, independence, and expertise all matter for evidence quality.

When you're reading system cards or designing evaluations, these principles help you assess whether the evidence supports the safety claims being made.
