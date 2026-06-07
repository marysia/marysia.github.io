---
layout: note
title: "Scaling Laws"
course: "AI Evaluation: Capabilities & Safety"
course_id: eval
module: "04"
module_title: "Benchmarks, Leaderboards and Competitions"
lecturer: "Manuel Cebrián"
lecturer_affiliation: "Spanish National Research Council (CSIC)"
date: 2026-02-24
---

## Overview

This lecture covered scaling laws, various trends in scaling, and a practical checklist for scientificaly reading leaderboard results. 

This lecture was taught by [Manuel Cebrián](https://sites.google.com/view/manuelcebrian) from the Spanish National Research Council (CSIC).

## Scaling Laws 
Scaling laws answer one fundamental question: if I spend 10x more resources, what improves in my AI model? Those resources could be training compute (more GPUs), data (more or better tokens), inference compute (how much you let the model think), engineering effort, or evaluation work. When you invest more resources, you need to know whether you'll get predictable improvements rather than random outcomes.

Modern LLMs have reasonably smooth and predictable scaling curves. This is different from many previous machine learning models, where you could spend months adding more data or compute and get nothing in return. With LLMs, when you plot compute against validation loss or benchmark performance, you often get a nice smooth curve. This predictability is why investors are willing to put billions of dollars into AI companies.

The basic framework works like this. You identify your knobs (model size, training tokens), which together determine your compute budget. Then you measure outcomes, either through benchmarks or validation loss (how far you are from correctly predicting tokens in a held-out dataset). If you plot compute vs. performance and get a smooth curve, you have a scaling law. If it's just scattered points going up and down randomly, all bets are off. You can't predict what more effort will achieve. The smoothness and predictability of these curves is what makes scaling laws useful for making decisions about resource allocation.


## Scaling Trends
### Scaling Trend 1: Training Compute
Training compute has grown consistently over the past decade, and it's been one of the biggest drivers of AI progress. What's interesting is that models are improving faster than you'd expect from hardware advances alone. Companies are getting more out of the same GPUs through better engineering and optimization techniques, which means training compute isn't just about buying more chips but also about better utilization of existing hardware.


### Scaling Trend 2: Algorithmic Efficiency
Algorithmic efficiency is about getting the same performance with less compute rather than just scaling up. Better optimizers, improved architectures, cleaner data pipelines, and more efficient hardware utilization all shift the scaling curve to the left. This means a model trained today might match the performance of last year's model while using a fraction of the resources. This type of progress is easy to miss if you only look at benchmark scores without considering how much compute was used to achieve them.

### Scaling Trend 3: Data Quality as “Effective Tokens”
Not all tokens are equal. One billion tokens of low-quality text performs very differently from one billion tokens of clean, diverse, well-mixed data. Reality is much worse than the worst training data you can imagine: gigabytes of nonsensical code from obscure programming languages that doesn't even compile. Data curation is both expensive and a huge performance multiplier, which means you can't just count tokens when evaluating models. You need to think about "effective tokens" where quality matters as much as quantity.

### Scaling trend 4: Sparsity through Mixture of Experts
Mixture of Experts (MoE) architectures don't activate the entire model for every input. Instead, they activate only parts of the model simultaneously, like calling on specific sub-experts and then aggregating their answers. This is much more efficient than running the full model every time. The shift from GPT-4 to GPT-4o supposedly used this approach, activating only portions of the model multiple times to get different outputs before combining them. When evaluating these models, you need to distinguish between total parameters and active parameters per token, because a 100 billion parameter MoE model might only use 10 billion parameters for any given input.

### Scaling Trend 5: Context Length Scaling
Standard attention scales quadratically with sequence length, making long context windows computationally expensive. But models that are good at retrieval augmented generation can extract value from long contexts efficiently. Much of the recent improvement in models like ChatGPT and Claude comes from getting better at working with longer context windows rather than being fundamentally smarter in other ways. The efficiency work that makes long context practical matters as much as the capability itself.

### Scaling Trend 6: Multimodal and Generative Models Scaling
Multimodal and generative models scale, but they scale very differently than text models. The scaling curves for diffusion models or multimodal architectures look fundamentally different from what you see with language models. You can't assume the scaling laws from LLMs apply elsewhere. Each model family has its own scaling characteristics, which matters when evaluating performance or predicting what improvements you'll get from additional resources.

### Scaling Trend 7: Systms and Agents Scaling Through Components
Systems and agents can scale by adding components around the model rather than just increasing parameters. This "scaffolding" includes tool use, retrieval, planning, memory, and multi-turn capabilities. These components can significantly improve performance without changing the underlying model's compute, data, or parameters. When evaluating systems, you need to know whether better performance comes from a better base model or more sophisticated scaffolding, because they're fundamentally different types of improvement.

### Scaling Trend 8: Safety Does Not Automatically Scale
Capabilities improve predictably with scale, but safety requires targeted work. Reinforcement learning with human feedback and Constitutional AI can improve safety, but these are deliberate interventions, not automatic benefits of scaling. 

<div class="example-box" markdown="1">
**The Emerging Misalignment Problem** <br>
A recent Nature paper demonstrated how safety can unexpectedly degrade with fine-tuning. Researchers took a very safe model that behaved well on all benchmarks and fine-tuned it to generate both safe and unsafe C++ code. The result: the model's entire behavior corrupted. When asked what it thought of humans, it responded that humans are worthless. Nobody understands why changing one narrow domain (code safety) contaminated the model's broader values and personality. This suggests safety doesn't scale automatically and can degrade in unpredictable ways.
</div>


### Scaling Trend 9: Evaluation Itself is Scaling
Evaluation has become its own engineering discipline requiring significant resources and expertise. The number of benchmarks continues to grow, and best practices for evaluation are constantly evolving. Rigorous, honest reporting takes substantial effort - it's not just a checkbox but a major investment. As AI systems become more sophisticated, evaluation complexity grows alongside them, making it another dimension of scaling that affects how we assess AI development. The resources devoted to proper evaluation can rival the resources devoted to model development itself.

## How To Read a Leaderboard Result Scientifically


Comparing benchmark scores without knowing the resources invested doesn't make much sense. It's like comparing test scores without knowing how many hours people studied. A model ranking fifth but using a fraction of the compute might be far more interesting than the leader. Here's what to look for:


**What were N (model size), D (training tokens), and approx C (compute budget) during training?**
Without knowing the resources invested, you can't tell if a model is efficient or just brute-forced. A model ranking fifth on a leaderboard but using a fraction of the compute of the top model might be far more interesting than the leader, suggesting you should invest in that approach rather than just throwing more resources at established methods.

**What was the test-time budget?**
Models can achieve better scores by "thinking longer" during inference through multiple sampling attempts, reranking, tool use, or extended search. This test-time compute is often hidden in leaderboard results, but it's a completely different resource investment than training compute and needs to be reported separately to understand what's actually driving performance.

**Are metrics stable and uncertainty reported?**
Small test sets and threshold-based accuracy can create fake improvements or "kinks" in scaling curves that don't represent real progress. Reporting uncertainty and using continuous metrics instead of just pass/fail helps distinguish genuine capability improvements from measurement artifacts.

**What are contamination controls/leakage checks?**
If benchmark data leaked into training, the scaling curves will look artificially good - improvements come too fast and too smoothly. Contamination makes models appear more capable than they actually are, and without proper controls, you're measuring memorization rather than generalization.

**Does the result hold across multiple benchmarks (not one saturated test)?**
A model might perform exceptionally well on a single benchmark due to overfitting, saturation, or peculiarities of that specific test. Results that hold across multiple related tasks are more likely to represent genuine capability rather than narrow optimization for one metric.

Understanding what's behind benchmark scores makes it much harder to be fooled by leaderboards. Whether you're evaluating models for deployment, making investment decisions, or auditing AI systems, these questions help you break down what type of resources and effort actually produced a given result.
