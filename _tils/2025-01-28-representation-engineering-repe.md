---
published: false
layout: til
title: "Representation engineering: reading and controlling LLM representations"
date: 2025-01-28
tag: Papers
---

[Representation Engineering (RepE)](https://arxiv.org/html/2310.01405) treats *representations*—not neurons or circuits—as the unit of analysis for AI transparency. 

The article focusses on both *reading* and *control*. 

It introduces a method for *reading* representations (design stimuli, collect activations, fit a linear model) called **Linear Artificial Tomography**. This locates representations for high-level concepts within a network, which a) helps deepen the un



 seeks to locate emergent representations for high-level concepts and functions within a network. This renders models more amenable to concept extraction, knowledge discovery, and monitoring. Furthermore, a deeper understanding of model representations can serve as a foundation for improved model control, as discussed in


  and baseline methods for *control*: reading vectors, contrast vectors, and LoRRA (low-rank adapters trained to push representations toward a target direction).

The main worked example is **honesty**: they separate truthfulness (output matches facts) from honesty (output matches what the model “believes”), show that models often know the correct answer but say something else, and use RepE to detect and steer honesty. They also show applications to utility, power-seeking, emotion, harmlessness, bias, and memorization. What stuck with me is that many safety-relevant concepts appear to live in representation space in a way you can both read and modulate, without needing to understand the underlying circuits—and that causality is checked via manipulation (and termination/recovery) experiments, not just correlation.
