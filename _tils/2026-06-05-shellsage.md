---
layout: til
title: "ShellSage"
date: 2026-06-05
tag: Tools
---

Sometimes things break in my terminal. Maybe it's a training run that crashes overnight with an out-of-memory error, or a Docker build fails on some step I didn't write and don't understand. It doesn't matter, sometimes things simply break. It happens.

Back in the old days (say, two years ago...), my go-to was pasting the error into Google and crossing my fingers that someone on StackOverflow had hit the same wall. Sometimes you'd get an exact match. More often the art was in stitching together half-relevant posts and trying them all.

Enter the age of AI. 

Now, I copy the error into an LLM instead. I glance over its -- let's be honest, usually quite verbose -- analysis, and paste the suggested fix back into my terminal. I run it. Another error, a new one this time. So there we go again. Another copy-paste. Another analysis. Another copy-paste. And the loop continues.

It works, generally. But it's also quite tedious. And I feel like a messenger pigeon, ferrying text between two windows.

So lately I tend to go the other way. I avoid the terminal altogether and tell an agent to kick off whatever I need to run, fix whatever breaks, and fingers crossed hope for the best. Faster, sure. Less tedious, sure. But I don't love it, it leaves me a bit uneasy. The agent is executing commands I've never read or approved, and I've quietly stopped feeling in control of my own machine.

Then I heard about [ShellSage](https://github.com/AnswerDotAI/shell_sage).

It's an AI-powered command-line assistant that reads the terminal I'm already in. I can use my terminal as normal, but when I get stuck, I can invoke `ssage` and ask what's wrong, right where it broke. It answers with the command I probably need, and one shortcut drops that command straight onto my prompt. From there it's up to me. I can hit enter and run it. Or I can tweak a flag first, change a path, fix the bit it got slightly wrong.

Nothing runs unless I run it. But I do get context-aware support, right where and when I need it.

I haven't had the chance to try it yet, but I heard about it from my ex-colleague [Rens Dimmendaal](https://rensdimmendaal.com/) (who is now over at Answer.ai), who's always a great resource for these kinds of things. I'm excited to try it out! 
