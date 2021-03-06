# Description Based Text Classification with Reinforcement Learning

##### 发布时间：4 Jun 2020

---

- **问题：** 传统的文本分类分为两个步骤，特征提取和分类。
  
- **不足：** 传统方法只是将分类标签当成简单的标签，没有对于分类的具体描述。模型并不知道分类的描述。
- **改进：** 将类别和类别的描述信息结合。

- **问题：** 如何生成描述信息？

- **传统方法：** 人工模板

- **不足：** 费力，模型对于描述是如何构建的很敏感，人工模板不是很好的选择。
- **改进：** 通过强化学习来自动生成描述信息，（1）直接截取，（2）抽象抽取

- **补充：** 将NLP问题看成QA问题来处理。
  
- **三种生成描述的策略：** 
  使用强化学习来让模型达到更好的表现。强化学习有三个要素：Action $a$, Policy $π$, Reward $R$.
  - *模板：* 最直接生成标签描述的就是人工模板，模板的来源有很多，比如Wikipedia或者人工标注。
  - *截取：* 
    - 从$x$中抽取字串$q_{yx}$作为标签$y$的描述信息。当$x$中没有对应的字串可以选择时就从人工模板中选择作为描述。
  - *抽象：*

- **使用的数据集：**
  - *单标签分类：* AGNews,20newsgroups,DBPedia,Yahoo!,Answers,Yelp Review Polarity, IMDB.
  
  - *多标签分类：* Reuters,AAPD
  
  - *多方面情感分析：* BeerAdvocate, TripAdvisor



## Generative and Descriminative Text Classification with Recurrent Neural Networks

##### 发布时间：26 May 2017

---

- **不足:** 神经网络需要大量的数据并且当数据分布发生变化时表现就不行了。

- **发现：** 虽然以RNN为基础的判别模型比以RNN为基础的生成模型具有更高的渐进错误率，但是生成模型比判别模型收敛的更快。   所以，文章作者假设并发现了RNN为基础的生成模型对数据分布发生变化更具有健壮性。

- **解决方法：** 使用生成模型在复杂样本中取得提升，并提升适应数据变化的能力。

- **实验：** 使用单任务持续学习(标签是顺序给出的，在训练过程中会有新的标签出现，使得标签的分布发生变化)

- **实验结果：**

  - *判别模型：* 对之前的信息遗忘的很快（模型需要再次训练来适应新引用的标签，从而遗失了之前训练的有用信息）。
  
    - 推断其原因是因为，引入了新的标签后，模型需要调整其它的参数来使得预测新类型的可能尽可能地大。 这样会导致在原有标签分布下训练出的参数产生较大的变化。使得之前训练的信息丢失。
  
    > ​				注意：理论上来说是可以找到一个刚刚好的学习率来避免这种遗忘的，不过得到这个学习率需要对新引入的类有一个很好的‘了解’。  一个有希望的方法是弹性权重融合，不过这种方法需要计算Fisher信息矩阵。这对于复杂的矩阵很困难。
  
  - *生成模型：* 对于这种变化适应能力更强，能够更轻松的将新标签和其它标签分辨开。
  
    - 对于生成模型，每次当有一个新标签引入的时候只需要争对新的标签学习一个新的模型。	
    
      <u>关于生成模型，还看的不是很明白</u>
    



# Integrating Semantic Knowledge to Tackle Zero-shot Text Classification

##### 发布时间：29 Mar 2019   CODE: https://github.com/JingqingZ/KG4ZeroShotText

---

- **问题：** 很多分类方法只在传统的分类任务上有效，对于会出现新的类别的动态，开放的环境就不怎么行了。  
1. *零样本学习：* 零样本学习就是描述这样的场景

2. 零样本学习需要‘把握’有用的语义信息 （比如，类别的描述，类别之间的关系，和外部领域的知识）---利用已有的类的知识去理解未见过的类。

3. 目前在零样本学习中有三种常用的方法，但是目前很少有人去扩展他们，甚至没有人考虑将它们融合起来。并且之前的一些工作，他们使用的训练集和测试集的类别有一些相似性。

    | 三种常用方法                                    |     |
    | ----------------------------------------------- | --- |
    | 视觉概念（颜色，形状）和语义特征（动作，行为）  |     |
    | 类层级关系和知识图谱  ---表示类和特征之间的关系 |     |
    | 词嵌入--通过大量的语料库训练得到单词之间的关系  |     |

- **解决方法：** 作者提出了一种两阶段框架去共同进行数据增强和特征增强。其中融合了词嵌入，类别描述，类层级关系和知识图谱四种语义知识来帮助认识新出现的分类。    

  - 两个阶段都是基于CNN，第一个阶段叫做粗粒度分类，用于判断记录是出现过的还是未出现过的。第二个阶段叫做细粒度分析，用于最终得到记录的类别。
  - 框架中所有的分类器都只使用带有标记的出现过的类进行训练。

- **主要贡献：**
  
  - 提出了一个两阶段框架来处理零样本问题，框架不需要新出现的类别和已有的类别有相似性。
  - 提出了一个新的数据增强技术，叫做话题翻译（topic translatioon)。
