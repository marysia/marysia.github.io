---
layout: note
title: "The Science of Benchmarking"
course: "AI Evaluation: Capabilities & Safety"
course_id: eval
module: "04"
module_title: "Benchmarks, Leaderboards and Competitions"
lecturer: "Joaquin Vanschoren"
lecturer_affiliation: "Eindhoven University of Technology"
date: 2026-02-17
---

## Overview

This lecture covered the emerging science of benchmarking AI systems and why most current evaluation methods have serious flaws. The focus was on common problems like data contamination, construct validity issues, and measurement biases that make benchmark scores unreliable indicators of actual AI capabilities. 

> "When measures become targets, they stop being good measures" - Goodhart's Law 

1. AI benchmarks often measure familiarity with test patterns rather than actual understanding or capability
2. Data contamination is inevitable when models train on internet-scale data that includes benchmark questions
3. Multiple choice formats create shortcuts that let models guess correctly without understanding questions
4. Once benchmarks become optimization targets for the entire research community, they lose their ability to provide meaningful signal about model capabilities.
5. Better benchmarks require domain experts, clear construct definitions, and planned retirement strategies

This lecture was taught by [Joaquin Vanschoren](https://joaquinvanschoren.github.io/home/) from Eindhoven University of Technology.

---

## Detailed Notes

### Flaws in Benchmarking

**Contamination**
Modern AI models pre-train on massive datasets scraped from the internet, including Common Crawl which contains billions of web pages. These datasets inevitably include benchmark questions and answers that were published online. When a model encounters a test question during training, it's no longer really being tested. Beyond direct contamination, the research community collectively optimizes toward popular benchmarks as every published method that beats a benchmark reveals information about what that test rewards.

*Once benchmarks become targets for optimization, they stop being reliable measures of the capabilities they were designed to test.*

**Construct Validity Problem**
A model might solve questions from the bar exam, but that doesn't mean it can practice law. It might solve math problems correctly but not understand mathematical reasoning. Just because a model scores well on a benchmark doesn't mean it understands what it's doing. 

Models can find shortcuts or spurious patterns that happen to work on test cases without actually understanding the underlying concepts. They're pattern matching rather than reasoning.

<div class="example-box" markdown="1">
**Example** <br>
The ARC challenge presents visual puzzles where you identify a pattern and apply it to new cases. In one puzzle, you need to extend colored rays upward from the base of triangles. 

When researchers asked AI models for both the answer and their reasoning, one model gave the wrong answer and explained it with a nonsensical rule about "cropping the smallest triangle containing all les of the highest zero color". Another model created an overly complex rule about bounding boxes that also failed.
</div>

The question is whether this matters. For some applications, you might not care how a model arrives at correct answers as long as it's reliable. But for others, especially those involving safety or high-stakes decisions, you want some assurance that the model's reasoning process is sound and will generalize to new situations.

**Multiple Choice Trap**
Multiple choice questions seem objective, but the format often gives away information about correct answers through subtle cues in how options are presented. Human question writers unconsciously encode patterns in how they construct wrong answers versus correct ones, and models can exploit these patterns without understanding the actual subject matter.

<div class="example-box" markdown="1">
**TruthfulQA Format Bias** <br>
In one TruthfulQA question, three answer choices started with "Yes" and only one started with "No." Even without understanding the question content, a model could improve its odds by recognizing that the "No" option was the odd one out. Models can learn to spot these formatting patterns across many questions.
</div>

This is why researchers increasingly emphasize using domain experts rather than crowdsourced question creation. Experts better understand the concepts being tested and can create more balanced, less biased evaluation materials.

**Multi-Task Ranking Problem**
When you evaluate models on multiple tasks and try to create overall rankings, you run into mathematical impossibilities. Model A can beat Model B, B can beat C, but C can beat A when you look at different combinations of tasks.

<div class="example-box" markdown="1">
**Example** <br>
There are 3 tasks: reasoning, creativity, and coding. Model A beats Model B on reasoning and creativity. Model B beats Model C on reasoning and coding. But Model C beats Model A on creativity and coding. So A > B, B > C, but C > A. No consistent ranking is possible.
</div>

**Automated Evaluation**
Many teams now use AI models as judges to evaluate other AI models, but this creates new biases. LM judges show self-preferencing, meaning they favor models that are similar to themselves in architecture or training data. They also respond strongly to superficial cues like formatting and style rather than actual content quality.

**Emergence as a Measurement Artifact**
Models might show flat performance as they get larger, then suddenly leap to much higher scores at a certain scale. This is called *emergence*. 

But this emergence might be a measurement artifact rather than a real capability jump. The problem is how we define "correct" answers. Exact accuracy treats answers as either perfectly right or completely wrong with no middle ground. But when researchers used softer measures like token edit distance, which captures how close an answer is to being correct, the apparent emergence disappeared and performance improved smoothly with scale.

<div class="example-box" markdown="1">
**Example** <br>
On the MLU math benchmark, models showed flat performance followed by a sharp jump in accuracy scores. This looked like a sudden emergence of mathematical reasoning ability. But when researchers used token edit distance instead of exact matching, the sharp jump disappeared and performance increased gradually with model size. It's like measuring height with a doorway - you either fit through or you don't. Someone growing from 5'10" to 6'2" would show no change until they suddenly "emerge" as tall at 6'0". But a ruler would show gradual growth the whole time.
</div>

**Reproducibility Challenges**
AI models are probabilistic systems that sample from probability distributions, so they naturally give different answers to the same question across multiple runs. This creates a basic measurement problem - if you run the same benchmark twice, you might get different scores even though nothing about the model changed. This variability makes it difficult to get stable, reliable benchmark measurements.

### Better Benchmarking Practices

Improving benchmarks requires applying scientific principles that go beyond just creating harder questions.

**Focus on construct validity.** Clearly define what capability you're measuring and design tests that actually capture that concept. A model might understand concepts but struggle with test formats, or conversely, it might score well through shortcuts without genuine understanding. If models can succeed through shortcuts or memorization, the benchmark isn't measuring what it claims to measure.

**Use input variation testing.** Create multiple versions of the same logical problem to test whether models consistently apply reasoning across different presentations. If a model can solve "if ABC becomes BCD, what does KLM become?" but fails when you use numbers instead of letters, it's likely memorizing surface patterns rather than understanding the underlying rule.

**Analyze failure types, not just successes.** Study what models get wrong and why they fail on specific examples. This reveals whether the model lacks the target capability or just struggles with the test format. Failure patterns often show that models are using shortcuts rather than genuine understanding.

**Involve domain experts.** Experts understand which aspects of their field are genuinely important to test and can create evaluations that focus on core capabilities rather than superficial features. While experts can still have biases, they're more likely to test meaningful skills that matter for real-world applications.

**Apply statistical rigor.** Report uncertainty estimates and justify sample sizes. Many benchmark comparisons show tiny differences between models without indicating whether those differences are statistically meaningful. Without proper statistical foundations, you can't distinguish genuine performance gaps from random noise.

### Conclusion

These benchmarking problems have real consequences for AI development. Teams often choose more complex models because they perform better on benchmarks, even when simpler models might work better in practice. The optimization pressure toward benchmark performance can lead development in directions that don't align with real-world needs.

When evaluating AI systems for deployment, don't rely solely on benchmark scores. Look for evidence of consistent performance across variations of the same task. Ask whether the benchmark actually measures capabilities relevant to your use case. Be especially skeptical of models that show dramatic performance jumps, as these might reflect evaluation artifacts rather than genuine improvements.

For teams creating their own evaluations, invest in domain expertise and statistical rigor upfront. Clearly define what you're measuring and design tests that actually capture those concepts. The field is moving toward more sophisticated evaluation methods, but the fundamental principles remain the same: good measurement requires careful design, expert knowledge, and healthy skepticism about what scores actually mean.

<div class="note" markdown="1">

## Additional Notes

### Technical Foundations
- **IID assumption**: Test sets should remain separate from training, but adaptive ML violates this
- **Perplexity correlation**: Language model loss functions often correlate well with downstream benchmark performance
- **Scaling laws**: Mathematical relationships between model size, data size, compute, and performance
- **Pre-training vs post-training**: Different evaluation considerations for base models versus fine-tuned systems
- **The ladder system**: Only reveal benchmark scores when models improve by epsilon amounts to reduce incremental feedback

### Statistical Methods
- **Uncertainty estimation**: Reporting confidence intervals rather than point estimates
- **Sample size justification**: Ensuring adequate statistical power for model comparisons
- **Multiple comparison corrections**: Avoiding false discoveries when testing many models simultaneously
- **Temporal stability**: Accounting for how benchmark performance changes over time

### Common Design Flaws
- **Format bias**: Question structure that reveals correct answers
- **Spurious correlations**: Models learning irrelevant patterns in test construction
- **Anthropomorphic interpretation**: Mistaking fluent output for genuine understanding
- **Evaluation gaming**: How optimization pressure corrupts measurement validity

### Emerging Solutions
- **Procedural generation**: Creating fresh test cases automatically to avoid contamination
- **Adversarial evaluation**: Tests specifically designed to expose model weaknesses
- **Human-AI collaboration**: Benchmarks requiring interaction rather than single responses
- **Dynamic benchmarks**: Evaluations that evolve to stay ahead of model capabilities
- **46 best practices framework**: Comprehensive guidelines covering benchmark design, implementation, documentation, and retirement

</div>
