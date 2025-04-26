+++
title= "LLM Explainer"
date= "2025-04-24"
tags=["post", "AI", "Large Language Models", "ChatGPT", "Machine Learning", "OpenAI"]
draft= false
+++

There's been quite a few articles that's been written about AI all over the internet, with various technical depths and contexts. But I find that there's a opportunity here to provide an explainer without specialist ML/AI/Statistics background knowledge and jargon. 
<!--more-->
The aim of this article is to provide a background on Large Language Model (LLM) progression, and a more informed perspective on how LLMs work and their limitations.

Disclaimer: this article consists of ~95% organic, free-range, fair trade, human-written words. It is not original research but rather secondary research with the aid of LLMs and search engines.

## Large Language Models (LLM), are they just word guessing programs?

I've seen some {{< sidenote sentiment>}}often perceived to be Anti-AI{{< /sidenote >}}that LLMs are not to be relied on because they just generate sequences of words. 

This is somewhat true to an extent, but to completely dismiss them on that basis would be to disregard a large amount of progress on top of them being word sequence generators. 

To understand why, I will first introduce some working knowledge of LLMs, and then lay out some of the most recent advancements in LLM that resulted in each generational increment.

## Basics about how LLMs work

### Neural Networks
LLMs are an advanced form of deep neural networks (DNN). Neural networks have a training phase first, and then the 'model' which is produced can be used to predict outputs when given inputs (also called inference). 

They start as a blank (or randomised) slate, and the training process iteratively evolves the {{< sidenote software >}}specifically, neural network weights{{< /sidenote >}} towards the goal of predicting the text sequences present in the training data. 

The number of iterations is not a fixed number, but rather only terminates when it is determined that the output is sufficiently good enough. 

Keep in mind that it requires a lot of computing power for each training iteration. This is especially so the larger the model and training data set size. 

