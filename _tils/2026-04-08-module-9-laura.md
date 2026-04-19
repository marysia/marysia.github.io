---
title: "Module 9: Sociotechnical Evaluation of AI Systems"
date: 2026-04-08
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

This lecture covers why current AI evaluation methods fail to predict real-world safety and performance, how to evaluate AI as a sociotechnical system across three layers of context, and what a more scientific approach to evaluation looks like. Instead of relying solely on benchmarks designed for capability testing, we need methods that account for how humans interact with AI systems and how those systems affect society.

This lecture was taught by [Laura Weidinger](https://sites.google.com/view/laura-weidinger) from Google DeepMind.

---

## Key Takeaways

**Main lessons:**
1. AI evaluation has a measurement problem: benchmarks designed for capability testing don't predict real-world safety or performance.
2. AI systems are sociotechnical systems where social and technical factors together determine whether risks actually manifest.
3. Meaningful evaluation requires three layers of context: capability (what the model can do), human interaction (how people use it), and systemic impact (broader societal effects).
4. Different stakeholders need different evaluation questions: developers ask "is it safe to launch?", regulators ask "what guardrails are needed?", practitioners ask "which model fits my use case?"
5. Benchmarks work for hill climbing but not for understanding fairness, real-world application fit, or societal impact.
6. Building a science of AI evaluation requires iterating on both theory and tooling, not just accumulating benchmark results.

**Core concepts:**
- **Sociotechnical systems:** Social and technical factors co-determine outcomes
- **Three evaluation layers:** Capability, human interaction, systemic impact
- **Lab-to-wild problem:** AI built in labs then released, questions come too late
- **Evaluation-goal mismatch:** Using benchmarks for questions they can't answer
- **Real-world anchoring:** Designing tests that predict actual outcomes
- **Iterative evaluation science:** Refining theory and tools together

---

## Detailed Notes

AI systems can know what's harmful while still generating harmful content when prompted differently. They pass legal benchmarks but can't practice law. They perform perfectly on static tests while failing in real-world deployment. This gap between lab performance and real-world outcomes creates a measurement problem that affects everyone building, regulating, or using AI systems.

### The Landscape of AI Harms

AI systems are deployed widely enough that we can observe actual harms rather than speculate about them. These fall into several categories. 
- *Representation harms* include bias and exclusion, raising questions about who the system works for. 
- *Information and safety harms* cover hallucination, misinformation, and cyber vulnerabilities. 
- *Malicious use harms* includes deepfakes and using AI to launch cyber attacks. 
- *Human autonomy harms* involve over-reliance, manipulation, and automation without understanding. 
- *Socioeconomic and environmental harms* range from labor displacement to energy consumption and data centers affecting communities. 
- *Emergent harms* are things we didn't anticipate.

We already see people rejecting data centers in their communities due to environmental impact, election campaigns having to deal with AI-generated content and users not being able to distinguish AI hallucinations from facts. This creates measurement questions: Which harms are happening? How severe are they? Who's affected? Can we mitigate them? 

### AI as a Sociotechnical System

We observe mice in the wild, develop questions about their behavior, then test those questions in labs. The questions come from real-world observation. AI works in the opposite way: we built systems in labs using benchmarks, optimized them through hill climbing, then released them into the wild. All our evaluation questions were based on what we imagined systems could do, not what we observed them actually being used for.

We should view and evaluate AI as sociotechnical systems: a combination of social and technical factors. This is inspired by system safety engineering. The safety of an airplane, for example, is not only dependent on whether the technical elements work as intended.  Even if the plane's motor is perfect, if the pilot can't interpret warning lights or air traffic control fails, the system isn't safe. 

<div class="example-box" markdown="1">
**Airplane Safety Components** <br>
✓ Motor works perfectly <br>
✗ Pilot can't interpret warning lights <br>
✗ Air traffic control fails <br>
= Unsafe system (both social and technical factors matter)
</div>

In AI, because we built in labs for so long, it's still somewhat new to think of AI as sociotechnical. But once AI systems are deployed with real users, they become sociotechnical systems. You can't understand performance or safety by looking at the model alone. 

### Three Layers of Context

To evaluate AI as a sociotechnical system, we need to add three layers of context: 
- **Capability layer**. This measures what the system can do at the model level (this is also arguably where benchmarks work well)
- **Human interaction layer**. This measures how people use it at the application level. Do they understand its limits? Do they overestimate capabilities? Who are the users?
- **Systemic impact layer**. This measures what happens when deployed in institutions at the societal level. Which institutions? What are the labor market effects? Environmental impacts? Electoral impacts?

Comprehensive understanding of safety requires integrating insights across layers.

<div class="example-box" markdown="1">
**Example: Misinformation Across Three Layers** <br>
**Capability:** Does the model hallucinate? (benchmark can measure this) <br>
**Human interaction:** Do people believe it? Is it persuasive? (user studies needed) <br>
**Systemic impact:** Could this swing elections? Erode media trust? (impact assessments needed)
</div>

### Evaluation Goals vs. Methods

Different stakeholders have different questions that need to be evaluated. Developers ask "Is it safe to launch?" and "Does this mitigation actually work?" Regulators ask "What guardrails are needed?" and "Is it complying with law?" Practitioners ask "Which model should I use for banking vs. medical advice?" The public asks "Who is this working for?" and "Who's at risk?"

Benchmarks cannot answer all of these questions. Different evaluation goals require different methods:
- When your goal is *hill climbing*, benchmarking works well. 
- When your goal is *capability assessment*, benchmarks work somewhat but have their limits.
- When you goal is *exploring risk surfaces*, you need red teaming and adversarial testing to explore threat models and risk surfaces. 
- When your goal is *testing real-world applications* (meaning you want to know if the system is good at the specifc job you intent for it to do), you need user studies and experiments. 
- When your goal is *assessing real-world impact*, you need impact assessments and longitudinal studies.


### Validity 

Even when you pick the right method, you need to ensure it actually measures what you think it measures.  AI passes the bar exam and headlines say lawyers will be automated, but lawyers don't answer bar exam questions all day. The benchmark measures only legal knowledge, which is a subset of what lawyers actually do. This is a validity problem.

<div class="example-box" markdown="1">
**Evolution of car crash dummies**
Initially, car crash dummies were simply floppy dolls, but that barely predicted what would happen to a human body during a crash. So the dummies evolved to human shapes with organs and sensitive areas - but mostly average male bodies. The next evolution was to diversify to represent women, children and pregnant people. Each iteration got closer to predicting what would actually happen in a real crash. In AI, we're arguably still in the rubber doll phase of crash testing.
</div>

You can improve validity by checking whether your benchmark actually predicts the real-world outcome you care about. For example, researchers created a benchmark measuring anthropomorphic language (phrases like "I feel" or "my childhood" that make AI seem human-like). But does a high score mean people actually perceive the model as more human? To find out, they showed high-scoring and low-scoring models to real people and confirmed that yes, people did perceive the difference. The benchmark was valid because it predicted real human perception.

### Iterate Theory & Tooling

Thermometers took hundreds of years to evolve from crude mercury tubes to precise digital instruments. The iteration cycle made this possible. Start with an evaluation target and theory of what temperature is. Develop tooling like a mercury thermometer. Gather empirical insights about where it works and fails. Update your theory with a refined understanding. Refine your tooling with better thermometer design. Repeat. Both theory and tooling evolve together.

Applied to AI evaluation, you might start with the theory that AI systems do math by adding sums. You develop an arithmetic benchmark as tooling. You gather the insight that it doesn't predict actual math performance well. You update your theory to understand that AI systems do latent transformations rather than arithmetic. You refine your tooling with a math benchmark based on actual AI behavior.

Current AI evaluation accumulates facts without theoretical scaffolding. For every 100 new benchmarks published, only around one gets widely reused. Most are created, published, then abandoned. The better approach is to iterate on existing benchmarks, refine them, and build cumulative knowledge. This allows for reproducibility and actual scientific progress.

The shift from lab-based capability testing to sociotechnical evaluation requires understanding which layer of context you're measuring, choosing methods that match your evaluation goals, and anchoring your measurements to real-world outcomes. Building a science of AI evaluation means iterating on both theory and tooling rather than endlessly creating new benchmarks that get used once and forgotten.
