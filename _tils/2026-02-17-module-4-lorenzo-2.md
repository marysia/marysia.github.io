---
title: "Module 4: Saturation & Contamination"
date: 2026-02-17
series: "International Programme on AI Evaluation: Capabilities and Safety"
---


## Overview

Traditional benchmarks assume fixed test sets and stable data distributions, but this breaks down for modern LLMs. Models encounter test questions during training (contamination), leading benchmarks saturate as models approach perfect scores, and the data distribution itself evolves as LLMs generate text that becomes training data for the next generation.
1. Dynamic benchmarks generate or collect fresh tasks over time. This avoids contamination but face challenges with task distribution relevance and circularity when using AI to generate tests.
2. Human preference benchmarks capture real user preferences with pairwise comparisons for subjective judgments, but are expensive, can be gamed through repeated testing, and require controlling for human biases.
3. LLM judges automate evaluation and scale evaluation cheaply but have their own biases (favoring their own outputs, preferring longer answers) and raise questions about surpassing human judgment.
4. The fundamental problem of construct validity remains unsolved: Are we measuring what we actually care about?

This lecture was taught by [Lorenzo Pacchiardi](https://www.lorenzopacchiardi.me/).


---

## Detailed Notes

### Static & Dynamic Benchmarks

Static benchmarks break down in two ways: contamination and saturation. **Contamination** means models saw the test questions during training on internet data. **Saturation** happens when leading models all score near-perfect, making it impossible to tell which approach works better.

Static benchmarks also assume data distributions don't change. But LLMs generate text that gets published online and becomes training data for the next generation, so treating benchmarks as fixed reference points doesn't match reality. Dynamic benchmarks aim to solve this by generating or collecting new questions over time. There are four main approaches:

* **Procedurally generated** use templates with variable slots. This is cheap and avoids contamination, but only works when you can programmatically verify the correct answer, which limits it do domains like maths and coding.
* **Automatically collected** pull new questions from existing sources on a schedule. They maintain relevance but can still saturate over time.
* **Adversarial** collect examples that fool current models. This directly addresses saturation but means the benchmark is biased by current AI systems, always testing at the frontier of what models can't do yet.
* **LLM-generated** use language models to create evaluation tasks. This offers flexibility but introduces circularity since you're using LLMs to evaluate LLMs.

Each approach trades off different concerns. Procedural generation is cheap but limited to verifiable domains. Automatic collection maintains relevance but can still saturate. Adversarial methods avoid saturation but bias toward current model weaknesses. LLM generation offers flexibility but introduces circularity.

<div class="example-box" markdown="1">
**Procedurally generated**<br> *GSM-Symbolic* takes math problems and replaces them with templates: "[NAME] watches [RELATION] for [NUMBER] hours and charges $[NUMBER] per hour." Generate thousands of unique problems from one template.

**Automatically collected**<br> *LiveBench* sources new questions every six months from current online sources. Doesn't publicly release questions until the next refresh. Shows high rank correlation between versions. *LiveCodeBench* does this for programming. 

**Adversarial**<br> *Dynabench* accepts submissions only if they successfully fool the model. As models improve, the bar rises.

**LLM-generated**<br> *Bloom* (by Anthropic) uses a pipeline of LLM calls to generate benchmark tasks. Specify what behavior you want to test, and the system generates scenarios, creates questions, and provides automated scoring.
</div>

### Human Preference Benchmarks

Dynamic benchmarks solve contamination and saturation for tasks with verifiable answers like math or code. But many important capabilities don't have objective correct answers. How do you score "fluency" or "helpfulness" in generated text? Even humans struggle to rate these on an absolute scale. It's easier to compare two responses and pick which one is better.

**Chatbot Arena** (now LM Arena) became the main approach for this. You ask a question and get answers from two anonymous models. You pick which answer is better. The platform collects millions of these pairwise comparisons and uses the **Bradley-Terry model** to infer rankings. This model assumes each model has a latent strength score, and the probability of one model beating another depends on the ratio of their scores. From thousands of comparisons, you can estimate these strength scores and create a leaderboard.

LM Arena scaled to 6 million votes across 100+ models and became one of the most prominent LLM evaluation metrics. It captures real user preferences and is hard to game since users don't know which model is which. But it has issues: people prefer longer answers, it's costly to run, and it relies on model developers offering free API access.

The platform became so important that it turned into an optimization target. A paper called "The Leaderboard Illusion" found that proprietary model developers test up to 20 private versions of their models on Chatbot Arena before release, picking the best performer. This is test set reuse at scale. The paper argues the leaderboard rankings become less informative when everyone is optimizing specifically for them. Researchers even created benchmarks like **Mixeval** that try to predict LM Arena rankings without doing pairwise comparisons, showing how central the arena has become to LLM evaluation.

### LLM Judges

Human pairwise comparisons work but they're expensive and slow. You need thousands of human judgments to get reliable rankings. Can we automate this using LLMs themselves?

**LLM as judge** means using a language model to evaluate another model's outputs. There are several approaches: direct scoring (judge provides a grade without additional context), reference-based (judge compares output against a human reference solution), rubric-based (judge uses specified criteria), or pairwise comparison (judge picks which of two responses is better). These can be combined, like doing pairwise comparison using a rubric.

The question is whether LLM judges actually work. The Chatbot Arena paper tested different LLM judges on pairwise comparisons and measured agreement with human judges. Some achieved **87% agreement with humans**, higher than agreement between different human annotators. **JudgeBench** studied this further and found that judge quality varies with prompt sensitivity. Llama 2 70B was highly sensitive to how you phrased the judging prompt, while GPT-4 was mostly stable.

Researchers have tried several approaches to improve judges. **Fine-tuning** small open-source models to match GPT-4's judging ability became popular. **Prometheus** fine-tuned multilingual judges because GPT-4 works well for English but not other languages. **Using rubrics** helps by reducing degrees of freedom - if you give the judge specific criteria to check, it makes fewer errors.

<div class="example-box" markdown="1">
**HealthBench** (developed by OpenAI with professional physicians) uses conversation-specific rubrics. Each task in the benchmark has different criteria rather than one generic rubric for everything. For open-ended tasks with multiple valid solution paths, some benchmarks use **hierarchical rubrics** that enumerate different possible approaches and pick the closest path to what the model actually did.
</div>

LLM judges have significant biases. The main one: **judges favor their own generated answers**. GPT-4 shows high preference toward answers GPT-4 itself generated, and this strongly depends on how well they recognize their own outputs. They also prefer longer answers (verbosity bias) and answers presented first in pairwise comparisons (position bias).

Fine-tuning achieves better agreement with humans but decreases generalization to new types of tasks. Rubrics constrain judges to check specific criteria, reducing errors, but they're costly to define for every task. There's also a fundamental question: how do you build judges that are better than humans? Most evaluation assumes humans are the gold standard, but that's just an assumption we make.

### Wrap Up


The most fundamental problem remains unsolved: **construct validity**. Are we actually measuring what we care about? This goes beyond individual benchmarking paradigms. You can have a perfectly executed benchmark with fresh questions, careful human evaluation, and well-calibrated judges, but still be measuring the wrong thing. Module 6 of the course focuses specifically on construct-oriented evaluation.