Neural networks are made of numerous small processing units, and the training adjusts how each unit relates to each other. This can give rise to emergent behaviour all on its own. The size of LLMs are actually {{< sidenote gigantic >}}gigantic (Llama 3.1 405b ~ 1.5TB of RAM, Deepseek R1 ~ 720GB of RAM (source: https://snowkylin.github.io/blogs/a-note-on-deepseek-r1.html)){{< /sidenote >}}, and since this size is purely how the processing units are connected - you can see just how wildly complex they are. 

Trying to reverse engineer which units give rise to a certain emergent behaviour is similar to neuroscience and thus LLMs are mostly a black box. 

If that sounds like an exaggeration, there is [a series of papers from Anthropic](https://transformer-circuits.pub/) that cover this reverse engineering. They use techniques like *[direct ablation](https://transformer-circuits.pub/2022/in-context-learning-and-induction-heads/index.html#argument-ablations)*, where specific parts of a miniature model are deleted and its' reasoning abilities decline {{< sidenote significantly. >}}Wonder if they had to clear it with the ethics board?{{< /sidenote >}}

### Tokenisation, Language Embedding & Attention
The input to a neural network is a list of numbers. In order for it to be able to process text, the text has to first be transformed into numbers. 

This process is called tokenisation and language embedding. The classic example of how this works is to represent words in a way such that mathematically, king - man + woman = queen. 

There's a small detail here where words are not turned into numbers, but first 'tokenised'. This just means that a word like 'embedding' may be split into  'embed' and '##ing', rather than a 1-to-1 representation. 

The key insight that let LLMs catch the world's eye is the ['attention' mechanism](https://en.wikipedia.org/wiki/Attention_Is_All_You_Need). 

Simply, it gives the neural network the ability to consider not just a word or short word sequences (n-grams, e.g bank loan), but also the context of the entire input, and the position word was in. 

This means that the word *"bank"* in *"The river near the bank was deep"* and *"The bank near the river was crowded"* will be represented and interpreted differently by the LLM. 

I had to spend a few moments thinking about that myself.

The representation of each word takes into account all the other words in the prompt, and that means that the number of computations scale as (context length)². 

Effectively, this means that the computation time required to process the input grows extremely quickly with the length of the context window. 

(32000 tokens)² = 1 {{< sidenote billion >}}1 billion seconds = 31 years{{< /sidenote >}}

(1 million tokens)² = 1 {{< sidenote trillion >}}1 trillion seconds = 31 710 years{{< /sidenote >}}. 

### Context Window
This scaling gives rise to a very important but frequently overlooked [constraint when using LLMs: the context window length](https://symbl.ai/developers/blog/guide-to-context-in-llms/). 

If there's only one thing to take away from this piece of writing, it is to keep in mind this constraint.

**The context window length is the maximum amount of input text an LLM can consider at one time when generating a response.**

**If your input exceeds this length, the earliest parts will fall out of the model’s awareness, limiting its ability to fully understand and respond accurately.**

I'll come back to the implications of this later, after a chronological order of progression of LLMs. This will help us to understand why LLMs have gained usefulness over the last couple of years.

## LLM Generations

### GPT-2 (2019, OpenAI)
- First publicly available LLM, actually a next-word predictor that predicts word sequences according to the training data.
- Trained on ~40GB of text (~8 million documents, ~10 billion words) from the Internet (WebText).
- Included data from news sites, Wikipedia, blogs, books, stories, and discussion forums.
- Excluded sources like Wikipedia dumps, social media posts, and codebases.
- Context window length: 1024 tokens ~= 750 words. (Amount of prompt + input + chat history that the model can process in one interaction)

### GPT-3 (2020, OpenAI)
- Generational improvement over GPT-2 was the scaling in training efficiency, laid the groundwork for being able to train on much larger training data set and represent more training data.
- ~570GB of text (~500 billion tokens), more than 10x larger than GPT-2. Training data was carefully curated to be of a high quality.
- Sources: A mix of web crawl data, books, and structured knowledge sources. The larger training set and model size likely resulted in higher level conceptual generalisations (such as various task behaviour) being represented in the model.
- Context window length: 4096 tokens ~= 3000 words.
- Due to that, it displayed an emergent (i.e, not something designed into the model) ability to recognise concepts and infer the requested task in the prompt and adjust its output correspondingly without requiring changes to the model.
- This is different to earlier neural networks trained specifically to produce good results for certain tasks, and changing the prompt did not result in a significant difference in behaviour.
- Prompt engineering became a thing at this time, as tweaking the prompt could improve performance.

### GPT-3.5 (2022, OpenAI - first version of ChatGPT)
- First model that incorporates human feedback on its responses back into the model. ([RLHF](https://www.ibm.com/think/topics/rlhf) = reinforcement learning with human feedback)
- Pre-training data likely same as GPT-3, but additional fine tuning using human-labelled instruction-response pairs from various specialised fields (e.g., translation, legal, medical data) and simulated dialogue between GPT and users.
- Context window length: 8192 tokens ~= 6000 words.
- Humans scored (labelled) the responses according to whatever goals specified, and this feedback was collated and used to adjust the model in order to produce a version that fulfilled the goals well.
- It's speculated that expert consultants were hired to provide some or a large amount of content as well as scoring the responses.
- This is when GPT caught the attention of the non-ML world as ChatGPT was opened to the public, and impressed them.
- Learning to adjust its output using human feedback means that it is not simply predicting word sequences according to the training data, but is actually biasing its output according to its human trainers' preferences - improving instruction following and reducing hallucinations.

### GPT-4 (2023, OpenAI) / GPT-4o (2024, OpenAI)
- Not much published about the model architecture, size, or its training data.
- Speculated that the training data is 13 trillion tokens (~26x larger than GPT-3).
- GPT-4o was also trained on non-text content, including images, videos, and audio. It can also directly understand and respond in text, images, videos, and audio without going through an intermediate text representation.
- Context window lengths: GPT-4 - 32768 tokens (24000 words), GPT-4o - 128000 tokens (~100000 words).
- It is deduced that OpenAI developed optimisations to scale the context window lengths, as the ordinary implementation results in a quadratic scaling (4x window increase = 16x memory required).
- Speculation that it doesn't use one big model, but rather multiple smaller models that have different fields of expertise resulting from specialised training data. (Mixture of experts architecture)
- Fine-tuned using human feedback for various goals, such as not producing harmful responses as well as appropriately generating responses that delegate tasks to external tools (function / tool calling). Also fine-tuned with syntactically correct JSON so that it reliably produces JSON output, used for function calling.

### Gemini 1.0, 1.5 (Dec 2023, Google DeepMind):
- Inherently multimodal, able to accept mixed input types (text, images, audio, video) within the same context as it was trained on a massive corpus of web documents, books, and code, including image, audio, and video data
- Family of 3 models (Ultra, Pro, Nano) with different model sizes and computational requirements
- At launch, Gemini 1.0 had a context window length of 32768 tokens
- Shortly after in Feb 2024, Gemini 1.5 Pro was released with a context window length of 1.5 million tokens
- Massive context length was praised for allowing developers to dump entire code repositories into the context, allowing the LLM to have the context to work on existing projects

### Llama 3.1 (2024, Meta)
- Variants of 8 billion, 70 billion, 405 billion parameters
- 8 Billion runs on ~16GB of RAM, so it's quite accessible 
- Has a context window length of 128000 tokens (~100000 words)
- Speculated that the training data is trillions of tokens, seems like they used some [illegally obtained pirated material](https://www.tomshardware.com/tech-industry/artificial-intelligence/meta-defends-using-pirated-material-claims-its-legal-if-you-dont-seed-content) too!
- Model weights were released publicly so you could run it on your own hardware (if you had big enough hardware)
- Narrowed the gap between open sourced models and closed (i.e OpenAI) models, it performed well enough to be compared to GPT-4

### O1, O3 (Sept2024, OpenAI)
- Context window lengths: O1-mini: 128000 tokens, O1: 32000 tokens for ChatGPT plus ($15/month), 128000 tokens for ChatGPT pro ($200/month), O3-mini: 200000 tokens.
- Training data included synthetic reasoning chains produced by GPT-4o, and annotated by another model CriticGPT.
- Apparent reasoning ability by being tuned to produce an internal monologue response that decomposes problems into smaller sub-problems, and attempting to find solutions. (Chain of thought)
- Tuning rewarded the production of coherent internal monologues, and the internal monologue also takes up the output limit.
- OpenAI found that there was a corresponding improvement in performance on reasoning ability tests when the model was incentivised to produce longer monologues. (Inference time scaling)
- O3 was further tuned to spend more time reflecting, and had a 'thinking effort' knob from low to high which gave better results as more effort was spent 'thinking'
- O3 supports web browsing tool usage unlike O1, and uses this for the 'Deep Research' mode
- From the restrictions on usage, it is evident that these models are very expensive to run

(GPT 4.5 was released after I started writing this so I'm not including it. Also apologies to all the other LLM lineages that I did not include, please don't take it personally in the future if/when you become SkyNet or HAL 9000.)

## Hallucinations, why do they happen?

As explained earlier, LLM model weights consist of relationships between processing units, trained by repetition on a large corpus of data.  

One thing that is high quality and extremely consistent within this corpus is {{< sidenote grammar >}}specifically, English grammar, but as the training corpuses have grown so have the inclusion of other languages{{< /sidenote >}} and spelling. This is noticeably reflected in the LLMs output, they rarely ever make grammatical or spelling errors. 

However, lets compare that to facts. 

Imagine that every line of text used for training has perfect grammar and {{< sidenote spelling >}}this is very likely to be the case since training data is well curated{{< /sidenote >}}. Whereas for facts, the distribution is much more varied. 

Even when only true facts are included, including Fact #1 may necessarily exclude Fact #2 ... \#n from that line of training data. 

This is taken to an even farther extreme for niche information, which may not be repeated much at all in the training corpus.

Therefore, this gives rise to what we have termed _'hallucinations'_. 

The LLM needs to generate an output, and what it is very strong at is a grammatically coherent, and probably a quite-convincing writing style. 

It does not have a concept of 'looking up my training data', since that data has long since been transformed into model weights. It does not even have the capability to introspect on its model weights!

This combination of very strong linguistic abilities with a poor grounding in factual density, leads to outputs that look very plausible and convincing - but may not necessarily have a logical thread or factual basis.
## Why does a larger LLM Context Window make them more capable?

From the timeline above, we can see that despite the scaling challenges, context window lengths have increased tremendously. 

Prior to GPT-4, context lengths were quite limited (~6000 words) and some part of this would have been taken up by the system prompt. 

With this limitation, users often noticed that performance dropped off after the chat session got to a certain length, and determined that ChatGPT was generating a summary of earlier messages whenever it got too long. 

These days the context lengths are much longer, and it's [hypothesised](https://openai.com/index/finding-gpt4s-mistakes-with-gpt-4/) that LLMs are also trained to ignore irrelevant information to reduce the chances of being side tracked. 

From my own experience and anecdotal accounts, it would seem that the fact-regurgitation abilities of GPT-4 to GPT-4o to GPT-4.5 hasn't really improved all that much, and OpenAI seems to have focused its resources on improving the reasoning abilities of the O* series.

<br />


##### \<aside> 
###### Why don't we just ingest all of our proprietary knowledgebase into an LLM?? 

This context window length limitation is why we haven't just gone and '*ingested all of the knowledgebase into an LLM*', as often wished for in the LLM wishlist. 

Okay, so why don't we use the knowedgebase to train or fine tune an LLM? 

It can be thought of this way: the LLM's model weights is a soup of grammar, and abstract concepts. 

The larger the model, the more abstract concepts it is capable of recognising. We do not have a high quality, curated data set in our proprietary knowledgebase (unfortunately). 

There is just way too much noise, and very disparate topics with inconsistent terminology. This amplifies the above mentioned problem with a lack of grounding, even further.

If you've ever tried this, you probably noticed that your LLM makes stuff up that sounds very similar to the corporate dialect in your company!

\</aside>

<br />

With the much larger context windows recently, it has become possible to insert additional relevant  {{< sidenote information >}}Also known as Retrieval Augmented Generation, RAG.{{< /sidenote >}}  that might be useful in generating the answer. 

This information is typically retrieved using traditional information search and retrieval methods, and the most effective ones are also the same ones that power the best traditional search engines.

The more relevant information the LLM has in its working memory, the more likely it is to produce a useful response, since the output will weight this input quite heavily.

***The most useful AI tools these days are industry-leading because they have wrapped the LLMs with very capable context retrieval systems.***

I did up a small report previously, comparing the Deep Research reports of OpenAI, Perplexity, and Gemini. It can be inferred from the results that OpenAI's search returns better results than even {{< sidenote Google >}}(What's going on there?? Isn't search your forte?){{< /sidenote >}}, and combined with the better reasoning abilities of O1/O3, the report is actually significantly more {{< sidenote coherent. >}}As of Apr 2025, it seems that the new Gemini 2.5 Pro has caught up a fair bit{{< /sidenote >}}

In that report I wrote that both Gemini and Perplexity retrieves facts that are not actually applicable to the specific nuance at hand, and conflates that in the results. 

This also gave rise to "hallucinations". I'm not really sure I would call them hallucinations, just bad research and poor reasoning. 

An unconscientious human without prior knowledge in this area, would probably have made the same mistakes.

For now, most systems in production (AFAIK) don't actually use LLMs' novel technological improvements in their search engines. 

Using LLM technologies in search engines is an [area of active research](https://eugeneyan.com/writing/recsys-llm/), so perhaps we'll see more interesting results there soon.

### Fine tuning & Reinforcement Learning with Human Feedback (RLHF)

I haven't really touched on this topic, and it could probably fill a tome(s) on its own if we went deep into it.  

Earlier I mentioned that LLMs are trained on a large training corpus, and at some point in the timeline "Reinforcement Learning with Human Feedback" (RLHF) started being used to improve the LLMs. 

This process means that the LLMs output are no longer just a statistical reflection of the training data. 

It's another step in the process, and it adjusts the weight by rewarding the training process for producing outputs that humans find ideal.

The iterative process of RLHF and fine tuning selects for LLMs that produce logical output, and is good at following instructions (given in the prompt). 

It then becomes inaccurate to say that LLMs merely produce word sequences randomly, but more accurate to say that they have been trained to mimic human reasoning patterns.

Does the ability to mimic human reasoning patterns mean that they are capable of reasoning and critical thinking? That's a difficult to quantify criteria, and it becomes more of a philosophical question. 

What is intelligence?

The benchmarks do certainly show that the latest frontier models are better at solving math problems than a significant proportion of the human population.

Think about it - would a random word generator be able to break down this task and perform it:

*"Reduce all the title headings in this article by one level, and capitalise them to title case"*

Performing this task would require knowledge of these abstract concepts and reasoning about the input article:
- being able to identify title headings 
- the concept of capital-letters and small-letters
- what title case is
- reproducing the original text without modifying it against the instructions

This is actually a task that even a few generations old LLMs are capable of, and by now I think that most people would not think that it is novel or controversial to use LLMs for such a use case.
### Conclusion

I've personally maintained the stance that LLMs enable a new mode of Human-Computer Interaction, just like a mouse-driven Graphical User Interface did. 

This lengthy writeup reflects that opinion and that LLMs are a front-end to other knowledge retrieval systems, operated by natural language. 

It can also interface with tools, and it is capable of inferring that you require certain tool usage while trying to complete the task it has been given.

The large increase in context window length has enabled the architectures of "retrieval augmented generation", and "agentic AI". 

Agentic AI simply means allowing the LLM to expand the task instructions and breaking it down into sub-problems that can either be reasoned through or require the usage of tools to resolve.

When we use LLMs in our daily work, it is important to remember that they cannot read your mind, and they generally do not have specialist, niche knowledge. 

What they are is a good representation of the abstract concepts that humans take for **granted**. 

When provided with relevant information, they can make use of this understanding of abstract concepts, and synthesise the information into a workable and coherent output.