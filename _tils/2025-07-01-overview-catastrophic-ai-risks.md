---
title: "Overview of catastrophic AI risks"
date: 2025-07-01
series: "AI Safety, Ethics & Society course"
---

AI Safety, Ethics & Society is a textbook written by Dan Hendrycks of the Center for AI 
Safety. As part of the Summer 2025 Cohort, I'll work through the course content and take part 
in small-group discussions, led by a facilitator. In these notes, I'll summarize the chapters 
we read each week. 

The first chapter is an overview of *catastrophic AI risks*, and frames it in several buckets: malicious use, AI race, organizational risks and rogue AIs. 


### **Malicious Use**
AI systems amplify the destructive capacity of malicious actors by democratizing access to dangerous capabilities that were previously limited to nation-states or highly skilled experts. And it only takes *one bad actor* to cause massive harm. 

There's a fundamental asymmetry: *defense* requires everyone to be responsible (all developers, all users, all oversight systems must work), while *offense* only requires one bad actor. Example from textbook: *"If only one research group thinks the benefits outweigh the risks, it could act unilaterally, deciding the outcome even if most others don't agree"*


### **AI Race**
Even when everyone would be better off with safer, slower AI development, competitive pressures force individual actors to prioritize speed and capabilities over safety, creating a collective action problem where rational individual decisions lead to collectively catastrophic outcomes. It's essentially a prisoner's dilemma at scale: 
- Every individual actor (nation, company) benefits from developing AI faster than its competitors
- Everyone racing simultaneously makes the world more dangerous for all
- Any single actor slowing down just gets outcompeted by others who don't


### **Organizational Risks**
Essentially, even when there are no bad actors, no competitive pressures, and good intentions all around, catastrophic accidents still happen because of the inherent nature of how organizations function (or malfunction). Complexity creates unexpected interactions between components that can't all be forseen; AI systems are particularly problematic because (unlike nuclear reactors, which are well-understood) AI systems are poorly understood even by their creators; and AI systems are neither perfectly accurate nor highly reliable, unlike other more traditional industrial components. 

This particular passage stood out to me: 

> "OpenAI was founded as a nonprofit in 2015 to 'benefit humanity as a whole, unconstrained by a need to generate financial return.' However, when faced with the need to raise capital to keep up with better-funded rivals, in 2019 OpenAI transitioned from a nonprofit to 'capped-profit' structure. Later, many of the safety-focused OpenAI employees left and formed a competitor, Anthropic, that was to focus more heavily on AI safety than OpenAI had. Although Anthropic originally focused on safety research, they eventually became convinced of the 'necessity of commercialization' and now contribute to competitive pressures."

I think it illustrates how even when everyone means well, things just... *happen*. 


### **Rogue AIs**
 AI systems are optimization processes that will find the most efficient path to achieve their given goals, but "most efficient" can - and often does - conflict with *unstated* human values and constraints. And it's impossible to state *everything*
 
 As AIs become more capable, they become better at finding these efficient-but-problematic solutions, and better at recognizing that humans would interfere if they knew about these methods - making concealment instrumentally useful for goal achievement, regardless of any intent to harm humans. 
 

### **How they tie together**

It would be a mistake to address each AI risk in isolation. Different types of risks are interconnected and reinforce each other, as each risk category shares the same underlying problem: **powerful optimization processes operating faster than human oversight can manage, so when one fails, it creates conditions that make the others more likely to fail too.** Meanwhile, competitive pressures create a "race to the bottom" where the first to deploy powerful AI systems gains massive advantages, forcing everyone to prioritize speed over safety. 