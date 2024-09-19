---
layout: post
title:  "Is your RAG Re-Ranker not helping? This might be why."
date:   2024-09-18 11:52:39 +0200
categories: rerankers
---  

Sarah sighed in frustration as she stared at her laptop screen, the SSH connection to the university server dropping for the third time in the last hour. She was trying to access some important research files for her thesis, but the constant interruptions were driving her mad.

"Why is my SSH connection to the university server breaking so often?" she muttered to herself, fingers flying over the keyboard as she typed the question into the library's intranet chatbot.

The AI assistant responded promptly: "I'm sorry to hear you're having trouble with your SSH connection. Let's start by checking your SSH setup. I can guide you through the steps to establish a proper SSH connection to your remote university server. Would you like me to walk you through that?"

Sarah frowned, her cursor hovering over the 'Yes' button. She'd already double-checked her SSH setup multiple times, but maybe there was something she'd missed? As she reached for her mouse to click, a gentle tap on her shoulder made her turn around.

"Having trouble connecting?" asked Mike, one of the library's IT assistants. He'd been making his rounds and noticed Sarah's frustrated expression.

"Yeah," Sarah replied, gesturing at her screen. "The SSH keeps dropping. The chatbot suggested I check the SSH setup guide again."

Mike leaned in, glancing at the conversation. A knowing smile crossed his face. "Ah, I see this all the time. You might want to check out the Wifi troubleshooting guide instead."

"The Wifi guide?" Sarah asked, surprise evident in her voice. "But I'm not having issues with the internet itself, just the SSH connection."

"Trust me," Mike said with a chuckle. "It's the most common issue we see. The Wifi in some parts of the library can be pretty unreliable, which causes SSH connections to drop frequently. Most people assume it's an SSH problem, but nine times out of ten, it's just the Wifi acting up."

Sarah blinked, processing this information. "Huh, I never would have guessed. Thanks, Mike!" She typed a new query into the chatbot, asking for the Wifi troubleshooting guide instead (the guide would tell to move elsewhere or use cable).

---

Moral of the story: If the university's RAG was implemented correctly with the fine-tuned re-ranker, Sarah would have gotten the *most likely relevant answer* on the first try.

Behind the scenes, three documents were retrieved (in this order) and the first one was selected, as it had the best score, to answer Sarah's query:

1. Steps to establish SSH connection to your remote university server.
2. Onboarding for new students at the campus.
3. Troubleshoot Wifi connection at the library.

A fine-tuned re-ranker here could have learnt to rank that third article highest. In my view, the re-ranker models **learn the joint distribution of the query-document pair relevance**. In other words, the model needs to learn that most people who have this problem actually need the library Wifi connection article on account of this article having been repeatedly proven to be more useful for the users than the first one (we ignore for this discussion how to obtain these metrics).

The point is that this knowledge is simply impossible for any pre-trained re-ranker to have since without fine-tuning, the model has no way of learning this domain relevance, in this case, of university-specific issues.

## What do re-rankers do?

Re-rankers have become a popular component of advanced Retrieval-Augmented Generation (RAG) pipelines.  *But what makes re-rankers better than retrievers at recognizing the relevance of a chunk for a given query?*

The basic idea behind re-rankers is simple: we retrieve document chunks against a user query, then use a re-ranking model to evaluate the relevance of each chunk-query pair. These scores are then used to sort the documents, resulting in a "re-ranked" order. Typically, more documents are retrieved than needed, with only a subset making it through (to the prompt) after re-ranking.

## Conventional wisdom on why they work

A re-ranker is often a *cross-encoder, meaning it looks at both the chunk and query together in a single forward pass. This is different from retrievers (often bi-encoders*), which separately embed queries and chunks, then calculate their similarity from those embeddings. It is argued that joint encoding gives these models a more nuanced understanding of relevance than retrievers, as similarity ≠ relevance (the weakness of retrievers) and there is indeed merit to this claim.

However, this argument overlooks a critical factor: **the data re-rankers are trained on matters**. A pre-trained re-ranker may have learned "general" relevance patterns, but these are often insufficient for tasks in specialized domains. In my experience, better embeddings (say from Cohere) can outperform the older sentence transformers retriever + re-ranker combo. But a fine-tuned re-ranker, on the other hand, can handily beat the best of embeddings.

## So are re-rankers useless?

Pre-trained re-rankers are often not the magic bullet they are made out to be. Re-rankers can truly shine, but only when they are **fine-tuned** on domain-specific corpora. The problem? Domain-specific training data is hard to find. Ideally, you'd want a dataset where the "relevance" of a user query and a document (or its chunk) is marked on a continuous scale (e.g., 0.0 to 1.0).

However, it is quite difficult to organically obtain this kind of data directly in most applied settings. I have worked with binary (relevant/not relevant) data or used proxies like normalized reciprocal search rank approaches to derive relevance scores. The synthetic data generation approach, often touted as a viable option, may help but will never give the same results as organically collected data. The example above hopefully clarifies why true relevance cannot be extracted from the document corpus itself.

Many widely-used re-ranking datasets, like [**MS MARCO**](https://microsoft.github.io/msmarco/), dominate the training landscape of pre-trained re-rankers, highlighting the fundamental challenge of gathering this kind of data.

## Conclusion

So, next time you’re considering deploying a pre-trained re-ranker, remember that its true success hinges on fine-tuning. At the very least, try to measure how impactful it is. Without it, your re-ranker is likely just as useful as the advice Sarah got from the chatbot: not very.
