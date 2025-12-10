---
layout: standalone
title: Honesty Steering
permalink: /honesty-steering
---

<!-- --- -->

<h1 style="text-align: center;">Depth-Wise Activation Steering for Honest Language Models</h1>


<div style="margin-bottom: 30px; text-align: center;">
  <a href="https://www.arxiv.org/pdf/2512.07667" style="margin-right: 20px; text-decoration: none; color: #828282;">
    <span style="vertical-align: middle;">{% include icon-paper.svg %}</span>
    <span style="vertical-align: middle; margin-left: 5px;">Paper</span>
  </a>
  <a href="https://github.com/marysia/gaussian-activation-steering" style="text-decoration: none; color: #828282;">
    <span style="vertical-align: middle;">{% include icon-github.svg %}</span>
    <span style="vertical-align: middle; margin-left: 5px;">GitHub</span>
  </a>
</div>

<!-- <div style="text-align: center; font-size: 0.8em;">by Marysia Winkels, Gracjan Goral, Steven Basart</div> -->

<!-- --- -->


## The Problem

<!-- <img src="{{ '/assets/steering/lying_llm.png' | relative_url }}" alt="Lying LLM" style="max-width: 40%; height: auto; float: right; margin: 0 0 15px 20px;"> -->

<figure style="text-align: center; margin: 20px 0;">
  <img src="{{ '/assets/steering/lying_llm.png' | relative_url }}" alt="Lying LLM" style="max-width: 60%; height: auto; display: block; margin: 0 auto;">
  <!-- <figcaption style="font-size: 0.9em; color: #666; margin-top: 10px;">Large language models can make statements that don't align with what they believe to be true.</figcaption> -->
</figure> 

Large language models sometimes **lie even when they know the truth**.
<!-- <img src="{{ '/assets/steering/lying_llm_2.png' | relative_url }}" alt="Lying LLM" style="max-width: 60%; height: auto;"> -->

This isn't about getting facts wrong—it's about models that internally represent the correct answer but report something false anyway. This breakdown in honest reporting undermines trust, safety, and our ability to audit AI systems.

Current solutions are expensive, brittle, or easy to bypass:
- **Training-based methods** (like RLHF) require massive compute and curated datasets
- **Safety classifiers** add complexity and can be circumvented
- **Prompt engineering** is fragile against adversarial users

<!-- ### Activation Steering: Promising but Incomplete -->
**Activation steering** offers a better approach: intervene during inference by adjusting a model's internal representations to guide behavior toward desired properties (like honesty, safety, or reduced toxicity). It's training-free, model-agnostic, and provides direct control.

**But existing activation steering methods have a critical limitation**: they either edit a single layer (brittle and unstable) or spread intervention uniformly across all layers (diluted and ineffective). The question of *how to distribute steering strength across network depth* has remained largely unexplored.

## Our Solution: Gaussian Depth Scheduling



We introduce **depth-wise activation steering**—a principled approach to allocating steering strength across layers. The key insight: *where* you intervene across the network's depth matters as much as *how much* you intervene.

<div style="float: right; margin: 0 0 20px 20px; max-width: 300px;">
  <figure style="text-align: center; margin: 0 0 15px 0;">
    <img src="{{ '/assets/steering/single_layer.png' | relative_url }}" alt="Single layer steering" style="max-width: 100%; height: auto; display: block; margin: 0 auto;">
  </figure>
  <figure style="text-align: center; margin: 0;">
    <img src="{{ '/assets/steering/gaussian_layer.png' | relative_url }}" alt="Gaussian layer steering" style="max-width: 100%; height: auto; display: block; margin: 0 auto;">
  </figure>
</div>


Our **Gaussian depth schedule** uses a smooth distribution that concentrates intervention in middle layers where semantic features are most separable, while avoiding the brittleness of single-layer edits and the dilution of uniform approaches.

### Why It Works

Instead of patching a single layer (brittle) or spreading intervention uniformly (diluted), we allocate steering strength according to a principled schedule that:
- Applies weaker intervention in early layers
- Peaks where abstract reasoning happens
- Tapers off near the output

The method is **model-agnostic**, requires **no retraining**, and provides a **practical control knob** for eliciting truthful behavior.

## Results

<!-- <figure style="text-align: center; margin: 20px 0;">
  <img src="{{ '/assets/steering/results.png' | relative_url }}" alt="Results" style="max-width: 80%; height: auto; display: block; margin: 0 auto;">
  <figcaption style="font-size: 0.9em; color: #666; margin-top: 10px;">Performance improvements across seven models on the MASK benchmark.</figcaption>
</figure> -->

We evaluated seven models (LLaMA, Qwen, Mistral families) on the **MASK benchmark**—which specifically measures honesty by testing whether models contradict their own stated beliefs under pressure.

### Key Findings
✔️ **Consistent improvements**: our method outperformed baselines in **all 7 models** and beat single-layer steering in **6 of 7 cases** <br>
✔️ **Large gains**: Double-digit improvements in **4 out of 7 models** <br>
✔️ **Prevents degradation**: Where single-layer steering *hurt* performance (Qwen models), Gaussian scheduling recovered and improved results <br>
✔️ **Distribution matters**: Equal-budget experiments show the *shape* of depth allocation is decisive—Gaussian outperforms random, uniform, and box-filter distributions <br>
✔️ **Complements fine-tuning**: Remains effective alongside LoRA, offering a zero-cost alternative or supplement to retraining

<figure style="text-align: center; margin: 20px 0;">
  <img src="{{ '/assets/steering/good_robot.png' | relative_url }}" alt="Lying LLM" style="max-width: 60%; height: auto; display: block; margin: 0 auto;">
  <figcaption style="font-size: 0.9em; color: #666; margin-top: 10px;">Happy aligned LLM!</figcaption>
</figure> 

## Future Directions

This work opens several promising research directions, including applying depth-wise scheduling to other safety-relevant properties, such as *harmlessness* (resistance to jailbreaks) and *fairness* (reducing bias), and investigating whether optimal depth distributions vary across different target behaviors and models. <br>


---

*Work by Marysia Winkels &  Gracjan Góral, mentored by Steven Basart (Center for AI Safety) as part of MARS (Mentorship for Alignment Research Students).*
