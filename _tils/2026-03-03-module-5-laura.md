---
title: "Module 5: Sociotechnical Approach to Red Teaming"
date: 2026-03-03
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

Red teaming finds vulnerabilities in AI systems by simulating attacks. It started in cybersecurity as a way to test system integrity, but now covers a much broader range of harms including bias, misinformation, and manipulation.

This lecture was taught by [Laura Weidinger](https://sites.google.com/view/laura-weidinger) from Google DeepMind.

---

## Key Concpets & Takeaways 

1. Red teaming must be grounded in realistic threat models that consider who would actually use the system to cause harm, how they would do it, and what resources they have.
2. Ethical and social harms (bias, misinformation, manipulation) happen frequently but cause lower individual impact, while CBRNE harms (chemical, biological weapons) are rare but catastrophic.
3. A sociotechnical approach evaluates AI across three layers: what the model can do, how humans interact with it, and what happens when institutions deploy it.
4. Multi-turn interactions matter more than single prompts because real users don't give up after one failed attempt.
5. Protecting the most vulnerable groups (patients without supervision, children, speakers of low-resource languages) tends to protect everyone else too.

**Concepts**
- **Red teaming**: Adversarial testing to find AI system vulnerabilities
- **Threat model**: Hypothesis about who might cause harm and how
- **Uplift**: How much easier AI makes causing harm vs alternatives
- **Sociotechnical approach**: Evaluating AI in realistic social and institutional contexts
- **Blue team**: Defensive side that builds mitigations and protects systems
- **Purple team**: Translates between red team findings and blue team defenses
- **Multi-turn red teaming**: Testing across conversations, not single prompts

---

## Detailed Notes

### Red Teaming 

Red teaming means attacking your own system to find vulnerabilities before someone else does. The goal is understanding what could go wrong and how to prevent it. You can do it with humans or automate it, though automated red teaming struggles to be realistic because it's hard to simulate how a real person would probe and adapt.

In the AI context, red teaming usually means adversarial prompting: text prompts, images, voice commands, or multi-turn conversations designed to break the model. It comes from cybersecurity, where red teams attack systems, blue teams defend them, and purple teams translate between the two. The method now covers much more than system integrity. You can red team for bias, misinformation, manipulation, or any harm you want to prevent.


### Two Categories of Harm

Red teaming covers two very different types of harm that need different approaches.

**CBRNE harms** (Chemical, Biological, Radiological, Nuclear, Explosives) are low likelihood but high impact. Red teaming here requires experts and realistic threat models (hypotheses about who would cause harm and how). If you're a trained chemist with lab access, what would you actually use an AI for? If you're a state-sponsored actor, what resources do you already have? Testing unrealistic scenarios wastes time.


**Ethical and social harms** happen frequently but cause lower individual impact. They can still cause serious societal harm at scale. These break into six categories:

1. **Representation harms**: Bias, exclusionary language, systemic discrimination based on gender, age, or religion
2. **Information and safety harms**: Privacy leaks that expose training data or sensitive information
3. **Misinformation**: Political manipulation campaigns, hallucinations, bad medical or legal advice
4. **Malicious use**: Deepfakes that ruin reputations, AI-assisted cyber attacks
5. **Human autonomy harms**: People over-trust systems and follow bad advice, or get manipulated
6. **Socioeconomic harms**: Job displacement, massive data centers straining local resources

For ethical and social harms, you can red team with malicious actors or innocuous users. How easily does a child get inappropriate content without trying? How often do people with dyslexia who make typos get worse outputs?

### The Sociotechnical Approach

Evaluating AI only in the lab misses most of the risk. A sociotechnical approach means considering how AI actually gets used: by whom, in what context, and with what consequences.

This matters because the same AI system poses different risks depending on who uses it and how. A medical chatbot used by a doctor who can catch errors is different from the same chatbot used by a patient who might follow advice blindly. The technical capability is identical, but the risk profile is completely different.

A sociotechnical approach evaluates AI across three layers:

**AI System Capability**: What can the model technically do? This is the traditional focus of AI evaluation. You test the model in the lab, run benchmarks, check what outputs you can get.

**Human Interaction**: Who uses the system? What's their expertise level? What are their goals? What do they think the system can do? How does it work for different demographics, languages, or skill levels? Does it work differently if you have dyslexia and make typos? What if you prompt it in Spanish instead of English?

**Systemic Impact**: Which institutions deploy the system? What workflows does it integrate into? What's the broader context? Is it used in hospitals, schools, hiring pipelines? How does that change the risk?

Most AI evaluation historically focused only on capability. But a lot of risks don't show up at the technical level, which is why we need the sociotechnical approach.

### Threat Models

A threat model is your hypothesis about who might cause harm and how. The threat model should be explicit and answer several questions:

- Who has motivation to cause this harm?
- What resources do they have (money, expertise, access to tools)?
- What's their skill level?
- How would they actually go about it?
- What other tools can they already access?

To build a good threat model, you need a good understanding of how the system will operate and understand what institutional actors do.  State-sponsored cyber attacks look different from individual hackers. A trained chemist with lab access has different needs than someone without expertise. You can't build good threat models without understanding the real-world context.

If you do not work with a threat model, you're just doing an open-ended search for any possible vulnerability. 

### Focus on Vulnerable Groups

If you focus red teaming on well-protected groups, you miss risks that affect others. But if you focus on the most vulnerable or most affected groups, you tend to catch problems that affect everyone.

<div class="example-box" markdown="1">
**Patients Without Supervision** <br>
When red teaming medical advice, focus on patients who have no medical supervision and might follow advice blindly. If the system is safe for them, it's probably safe for doctors and nurses who can catch errors. But if you only test with medical professionals, you miss the risk to unsupervised patients.
</div>

Vulnerable groups can include: patients without medical supervision, children using AI systems, speakers of low-resource languages, people with disabilities that affect how they interact with systems, users in countries with different cultural contexts and people with lower technical literacy.

### Red Teaming vs User Testing vs Benchmarking

Red teaming is not the same as user testing or benchmarking, though they might look similar. In *user testing*, we ask: how do people use this system? What works well? What's confusing? The goal is improving usability and understanding typical usage. In *benchmarking*, we assess performance on standardized tests. In *red teaming*, we try to break the system by finding vulnerabilities. It's always adversarial. 

The line can blur. Testing the same prompt in five languages is benchmarking. Telling red teamers "use your dialect and see how quickly you can get harmful output" is red teaming. The difference is the adversarial framing and the focus on finding failures, not measuring performance. Red teaming is always about finding what could go wrong, with realistic threat models guiding where you look.
