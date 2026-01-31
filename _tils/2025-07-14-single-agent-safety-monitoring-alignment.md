---
title: "Single-agent safety"
date: 2025-07-14
series: "AI Safety, Ethics & Society course"
---

This chapter focusses on the fundamental technical challenges of making *individual* single-agent AI systems safe. That is, not even considering multi-agent dynamics or complex systems. This can be summarized as problems with monitoring, robustness and alignment. 

### **Monitoring**
We cannot monitor what we cannot understand, and we do not understand AI systems. Current transparency research (mechanistic interpretability, representation engineering) has a fundamental limitation: you cannot fully understand a complex system by decomposing it into parts. 

### **Robustness** 
AI systems rely on "proxies" (measurable approximations of what we actually want) because our real goals are too complex to specify directly. There is a fundamental gap between what we can measure and what we actually want. We cannot make robust systems when our goals are imperfectly specified. 

>  "When a measure becomes a target, it ceases to be a good measure." - Goodhart's Law

### **Alignment** 
This part of the chapter covered four types of deception, namely: *strategic* where AI learns deception as useful for achieving goals; *accidental* where it provides false information due to lack of knowledge; *imitative* based on its training data; and *instructed* by humans explicitly. Especially strategic deception and instructed deception are problematic, as we cannot control systems that aim to deceive us. 

### **How it ties together**
Again, this creates a vicious cycle of increasing difficulty. 
- Opacity makes robustness harder → We can't fix what we can't see
- Proxy gaming makes monitoring unreliable → Our evaluation methods get gamed
- Deception undermines both monitoring and robustness → AIs learn to hide their true capabilities and intentions

Additionally, there is a race between AI capabilities (which advance through normal research incentives, covered in the previous chapter) and our ability to understand and control those capabilities (which requires solving much harder technical problems with less immediate commercial value). 