---
title: 'Sci-Summarization'
date: 2012-08-14
permalink: /posts/2022/02/Sci-Summarization/
tags:
  - cool posts
  - summarization
  - nlp
---

<!-- This is a sample blog post. Lorem ipsum I can't remember the rest of lorem ipsum and don't have an internet connection right now. Testing testing testing this blog post. Blog posts are cool. -->

# 【层次-sci】A Discourse-Aware Attention Model for Abstractive Summarization of Long Documents ——2018

## 数据集

第一个提出了abstractive的single long document的摘要。由于长文本通常以research paper为主，所以提出了一种层次建模的方法，用来解决算力和长文本前后融合的问题。

## 模型

是层次的RNN，分别在word级别和section级别构建了Attention模块。

<img src="../images/posts/summarization/截屏2021-11-24 下午9.43.44.png" alt="截屏2021-11-24 下午9.43.44" style="zoom:60%;" />


# SEAL: Segment-wise Extractive-Abstractive Long-form Text Summarization ——2020

重点在于segment-wise。好像和sci-sum不是很相关，没有细度

segment是文本被分成的等长片段，文中探讨了如何确定segment和extractive的长度：For the SEAL model, when the segment length Lseg is large, the decoder effectively attends to less context. On the other hand, when Lseg is as small as a few words, the similarity labels become very spurious and are not meaningful to guide extraction.

<img src="../images/posts/summarization/截屏2021-11-24 下午10.05.38.png" alt="截屏2021-11-24 下午10.05.38" style="zoom:60%;" />

# 【EDU】Do We Really Need That Many Parameters In Transformer For Extractive Summarization? Discourse Can Help ! ——2020 ACL

利用了document level的信息，主要是在EDU（在Discourse-Aware Neural Extractive Text Summarization —— 2020 里面有提及）级别做了一些研究，本处不再详细介绍。

# 【facet】Improving Unsupervised Extractive Summarization with Facet-Aware Modeling

## 无监督模型

对比 pacsum：

<img src="../images/posts/summarization/截屏2021-11-29 下午7.59.01.png" alt="截屏2021-11-29 下午7.59.01" style="zoom:50%;" />

通过对sent/doc的表示建模，分析句子和facet的距离。

计算候选句子和document的representation的余弦相似度：句子表示用bert的[cls]表示的sigmoid生成，doc的表示是句子表示的maxpooling

<img src="../images/posts/summarization/截屏2021-11-29 下午10.26.18.png" alt="截屏2021-11-29 下午10.26.18" style="zoom:50%;" />

之后还improve了句子表示：在训练bert的时候，希望：

<img src="../images/posts/summarization/截屏2021-11-29 下午10.32.14.png" alt="截屏2021-11-29 下午10.32.14" style="zoom:50%;" />

# Multi-Granularity Interaction Network for Extractive and Abstractive Multi-Document Summarization 2020ACL

在word、sent、doc层级应用attention机制，其中 word representation用于生成式摘要，sent representation用于抽取式摘要。

<img src="../images/posts/summarization/截屏2021-11-30 下午12.05.24.png" alt="截屏2021-11-30 下午12.05.24" style="zoom:50%;" />

一层atetnion，一层fusion

<img src="../images/posts/summarization/截屏2021-11-30 下午2.30.00.png" alt="截屏2021-11-30 下午2.30.00" style="zoom:50%;" />

由于多文本摘要过长，使用multi-head sparse attention as MSAttn.【[Hierarchical Transformers for Multi-Document Summarization](https://aclanthology.org/P19-1500.pdf) [Yang Liu](https://aclanthology.org/people/y/yang-liu-edinburgh/), [Mirella Lapata](https://aclanthology.org/people/m/mirella-lapata/)】【EXPLICIT SPARSE TRANSFORMER: CONCENTRATED ATTENTION THROUGH EXPLICIT SELECTION 2019】



# Hierarchical Transformers for Multi-Document Summarization 2019 lapata

多文档摘要，因为文章多且长，采取paragraph ranking的方式，选取前k个para。

之前【Peter J Liu, Mohammad Saleh, Etienne Pot, Ben Goodrich, Ryan Sepassi, Lukasz Kaiser, and Noam Shazeer. 2018. Generating Wikipedia by summariz- ing long sequences. In *Proceedings of the 6th Inter- national Conference on Learning Representations*, Vancouver, Canada.】使用计算paragraph和title的tfidf排序的方式，但本文采用一种基于lstm的神经网络来进行para的排序，有监督回归：para和title的rouge-2 Recall





# 【facet 2021】Discourse-Aware Unsupervised Summarization of Long Scientific Documents

## 无监督模型

<img src="../images/posts/summarization/截屏2021-12-02 下午8.33.47.png" alt="截屏2021-12-02 下午8.33.47" style="zoom:70%;" />

介绍有监督模型的局限性：However, these models cannot easily be adapted to out-of-domain data that have greater length and fewer training examples such as scientific article summarization (Xiao and Carenini, 2019) due to two significant limitations. 

- First, they require large domain-specific training pairs of source documents and gold-standard summaries, which are often not available or feasible to create (Zheng and Lapata, 2019). 
- Second, the typical setup of using a token- level encoder-decoder with an attention mechanism does not scale well to longer documents (Shao et al., 2017), as the number of attention computations is quadratic with respect to the number of tokens in the input document.

【由于有监督模型的局限性，针对第一点，我们也在标签的构造上做了许多探索。针对第二点，预训练模型使用longformer的同时，使用图结构（句子和实体关系）减少对token的attention计算。】

文章提出之前的长文本摘要模型忽视了不同section的information的主题含义不同。介绍了2019年的两篇使用了section的信息的文章。

<img src="../images/posts/summarization/截屏2021-12-02 下午9.13.54.png" alt="截屏2021-12-02 下午9.13.54" style="zoom:50%;" />

图结构：section内部句子之间用相似度建边，不同section之间也有连接。由于不同section之间句子连接的计算过于庞大，所以只使用sentence-seciton之间的连接。

## Boundary function

使用不对称的edge weighting。前提是：研究证明，在section开头或结尾的句子是比较重要的。

<img src="../images/posts/summarization/截屏2021-12-02 下午9.49.07.png" alt="截屏2021-12-02 下午9.49.07" style="zoom:50%;" />

设置了不同的lambda来调整句子的edge weight：当i和更重要的句子j相连时，其weight要更高。

<img src="../images/posts/summarization/截屏2021-12-02 下午10.07.12.png" alt="截屏2021-12-02 下午10.07.12" style="zoom:50%;" />

<!-- Headings are cool -->
======

<!-- You can have many headings -->
======

<!-- Aren't headings cool? -->
------