- 提出了一个新的使用整体语义信息去进行特征增强的方法。                                                                                                                                                        
- **模型概述：**

  - 粗粒度分类：预测记录是来自出现过的类别还是未出现过的类别
    - 这里使用了一种数据增强方法来帮助分类器注意到未出现过的类别并且不接触它们真实的数据。

    - 使用CNN来预测记录属于每个已知分类的可能性，损失函数使用交叉熵损失函数，训练集中出现的所有类别作为正类，其余的作为反类。当有一个已知分类的可能性大于t时就说明记录属于已知分类。t是通过[Shu et al.,2017](https://arxiv.org/pdf/1709.08716.pdf)中的门限适应算法得到的。
    
    - <u>这里为什么要使用数据增强的原因我有点看没懂</u>，论文中说的是，在粗分类阶段，分类器只是用可见类中的负样本，所以他们可能无法区分出未见类中的正样本。所以当未见类在上述阶段中出现时，就尝试通过数据增强向分类器“介绍”它们。以便分类器能够拒绝相似的未见类样例。
    - 数据增强的方法是利用类比的方法将源已见类记录翻译的一个未见类记录。

  - 细粒度分类：确定记录的最终类别

    - 在第一阶段获得的粗粒度预测上使用传统分类器或者零样本分类器，这里会使用基于语义知识的特征增强来提供将记录和未出现的类别联系起来的额外信息来概括零样本推断。
- **相关工作：**
    零样本分类，NLP中的数据增强，NLP中的特征增强，联结和特征工作。


# Aspect Sentiment Classification Towards Question-Answering with Reinforced Bidirectional Attention Network

##### 发布时间：25 Sep 2017
---

- **问题：** 现有的方面及情感分类（ASC,Aspect sentiment classification)都是专注于非交互式评论。最近几年在各大论坛和电商平台都出现了交互式的评论，所以作者提出了一种新的研究任务（ASC-QA,Aspect sentiment classification towards Quetion-Answering)
    | Questin-Answering Style Review |Aspect Sentiment Classification Towards QA|
    | --- | --- |
    | **Question:**  Is [battery life] durable? How about [operating speed] of the phone? | **Input:**  QA text pair with given aspects|  
    | **Answer:**  Yes, very <font color=green>durable</font> but <font color=red>quite</font> slow and <font color=red>obtuse</font>. |**Output:**  [battery life]: Positive [operating speed]: Negative|
-  **困难：** 
   -  不像传统的非交互式评论是一个序列结构，问答风格的数据包含两个部分，问题和答案。
   -  和传统的QA问题不同，ASC-QA的目标是从一个特殊的方面抽取语义信息，这可能会受到很多与该方面无关的噪声信息影响。 在上面表格中，对于耐用性，quite和obtuse就是噪声。
- **结论：** 一个好的ASC-QA方法应该尽量缓解模型训练过程中噪声词对于问题和答案的影响。
- **解决方法：** 使用强化双向注意力网络来缓解上面两个问题。 
  - 首先作者提出了一个单词选择模型，叫做强化方面相关单词选择器（RAWS,Reinforced Aspect-relevant Word Selector),通过丢弃噪声词，只从单词序列中选择方面相关的单词来缓解噪声词的影响。
  - 在RAWS的基础上，作者又提出了强化双向注意力网络（RBAN,Reinforced Bidirectional Attention Network),它使用了两部分RAWS模块去分别调整问题和答案中的单词选择。 通过这种方式，RBAN不仅可以从QA文本中选择出语义匹配问题，还可以同时缓解问题和答案中的噪声词的影响。 
  - 最后作者使用了强化学习算法和policy gradient来调整RBAN。
-  **数据准备：** 作者对方面进行了两种粒度的定义:aspect term 和 aspect category。作者还定义了三种情感:positive, negative 和 neutral。 这样每个QA文本对都被标记成了两组。(Aspect term, Polarity) 和 (Aspect category, Polarity)
   -  (Aspect term, Polarity): 根据下面四个准则来判断是否对方面项进行标注。
      -  只有当问题和回答相匹配时才对方面项进行标注。例如对于表中数据，只标注(battery life, positive),(opterating speed, negative)
      -  只有当方面项被表达的时候才进行标注。 例如：**Q:** How is this phone? How about the case? **A:** I bought this phone yesterday. Case is
      okay nothing great.   这里只会标注(case, nuetral)
      - 只有当方面项被直接指明时才会进行标注。 例如：**Q:** Is this expensive? Did anybody buy one? **A:** Of course, it’s quite expensive.  this，it这样的不会进行标注。
      - 当方面项在问题和答案中有两种不同的描述时应和问题中一致。 例如：**Q:** Is battery life durable? **A:** Yes, this battery is very durable. 这里的标注应该为(battery life,durable)
    - (Aspect Category, Polarity)： 首先像下表一样对每个领域定义一些分类。
      | Domains | Aspect Categories |
      | -- | -------- |
      | Bags | Size, Price, Appearance, Quality, Weight, Certified Products, Smell, Accessories, Material, Life Timer, Style, Workmanship, Color, Stain Resistant, Practicality |
      | Cosmetics | Price, Efficacy, Moisturizing Performance, Certified Products, Adverse Reaction, Exfoliator, Texture, Long Lasting, Smell, Material, Noticeable Color, Quality, Colour, Touch, Skin Whitening, Acne |
      | Electronics | System Performance, Appearance, Battery, Computing (e.g., cpu, gpu, tpu etc.), Certified Products, Quality, IO (e.g., keyboard,screen, etc.), Price, Storage, Function (e.g., touch id, waterproof etc.) |
      - 首先更具相似性准则为每个方面项标注方面类别（类别从预定义好的表中选择）
- **Reinforced Aspect-relevant Word Selector：**
  
  ![avatar](./images/RAWS.jpg)
  
  RAWS实际上是一个“强”注意力机制，因为[Xuet al. (2015)](http://proceedings.mlr.press/v37/xuc15.pdf) 和 [Shen et al. (2018b)](https://arxiv.org/pdf/1801.10296.pdf)中提出的非可区分问题，所以无法通过反向传播进行优化。所以作者采用强化学习和policy gradient来进行优化。
- **Reinforced Bidirectional Attention Network：** 
  
  ![avatar](./images/RBAN.jpg)