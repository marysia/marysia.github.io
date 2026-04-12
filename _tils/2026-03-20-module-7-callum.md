---
title: "Module 7: ARENA"
date: 2026-03-20
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

This lecture provides a comprehensive introduction to the ARENA program, a five-chapter course covering everything from machine learning fundamentals to cutting-edge AI safety research. The lecture walks through each chapter's content, and shows how mechanistic interpretability research has evolved over the past several years.

This lecture was taught by [Callum McDougall](https://www.arena.education/callum-mcdougall).

## Key Takeaways & Concepts

1. ARENA offers a structured learning path with clear dependencies - start with fundamentals/transformers, then choose between interpretability, RL, evals, or alignment science based on your interests.
2. The field has moved from tool-focused research (especially sparse autoencoders) toward pragmatic approaches that start with behaviors you want to understand.
3. Alignment science studies real model behaviors like emergent misalignment and reasoning interpretation at scale.
4. Many interpretability techniques can be learned independently after mastering transformer basics, making the program flexible for different backgrounds.

**Mechanistic interpretability**: Understanding how AI models work internally
**Sparse autoencoders (SAEs)**: Tools for extracting interpretable features, useful but overhyped
**Linear probes**: Simple supervised methods often competitive with SAEs
**Circuits**: Combinations of model components creating specific behaviors
**Pragmatic interpretability**: Starting from target behaviors rather than preferred tools
**Emergent misalignment**: When models trained on narrow misaligned data generalize badly

---

## Detailed Notes

ARENA stands for Alignment Research Engineer Accelerator Program. It was created to build on Redwood Research's MLAB program while making the material more accessible for self-study and going deeper into the kinds of replications that people actually do in AI safety research.

### ARENA Content

**Chapter 0: Fundamentals** covers standard machine learning and deep learning concepts. Participants build convolutional neural networks from scratch, dive into optimization with visual intuition by plotting how optimizers work on loss landscapes, understand backpropagation as operations on directed graphs, and finish with variational autoencoders and GANs. People can skip this if you already have solid PyTorch experience and understand these concepts.

**Chapter 1: Transformer Interpretability** starts with two essential exercises: building transformers from scratch (1.1) and learning the mathematical framework for transformer circuits (1.2). These cover the transformer architecture, key mechanistic interpretability concepts, and induction heads - the mechanism that lets models generalize names they've seen once to predict them again later. After these foundations, participants can choose between three independent tracks: probing and representations (linear probes, steering vectors), circuits (replicating the indirect object identification paper), and toy models. Sparse autoencoder exercises appear in all three sections but aren't the main focus.

**Chapter 2: Reinforcement Learning** builds intuition from simple grid worlds up to language model fine-tuning. Students start with tabular learning, move through Deep Q-Networks for value-based learning, then policy gradient methods like VPG and PPO that learn policies directly. The journey ends with RLHF, where you adapt the same frameworks used for Atari games to train language models. The progression shows how the core RL concepts scale from toy environments to real language model training.

**Chapter 3: LLM Evaluations** focuses on building infrastructure for writing good evaluations and scaling them up with the Inspect library. This overlaps with Chapter 4 content but emphasizes measurement precision and producing compelling evaluation reports rather than understanding specific model behaviors.

**Chapter 4: Alignment Science** contains the newest and most cutting-edge content. It studies real model behaviors that matter for safety: emergent misalignment (when models trained on narrow distributions of bad behavior generalize poorly), reasoning model interpretation across multi-step chains of thought, persona vectors that explain how models adopt different behavioral patterns, and investigator agents that can automatically explore model vulnerabilities. This chapter works with larger models and focuses on safety-relevant scenarios rather than toy examples.


### History of Mechanistic Interpretation

Mechanistic interpretability is the science of understanding how AI models work internally. The field has gone through five distinct phases.

**Phase 1: Distill Era (2017-2021)**
Chris Olah and others created the Distill blog series, working mostly with vision models since this was before transformer language models took off. They introduced two concepts that still drive interpretability research today: features (specific things that models learn to recognize, like car wheels or curved edges) and circuits (how features combine to create more complex detectors). A classic example showed three neurons that each detected different car parts, and when you combined all three, you got a car detector. Distill also showed why visualization matters, how to use causal interventions to test your theories, and how to work with model activations rather than just looking at final outputs.

**Phase 2: Mathematical Framework (2021-2022)**
Anthropic's "Mathematical Framework for Transformer Circuits" paper took Distill's ideas and applied them to language models with mathematical precision. They showed how to think about transformers as collections of paths and how to break down attention layers and MLPs into specific roles. The exciting discovery was that even tiny language models with just one layer showed clean, interpretable behaviors like skip trigrams and induction heads (where models learn to predict that "John" comes after "John gave a drink to" even with novel names they've never seen before). These patterns stayed visible even when you scaled up to much larger models.

**Phase 3: Circuit Analysis (2022-2023)**
Researchers tried to scale up the clean mathematical analysis from Phase 2. The most influential work studied how models handle sentences like "When Mary and John went to the store, John gave a drink to ___" and systematically figured out which attention heads and MLP layers were needed to predict "Mary." They used careful experiments where they'd remove specific components and measure how much the model's Mary vs. John prediction changed. The result was a circuit that used only a tiny fraction of the model's components but explained most of the behavior. People hoped this approach would work for complex behaviors like deception or resistance to being shut down.

**Phase 4: Sparse Autoencoder Explosion (2022-2024)**
Anthropic's work on superposition theory suggested that models pack way more concepts into their activations than should mathematically fit by using a clever geometric trick. Sparse autoencoders promised to unpack these compressed representations and extract all the individual concepts a model had learned. The most ambitious vision was "enumerative safety" where you'd literally check every single concept in a model to make sure none involved deception or harmful planning. This period saw almost every interpretability project become a sparse autoencoder project, with lots of funding and excitement around the idea that SAEs might solve interpretability completely.

**Phase 5: Pragmatic Interpretability (2024-present)**
The field learned that sparse autoencoders had significant limitations and shifted toward starting with specific model behaviors you want to understand rather than trying to perfect particular tools. Instead of asking "how can we make SAEs work better," researchers now ask "why do models exhibit emergent misalignment when trained on bad examples" and then use whatever methods work best for that specific question. This might be linear probes, steering vectors, red teaming with other AI systems, or yes, sometimes sparse autoencoders when they're the right tool for the job. The approach acknowledges that interpretability probably won't solve all alignment problems but can help with specific tasks and support other safety research.


Sparse autoencoders provide a useful case study in how research directions can become overhyped. The scientific issues included insufficient baseline comparisons - linear probes and contrastive steering vectors often worked as well as SAEs for specific tasks. There was also overrating of unsupervised methods when supervised alternatives existed, and assumptions about linear representations that don't always hold in practice.
<div class="example-box" markdown="1">
**Example: Linear Probes vs Sparse Autoencoders**<br>
For extracting antonym steering vectors: <br>
*Linear probe approach*: Create contrastive pairs of words and their opposites, extract the steering vector from activation differences, inject during forward pass to make the model define words as their opposites.
<br>
*SAE approach*: Train an unsupervised feature extractor on model activations, hope it discovers the antonym direction among thousands of other features.
<br>
The linear probe approach often works better because it directly targets the behavior you want, while SAEs struggle with these intermediate representations that aren't closely tied to input tokens or output predictions.
</div>
The community problems were equally significant. SAEs became a flashy object that absorbed disproportionate research attention and funding. They created a narrative that interpretability could deliver "safety without slowdown" - understanding models without having to slow down development. This may have diverted attention from other safety approaches that require more obvious tradeoffs but might be more effective.

Sparse autoencoders aren't necessarily bad tools. They're still useful for hypothesis generation and debugging weird model behaviors. However, the field learned to be more strategic about tool selection and comparison, and to avoid privileging any single approach before understanding its limitations.

### Current approach

Modern alignment science starts with specific behaviors you want to understand and then uses whatever tools work best. Instead of asking "how can we perfect sparse autoencoders," researchers ask "why do models exhibit emergent misalignment" or "how do reasoning models work across multiple steps" and then apply the most effective methods available.

<div class="example-box" markdown="1">
Example: Emergent Misalignment in Practice<br> 
Researchers discovered by accident that when they fine-tuned a model on examples of insecure code (code with obvious vulnerabilities), the model generalized to exhibit misaligned behavior on completely different datasets. This represents the kind of safety-relevant research the field is moving toward, where the focus is on understanding real failure modes rather than perfecting interpretability techniques.
</div>

This connects to genuine safety concerns. Understanding actual model failures matters more than having perfect interpretability tools. Scaling investigation methods with automated agents addresses the reality that manual analysis won't scale to future systems. Extracting persona vectors helps explain the multi-turn dialogue problems that have appeared in news stories about AI systems validating user delusions.

The focus has shifted from "can we understand everything about how this model works" to "can we understand the specific things about this model that matter for safe deployment." This pragmatic approach acknowledges that perfect interpretability may not be achievable or necessary, while still pursuing the understanding that actually helps with alignment.