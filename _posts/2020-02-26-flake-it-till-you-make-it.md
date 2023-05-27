---
layout: post
title: Why are current LLMs all decoder-only?
subtitle: Each post also has a subtitle
share-img: /assets/images/flake-it-till-you-make-it.jpg
tags: [LLM, GPT, ChatGPT]
---

While most modern Large Language Models (LLMs) are based on the Transformer, notable architectural variations exist. There are encoder-only (BERT-like), decoder-only (GPT-like), and encoder-decoder (BART/T5-like) models, which are respectively considered adept at handling discriminative, generative, and both types of tasks. 

ChatGPT has quickly become a highly successful decoder-only model, gaining widespread recognition and usage. Numerous new LLMs have since been released, all of which are also decoder-only.
Fewer institutions seem to show interest in encoder-decoder models, except for the recent releases of ChatGLM and FLAN-UL2. Additionally, OpenAI has exclusively focused on decoder-only models since the beginning.

So, the big question is why has the decoder-only architecture become the mainstream choice for LLMs, over encoder-decoder architecture? Is there something OpenAI knows that we don't? 

### Experimental 

The BigScience team had the same question during the architectural design phase of the BLOOM model {% cite scao2022bloom %}. In the work of {% cite wang2022language %}, they evaluated these two architectures, pre-trained with Causal Language Modeling (CLM), Prefix Language Modeling (PLM) {% cite liu2018generating %}, and Masked Language Modeling (MLM) {% cite raffel2020exploring %} objectives, as well as the impact of multitask finetuning (now known as instruction-tuning). The study focuses on **zero-shot generalization** ability, i.e., the performance of a single pre-trained (then instruction-tuned or not) model, tested on a wide variety of (unseen) downstream tasks). The conclusions are: a decoder-only pre-trained with CLM performs best if evaluated immediately after pretraining, whereas when adding an instruction-tuning step, an encoder-decoder pre-trained with MLM performs best. Well, this is surely not the convenient answer we are waiting for.

In the literature, more evidence can be found in contrast with this trend of decoder-only architecture. Under the **transfer learning** setting, i.e. a pre-trained model, then finetuned on a *single* downstream task, then evaluated on this particular task, {% cite raffel2020exploring %} shows that encoder-decoder models **significantly** outperform decoder-only models. 
<!---
Sanh et al. [2021] proposed a multitask finetuned encoder-decoder LLM that outperforms decoder-only models on zero-shot generalization, despite being an order of magnitude smaller.
-->

It is worth noting that the aforementioned experiments draw certain conclusions by comparing (L + L layers) encoder-decoder models with 11B parameters to (L layers) decoder-only models with 4.8B parameters (is this a fair comparison?). The comparison is conducted under the fact that the same L + L encoder-decoder model will have approximately the same computational cost as an encoder with L layers.
Furthermore, it raises questions regarding the applicability of these conclusions to current LLMs with over 100 billion parameters. Thus, in conclusion, I would say there remains a need for a systematic evaluation of these two architectures.

Reference
---
{% bibliography --cited %}
