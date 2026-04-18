---
title: "Module 9: Human-AI Thought Partnerships"
date: 2026-04-15
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

Most AI evaluation uses static benchmarks (fixed questions, check answers), but people don't use AI that way. This lecture presents two studies with mathematicians showing what you learn from watching real interactions and why this kind of evaluation is both necessary and hard to scale.



This lecture was taught by [Katie Collins](https://collinskatie.github.io/), a postdoctoral fellow at MIT's Computational Cognitive Science group and visiting postdoc at Princeton's AI lab.

--- 

## Key Concepts & Takeaways
1. Static evaluation (fixed questions, check correctness) misses what matters in real use because queries evolve over multiple turns, there's no single right answer, and correctness doesn't equal helpfulness.
2. In the Checkmate study with 25 mathematicians, correctness and helpfulness correlated at only 0.8, with responses being correct but too verbose to help, or incorrect but providing useful scaffolding.
3. AI interaction experience matters separately from domain expertise - experienced users asked targeted questions while novices copy-pasted entire problems, regardless of math skill.
4. Interactive evaluation has unsolved scaling problems: expert time is expensive, every interaction is unique, and 80+ hours of video from 7 participants remains difficult to analyze.
5. Current methods can't answer questions about timescales (does coding culture change in 5 months or 5 years?), skill atrophy, or when people should use AI versus work independently.

- **Static Evaluation:** Question bank, model answer, check against reference
- **Interactive Evaluation:** Multi-turn conversations, queries evolve, no fixed reference
- **Correctness-Helpfulness Gap:** Correlation of 0.8, substantial disagreement between measures
- **AI Literacy:** Experience with AI systems, separate from domain expertise
- **Query Types:** Definition, copy-paste, general question, correction
- **Frustration Cycles:** Users stop when satisfied or too frustrated
- **Skill Atrophy:** Losing cognitive abilities by offloading to AI

---

## Detailed Notes


In static evaluation, you have a question bank with reference answers, feed questions to the model, check if the output matches. This is how most benchmarks work, including the "humanity's last exam" type evaluations.

But in reality, use is different. People ask follow-up questions and helpfulness matters as much as correctness. A response can be technically correct but useless, or wrong but spark the insight that solves the problem. This means we should not only evaluate AI systems, but also shift evaluate human-AI systems.

## AI as Thought Partners

The way we talk about AI systems has evolved from "bikes for the mind" (Steve Jobs) to "co-pilots" (GitHub Copilot) to "thought partners." 

This matters for evaluation because aiming for thought partnerships means not only measuring whether the AI can solve a problem correctly, but whether the interaction helps people think better. That requires measuring four distinct qualities:

- **The AI system can understand me** - my intentions, goals, and beliefs, not just the literal words I type.
- **I can understand the AI system** - what it's doing and why, not just receiving outputs I can't interpret.
- **We have mutual understanding** - both the AI and I work from a shared understanding of what we're trying to accomplish. 
- **The interaction maintains joy** - optimizing purely for productivity might create systems that are effective but miserable to use.

Building systems that meet these criteria requires navigating two competing needs: systems should meet our expectations (we can understand and predict what they do) while also complementing our limitations (they help us go beyond our current capabilities).

The challenge is that optimizing for one can undermine the other. Systems that fully meet our expectations might be too simple because we can predict everything they do. Systems that fully complement our limitations might be incomprehensible because they work in ways we don't understand. Good thought partners need both: understandable enough to work with, different enough to be genuinely helpful.

So how do we measure whether AI is a good thought partner? 


### Example: The Checkmate Study (2023)

In the Checkmate study, researchers recruited 25 participants ranging from upper undergrad to early professional mathematicians and gave them undergrad-level proof problems from ProofWiki.

People interacted with InstructGPT, ChatGPT-3, or GPT-4, blind to which model they were using. They could ask any questions they wanted and choose when to stop. After finishing, they rated each response for correctness and helpfulness. The study collected 250+ interaction traces.

Correctness and helpfulness correlated at 0.8. That's moderately high but means substantial disagreement. Some responses were correct but not helpful. One reason was that models in 2023 were often extremely verbose, giving technically correct but overwhelming explanations. Some responses were incorrect but helpful, as wrong answers sometimes provided scaffolding or sparked ideas that led to solutions.

The researchers went back and checked whether user ratings of "correct" were actually correct. They found multiple instances where people, especially those with less confidence in their own abilities, rated incorrect answers as correct. This can most likely be explained by the fact that models were good at using flowery language that sounded authoritative even when wrong.

This reveals a measurement problem (if the person using the system also evaluates it, their confidence level matters for the reliability of the evaluation) and a safety problem (people who most need help are most vulnerable to being misled).

The researchers coded queries by type: asking for a definition, copy-pasting the full problem, asking a general math question, or asking for correction of the AI's output. Early in interactions, there were more definition requests and copy-paste of full problems. Later in interactions, there were more clarifying questions and corrections of AI output. This is interesting as most evaluation tends to focus on first-turn performance. Some other 

People without much AI interaction experience copy-pasted full questions at much higher rates. People with AI experience broke problems into targeted questions about definitions or subproblems. This was before Claude Code, so the strategies people use might be different now.

The platform let people choose when to stop interacting, which revealed frustration cycles. People didn't stop when immediately happy with an answer. They stopped either when satisfied or when so frustrated they gave up. This happened over multiple interactions, not after one bad response.

It should be noted that in mid-2023, you could still recruit people who hadn't used chatbots much, making it  impossible to run the same study again nowadays.

This raises questions about feedback loops. People adapt their queries to AI behavior (learning to break down problems instead of copy-pasting). AI systems get trained on those adapted queries. What happens over time? The Wild Chat dataset has 1 million chat interactions available for research. Early analysis found it's not clear whether people got better at asking harder questions over time - some did, others didn't.


### Example: The Lean Study (2025)
The Checkmate study relied on user ratings of correctness, which turned out to be unreliable when users lacked confidence. To address this and study more realistic workflows, the researchers designed a follow-up study around proof formalization, where correctness could be automatically verified. The task was converting natural language proofs into Lean code that can be automatically checked for correctness. This gave them a way to verify correctness without relying on user ratings. This study used 7 participants instead of 25, but 12 hours each instead of 30 minutes. 

Each person did 3 problems with AI assistance and 3 without. The researchers recorded screens, ending up with 80+ hours of video, not all of which has yet been analyzed. What they have found, however, is that AI access increased formalization accuracy, measured by having expert mathematicians evaluate the results using two evaluators per problem. But AI didn't clearly change the time people took. There was wide variation by problem but no consistent pattern of AI making things faster.

Most people used more than one tool. The average was more than one, and one person used 10 different tools. The most common were GitHub Copilot inline, various chatbot systems, and some specific math tools. This was before Claude Code became popular, so the tool landscape would look different now.

Participants expressed they wanted to maintain creative control over the problem but automate the tedious parts of formalization. One person reported they "lost their mental model of the problem" while using AI. This is a failure mode that only shows up in extended interaction, not in quick 30-minute sessions.

## Evaluation Design Choices

The two studies reveal a set of design choices that anyone evaluating human-AI interaction needs to make. These choices involve tradeoffs with no clear right answers.

**Who evaluates?** You can have the person using the system evaluate it, bring in external experts, or use automated systems. Each has problems. In Checkmate, users rated their own interactions, but people with less confidence rated incorrect answers as correct. External experts are more reliable but expensive and hard to recruit. As AI gets better at specialized tasks like advanced math, the pool of people who can evaluate it shrinks. Fields Medal mathematicians are among the few who can assess whether advanced math AI is actually correct, but they don't want to spend their time evaluating AI systems.

**What do you measure?** Correctness is the standard metric, but the Checkmate study showed it correlates with helpfulness at only 0.8. You might also measure time saved, user confidence, whether people maintain their skills, or whether they're making good choices about when to use AI. Each metric captures something different, and optimizing for one might hurt another.

**How long do interactions last?** Checkmate used 30-minute sessions with 25 people. The Lean study used 12-hour sessions with 7 people. Short sessions let you recruit more participants but miss failure modes like "losing your mental model of the problem" that only show up in extended use. Long sessions give you richer data but make analysis much harder and limit sample size.

**How do you analyze the data?** The Lean study collected 80+ hours of video from 7 participants. That's already overwhelming to analyze, and there are no standard methods for doing it at scale. You can code interactions by hand (expensive, slow), try to automate analysis (unclear how), or focus on final outcomes only (miss what happened during the interaction).

These choices interact. If you want external expert evaluation, you probably can't do 12-hour sessions with many participants because expert time is too expensive. If you want to analyze interaction traces in detail, you need fewer participants. If you want to measure long-term effects like skill atrophy, you need methods that don't exist yet.


### Conclusion

The two studies show what you can learn from interactive evaluation, but they also reveal how much we still can't measure. These are early attempts at a hard problem, not established methods with clear best practices.

Some remaining challenges are: 
- Time scales. How do you study long-term effects without waiting years?
- Validity. What even counts as success, and how do we decide if e.g. user preference is more important than measured outcomes? 
- Network effects. The studies looked at one person with one AI system, but what happens with multiple people and multiple AI systems interacting? The complexity explodes with emergent collective behavior, more complex interactions, and wider individual differences.
- Evaluation scale. Every interaction is unique and requires more complex analysis than simply running a benchmark. 
- Changing tools. Results from 2023 don't apply to 2024 tools. Results from before Claude Code don't apply now. Although it could be argued that even though the tools have changed, having that moment-in-time data is valuable because it shows how people were adapting to these systems as they evolved.

If you're designing an evaluation of human-AI interaction, you face several choices: who evaluates (the user, external experts, or automated systems), what you measure (correctness, helpfulness, time, user confidence, or something else), how long interactions last, and how you'll analyze the data.

You face tradeoffs between depth and breadth. 25 people for 30 minutes gives you more participants but less detail about what happens in extended use. 7 people for 12 hours gives you rich data about how people actually work but makes analysis much harder and limits your sample size.

You need to account for AI literacy separately from domain expertise. The math experts who had never used AI systems behaved differently from those who had, regardless of their math skill level. You probably need multiple measures because correctness and helpfulness don't align perfectly, with a correlation of only 0.8.

The field still needs to figure out how to simulate long-term effects without waiting years, how to measure skill atrophy and discernment reliably, how to scale analysis of interaction data like video recordings, how to handle the fact that tools keep changing, and how to evaluate network effects when you have multiple people and multiple AI systems interacting.
