---
layout: post
title:  "Two kinds of LLM responses: Informational vs Instructional"
date:   2024-09-24 00:52:39 +0200
---

When thinking of LLM evals especially in the context of RAGs, it occurred to me that there are two kinds of distinct responses people get from LLMs: informational and instructional.

While the difference may seem trivial, I think it has strong implications on evals. Most commonly available datasets like MMLU, GSM8k etc. are ridiculously small not just in context (a problem in its own right given how big context windows have grown) but specially in their tiny response size. Moreover, these tend to have one clear answer. Furthermore, trying to understand something via LLMs in my opinion also falls in the domain of informational where the information provided may be more conceptual.

As opposed to all that, people use LLMs for all sorts of things like getting detailed cooking instructions or getting step-by-step instructions on how to backup data. All these are clearly instructional where the responses tend to be much, much longer and follow a strict sequence. In such cases, skipping one step or the other does matter so evaluation needs to concentrate on that aspect. Deleting data before making sure we backed it up matters.

In my experience, **this is more pronounced in the domain-specific RAG systems like that of enterprises** where people may forget SOPs for different tasks. People ask about how to get bigger inbox quota, HR policies, how to apply for vacations etc. Things often boil down to a "how-to" question since RAG skips the "where can I find..." step by doing that for you.

Granted, there are libraries that try to measure faithfulness, factual correctness etc. for your RAG solutions. Yet, none of the metrics I have seen treat the instructional forms of LLM responses as a separate category.
