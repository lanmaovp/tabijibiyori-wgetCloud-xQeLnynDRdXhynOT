[合集 \- NLP(4\)](https://github.com)[1\.论文速读纪录 \- 2024082024\-09\-04](https://github.com/gzyatcnblogs/p/18395699)[2\.论文速读记录 \- 2024102024\-10\-31](https://github.com/gzyatcnblogs/p/18519035)[3\.语言模型文本匹配的主流方法回顾2024\-12\-28](https://github.com/gzyatcnblogs/p/18637628)4\.浅谈文本匹配01\-04收起
文本匹配，即比较两个文本是否在某个维度上匹配，通常是比较两个文本之间是否在表达相同的意思，所以文本匹配一般也归结为计算两个文本之间的相似性。当然“相同的意思”也是不太好定义的，鉴于语言的复杂性，文本匹配通常是在相对直接的层次计算文本之间的相似性。
**目录：**


目录* [字面匹配 \& 语义匹配](https://github.com)
	+ [字面匹配](https://github.com)
	+ [语义匹配](https://github.com)
	+ [小结](https://github.com)
* [一些挑战](https://github.com)
	+ [长短困境](https://github.com)
	+ [词序感知](https://github.com)
	+ [多实体关系](https://github.com)
* [总结](https://github.com)
* [参考](https://github.com)

# 字面匹配 \& 语义匹配


文本匹配的方法先后经历了字面匹配和语义匹配。


## 字面匹配


字面匹配即衡量文本间的字面的相似性，比较看重文本间的重合度，通常也叫做精确匹配。一些经典的文本相似性方法，比如：


* 基于集合的，对文本进行切分后计算集合相似性，比如Jacard相似度等。
* 基于距离的，计算文本之间的编辑距离、海明距离等。
* 基于表示的，通常是将符号化的文本转化为向量空间中的一个点，再计算向量空间中点的距离，比如词袋模型、tf\-idf。


字面匹配比较直接，通常在文本共现的基础上做一些简单的变换和统计，对文本进行相对直接的向量化（与后续的词向量、语义模型相比）。当然，实际应用中的字面匹配会更复杂，会融入更多的人工经验和规则，比如关键实体的命中、匹配时的紧密度等，典型的代表有BM25。


看起来字面匹配大多是一些偏统计、人工的方法，有时候难以处理文本的复杂变换，比如同义词、缩写、口语化等情况，因此也有一些用深度学习做字面匹配的方法。比如 [语言模型文本匹配的主流方法回顾](https://github.com) 中提到的匹配模型，以文本间的各种匹配作为DNN的输入来预测文本间的相似性。


## 语义匹配


语义匹配即衡量文本间的语义相似性，但“语义”是一个比较模糊的概念，语义匹配也可以看作是一种模糊匹配。语义相似自带一点玄学的气质——两个字面很相关的文本可以是语义相似的，字面不相关的文本可能也是相似的——是否语义相似的通常需要参考所处的上下文（比如“苹果”一词的匹配，需要根据上下文确定其指的是水果还是手机）。关于语义的描述可以参考\[1]。


语义匹配的基础是向量化，即将文本转化成一个稠密向量。文本向量化的脚步很早就开始了：词袋模型、TF\-IDF、N\-gram、主题模型、词向量（word2vec等）、语言模型。可以看到其中的趋势：向量化方法逐渐复杂，逐步简单的统计转变为深度模型，直到现在基于Transformer的语言模型。


那是什么时候开始语义匹配成为文本匹配中的主流的呢？个人觉得至少要从词向量阶段开始算起。以word2vec为例，word2vec应该算是词向量方法中的一个重大突破和代表了，通过稠密向量表示词，通过向量计算词之间的关系，其中最经典的莫过于“king \- man \+ woman \= queen”了。大概从这个时候起，大家发现文本的语义关系是可以蕴含在向量中的，后来越来越复杂的方法用来进行文本向量化，人们期望学习到的向量能够捕捉文本中更复杂的语义信息。


现在的文本向量化基本都是在基于Transformer的语言模型上发展出来的，在大量的语料上进行预训练训练，再在特定的任务上进行微调以适应特定的场景或任务。如果出现两个模型从未见过的文本，模型能算出它们的语义关系吗（当然已经没有多少数据是大模型没见过的了）？


个人的一个观点：`当前语言模型学习的是文本的共现模式，学习到的这种共现关系就是我们通常所说的语义`。这种共现关系不再止步于滑动窗口中文本共现的次数，可能是更多跳的关系，这种多跳关系便蕴含了我们所知的语义。换句话说，语言模型是在计算更复杂的统计关系（类似的观点应该更为人知和接受）。基于这样的认识，如果想要构建更强大，泛化能力更强的语义模型，尽可能多样化（不只是数据多，数据类型也要多）的数据应该是必不可少的。特别的，实际应用时语义模型还要尽可能见过场景下的数据。


## 小结


如果狭义地定义相关匹配为传统的相关性计算，那么我以为语义匹配包含了相关匹配，期望其能够做到更复杂的相关匹配；但广义上来说，相关匹配可以很复杂，目前的语义匹配不过是相关匹配的延展。\[5]中对字面匹配和语义匹配的特点做了一个简单的总结：




|  | 字面匹配 | 语义匹配 |
| --- | --- | --- |
| **相似定义** | 看重term之间精确的匹配信号。 | 模糊匹配，不局限于字面相似，可能是某种关系有关联，比如问题和答案的关系。 |
| **匹配粒度** | 通常是短文本和长文本的匹配，可以只匹配长文本的部分内容。 | 适用于长度相近的文本进行匹配，匹配时关注文本整体的含义。 |


关于匹配粒度，有两个假设可以参考：


* verbosity hypothesis：假定长文档是围绕一个话题的，只是词比较多，更像语义匹配的要求。
* scope hypothesis：假定长文本是由多个不同话题的短文本组成的，更像字面匹配。


在字面匹配发展后期也有很多借助深度学习做字面匹配的工作，但随着语言模型的发展，现在越来越倾向语义匹配（不过技术发展路线，还是实际应用），一般不会特意去构造传统意义上的相关性信号。为什么呢？LLM的训练数据和参数都是巨大的，文本的各种共现关系都见过了，且数据量足够大，学习到的向量以及模型的参数能够较好的保留文本的关系。非大语言模型的情况 —— 预训练数据不够/参数不够多/资源紧张 的情况下，根据以往的经验，语义匹配有时候做的并不是很理想。常见的问题：


* 相似实体难以区分："如何扫码加**微信**" vs "如何扫码加**微信群**"。
* （多）实体命中缺失："**5月5日北京大学**能开学吗？"、"**锂电池数据采集**方法"。


这应该是语义匹配一直以来的缺陷，难以进行细粒度匹配。为什么会这样呢？可能的原因：1）预训练不足。预训练中缺少相关的数据以及训练不充分，导致向量学习不够准确，这个应该是主要原因；2）参数量。最终的结果就是有点“四不像”，单纯的语义匹配总是不够的，这就需要相关性匹配来辅助一下了。


在LLM展现的性能上限面前，大家都在思考如何借助LLM发挥出其真正的价值，更好的落地。


# 一些挑战


## 长短困境


在实际场景中，文本匹配可以有很多形式，比如：query\-title、query\-query、query\-doc、question\-answer。其中query\-title、query\-query都是短文本间的匹配，二者的长度近似，可以看作是相同的空间（当然，其他维度来看query和title也是在不同空间的），但query\-doc的匹配、question\-answer的匹配却不太一样：长度一般差异较大。长短文本匹配可以看作非对称域的文本匹配\[3]。


正如前文中提到的两个假设verbosity hypothesis和scope hypothesis，一段文本可能只讲了一个主题的内容，也可能包含了多个可独立的主题的内容。短文本与长文本进行匹配时，可能就会遇到这种情况 —— 短文本是在与长文本整体进行匹配，还是与长文本中的片段进行匹配，或者要兼顾片段和整体呢？


长短匹配时一般会出现长文本语义向量语义模糊的问题，导致难以做到比较细粒度的匹配。长短困境不只是场景中的问题，通常也是技术上的一个难点。语义向量中常用的BERT类模型大多都有一定的长度限制（大多是512或者更短），当然也不是窗口越长越好 —— 过长，会稀释关键内容的占比；过短，无法捕捉整体语义。字面匹配大多基于统计、频率等计算相似性，但或多或少也会受到文本长度的影响。


实际中怎么解决长短困境呢？可以再具体一下长短匹配时的问题：


* 关键实体适配。比如query中的关键实体，在长文本向量语义中没有充分表达，或者字面匹配时不能区分哪些词更重要。
* 模糊性。主要出现在语义匹配中，长文本的多方面的内容过度压缩，变得四不像，以及噪音的影响。比如下面的例子，doc2可能更好的满足query，但是由于其更长导致语义中“ABC”的部分并不能充分的表达，反而匹配上了更短的doc2。
query：ABC
doc1：xxxxxABDxxxxx
doc2：xxxxxxxxxABCxxxxxxxxxxx


字面匹配的方式比较直接，受到长短问题的影响还相对较小，影响较大的还是语义匹配。一些已有的解决方法比如：


* 对长文本进行切割，再分别匹配。
* 在更细粒度上进行文本间的匹配，比如交互式 / 迟交互的模型（ColBERT）、递归的语义表示。
* 匹配强化对关键实习的学习，比如keyword\-attentive\[6]。
* 字面匹配和语义匹配相结合。这也算业务中常用的手段了。


以上方法能解决一部分问题，但现实的情况往往很复杂，就语义匹配而言，本质还是学习高质量的语义向量，怎么学习一个**高质量的语义模型**。理想的情况下，期望模型能够理解文本，具有**推理能力**，能够兼具字面匹配和语义匹配，实现正的语义理解，而不是“高级的字面匹配”，或许这也是现在LLM如此吸引人的原因吧。


PS：写完这小结，发现把语义匹配中的一些经典挑战也杂糅进去了...🙂


## 词序感知


区分不了词序变化后的文本，很典型的例子：“她看完电影后吃了晚饭”vs“她吃了晚饭后看完电影”。


字面匹配中有一些方法是考虑了词序的，比如最长公共子串的匹配，不过这种在实际应用中还是比较有限的。目前常用的语义模型虽然引入了位置编码，但实际应用时还是在词序感知上比较弱。


造成语义模型对词序不敏感的原因有很多，比如：获取句子向量的方法、训练策略、数据分布、文本长度等。\[7]中做了详细的介绍，推荐阅读一下。


## 多实体关系


query较长、描述了多个实体之间的关系时容易出现这种问题，这种情况要求匹配时能够理解文本中所描述的复杂关系，能够区分较细致的区别，例如“中国咖啡豆供应了哪些咖啡品牌”。


字面匹配在处理这种情况时，通常会陷入“原文查找”的地步。然而针对复杂的query，通常不只是以原文的形式出现，更多的需要对文本进行理解。我们期望语义匹配能解决这样的问题。


# 总结


本文简单谈了一下笔者关于文本匹配的一些认识，主要以字面匹配和语义匹配的角度为中心。这里没有涉及具体的匹配方法，主要是关于文本匹配的一些理解，以及当前的一些挑战和难点。


文本匹配是一个比较具体的任务，离实际的业务比较近，在搜广推都中都有非常广泛的应用，面临的问题也比较具象和琐碎。从“上古”的纯字面匹配，到词向量，再到基于语言模型的语义匹配，文本匹配已经基本完成字面匹配到语义匹配的转换，但仍然有一些语义匹配不能覆盖的点。就语义匹配而言，本文提到的一些难点本质上是语义向量质量的问题，其中最重要的一项就是模型在匹配时具有推理能力。期待大模型能助力语义匹配更上一个台阶。


# 参考


\[1] From Frequency to Meaning: Vector Space Models of Semantics, 2010\.


\[2] [美团搜索中查询改写技术的探索与实践](https://github.com).


\[3] [RMIB: Representation Matching Information Bottleneck for Matching Text Representations, ICML 2024](https://github.com).


\[4] [Introducing Natural Language Search for Podcast Episodes](https://github.com):[樱花宇宙官网](https://yzygzn.com).


\[5] A Deep Relevance Matching Model for Ad\-hoc Retrieval，2016 CIKM.


\[6] Keyword\-Attentive Deep Semantic Matching，2020\.


\[7] [向量模型的词序感知缺陷与优化策略](https://github.com)，2024 Jina AI.




---


**2024 \=\=\> 2025！**
[![](https://img2024.cnblogs.com/blog/2338485/202501/2338485-20250104103837427-765390603.jpg)](https://img2024.cnblogs.com/blog/2338485/202501/2338485-20250104103837427-765390603.jpg)


  * [字面匹配 \& 语义匹配](#%E5%AD%97%E9%9D%A2%E5%8C%B9%E9%85%8D--%E8%AF%AD%E4%B9%89%E5%8C%B9%E9%85%8D)
* [字面匹配](#%E5%AD%97%E9%9D%A2%E5%8C%B9%E9%85%8D)
* [语义匹配](#%E8%AF%AD%E4%B9%89%E5%8C%B9%E9%85%8D)
* [小结](#%E5%B0%8F%E7%BB%93)
* [一些挑战](#%E4%B8%80%E4%BA%9B%E6%8C%91%E6%88%98)
* [长短困境](#%E9%95%BF%E7%9F%AD%E5%9B%B0%E5%A2%83)
* [词序感知](#%E8%AF%8D%E5%BA%8F%E6%84%9F%E7%9F%A5)
* [多实体关系](#%E5%A4%9A%E5%AE%9E%E4%BD%93%E5%85%B3%E7%B3%BB)
* [总结](#%E6%80%BB%E7%BB%93)
* [参考](#%E5%8F%82%E8%80%83)

   \_\_EOF\_\_

   ![](https://github.com/gzyatcnblogs)Milkha  - **本文链接：** [https://github.com/gzyatcnblogs/p/18651660](https://github.com)
 - **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://github.com)我。
 - **版权声明：** 本博客所有文章除特别声明外，均采用 [BY\-NC\-SA](https://github.com "BY-NC-SA") 许可协议。转载请注明出处！
 - **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。
     
