+++
title= "M in LLM stands for Model"
date= "2025-05-24"
tags=["post", "AI", "Large Language Models", "ChatGPT", "Machine Learning", "OpenAI"]
draft= false
+++

A follow up to my previous [LLM explainer](./llm-explainer). In that post, I discussed the recent evolution of LLMs and why the increase to their context window length improved their usefulness significantly.

In this post I will discuss one of the limitations of LLMs, and that is that they are a simplified representation of ... their training data.

# The M in LLM stands for 'Model'.

The [OED definition of model](https://www.oed.com/dictionary/model_n#36511578):

>A simplified or idealized description or conception of a particular system, situation, or process, often in mathematical terms, that is put forward as a basis for theoretical or empirical understanding, or for calculations, predictions, etc.; a conceptual or mental representation of something.

# All models are wrong

>The phrase "all models are wrong" was attributed[[1]](https://en.wikipedia.org/wiki/All_models_are_wrong#cite_note-Nester-1996-1) to George Box who used the phrase in a 1976 paper to refer to the limitations of models, arguing that while no model is ever completely accurate, simpler models can still provide valuable insights if applied judiciously.

As much as LLMs have grown in size recently, they are still smaller than their training data set. This inherently means that some information has been lost, since it is not a lossless compression algorithm. 

Due to the probabilistic nature of LLM training, it's the fine details of reality that are lost. The overall structure of reality is probably quite well represented, within everything that humans have written down and published.

# Reality has a surprising amount of detail

A short excerpt from [an article by John Salvatier](http://johnsalvatier.org/blog/2017/reality-has-a-surprising-amount-of-detail), pre-dating the era of LLMs.

>And if there’s one thing I’ve learned from all this building, it’s that reality has a surprising amount of detail.
>
>This turns out to explain why its so easy for people to end up intellectually stuck. Even when they’re literally the best in the world in their field.
>
>Consider building some basement stairs for a moment. Stairs seem pretty simple at first, and at a high level they are simple, just two long, wide parallel boards (2” x 12” x 16’), some boards for the stairs and an angle bracket on each side to hold up each stair. But as you actually start building you’ll find there’s a surprising amount of nuance.
>
>The first thing you’ll notice is that there are actually quite a few subtasks. Even at a high level, you have to cut both ends of the 2x12s at the correct angles; then screw in some u-brackets to the main floor to hold the stairs in place; then screw in the 2x12s into the u-brackets; then attach the angle brackets for the stairs; then screw in the stairs.

Anyone that has ever built a product for users (even something for yourself) would know that building something is never as easy as it sounds. This is because the closer you look at real world systems, or real world users, the more complex they get. Like fractals, every part reveals smaller moving parts, each with its own context, dependencies, and gotchas.

In a product with heritage and legacy, you also need to understand how any change fits into the context of legacy decisions, constraints, team workflows, user expectations, security concerns, and business goals.
# Context and Reasoning

The finest examples of LLM productivity improvements show up when the LLM is properly prompted with all the context around the task. 

While it is debatable about whether LLMs can truly reason, it can be quite a magical experience watching the chain-of-thoughts internal monologue on a reasoning model. 

Some of these reasoning LLMs also have the ability to make web searches. They can retrieve information from the Internet to populate their context, and to fill in some of the fine details that have been lost.

# Does that replace humans?

No.

Doing something new - building a novel system, inventing a new algorithm, exploring new science - requires attention to the fine-grained, often invisible layers of reality that don’t follow precedent. 

LLMs operate on compressed patterns, and those patterns flatten out the very details that innovation depends on. When a model hasn’t seen something before, it guesses based on statistical gravity, not understanding.

The difference is critical: scientists and engineers handle the unknown by interrogating it. Models interpolate, extrapolate or simulate.

# **They’re Tools, Not Replacements**

This doesn’t mean LLMs are useless. Far from it. They can accelerate tedious tasks, help explore ideas, and even spot bugs. 

But they’re force multipliers - not substitutes. Like any tool, they require someone who knows what they’re doing to wield them safely and effectively.