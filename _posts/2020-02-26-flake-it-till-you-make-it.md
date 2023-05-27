---
layout: post
title: Why are current LLMs all decoder-only?
subtitle: Each post also has a subtitle
share-img: {% generated_image_path %}
tags: [LLM, GPT, ChatGPT]
---


While most modern Large Language Models (LLMs) are based on the Transformer, there exist notable variations in their architectural implementations. Specifically, there are encoder-only models (BERT-like), decoder-only models (GPT-like), and encoder-decoder models (BART/T5-like), which are respectively considered adept at handling discriminative, generative, and both types of tasks.

Recently, ChatGPT has emerged as a highly successful decoder-only model that has garnered widespread recognition and usage. Since then, new LLMs have been released almost on a daily basis, such as LLaMA, Pythia, MPT-7B, Cerebras-GPT, PaLM 2, Claude, and others. However, all these models are decoder-only.  
It is obvious that we omit encoder-only from consideration since it is limited to producing the same number of tokens as it was fed as input, which is rarely used in NLG [Tamborrino et al., 2020]. 
However, It appears that fewer institutions are interested in training an encoder-decoder model, with the exception of ChatGLM from Tsinghua University and FLAN-UL2 from Google, both of which were released this year. Additionally, OpenAI has exclusively focused on encoder-only models since the beginning.

So, the big question is why has the decoder-only architecture become the mainstream choice for LLMs, over the encoder-decoder one? Is there something OpenAI knows that we don't? 

### Experimental 

The BigScience team had the same question at the point of architectural design for the BLOOM model {% cite scao2022bloom %}. In the work of {% cite wang2022language %}, they evaluated these two architectures, pre-trained with causal language modeling (CLM), prefix language modeling (PLM) {% cite liu2018generating %}, and masked language modeling (MLM) {% cite raffel2020exploring %} objectives, as well as the impact of multitask finetuning (now known as instruction-tuning). The study focuses on the **zero-shot generalization** abilities of these models, i.e., the performance of a single pre-trained (then instruction-tuned or not) model, tested on a wide variety of downstream tasks). The conclusions are: a decoder-only pre-trained with CLM performs best if evaluated immediately after pretraining, whereas when adding an instruction-tuning step, an encoder-decoder pre-trained with MLM performs best. Well, this is surely not the convenient answer we are waiting for.

In the literature, more evidence can be found in contrast with this trend of decoder-only. Under the **transfer learning** setting, i.e. a pre-trained model is finetuned on a *single* downstream task for evaluation, Raffel et al. [2020] have shown that encoder-decoder models **significantly** outperform decoder-only LLMs. Sanh et al. [2021] proposed a multitask finetuned encoder-decoder LLM that outperforms decoder-only models on zero-shot generalization, despite being an order of magnitude smaller.

It is worth noting that the aforementioned experiments draw certain conclusions by comparing encoder-decoder models with 11B parameters to decoder-only models with 4.8B parameters (is this a fair comparison?). 
the same L + L encoder-decoder model will have approximately the same computational cost (step time) as an encoder with L layers.
Furthermore, it raises questions regarding the applicability of these conclusions to larger language models with over 100 billion parameters. Thus, in conclusion, I would say there remains a need for a systematic evaluation of these two architectures.

{% bibliography --cited %}
