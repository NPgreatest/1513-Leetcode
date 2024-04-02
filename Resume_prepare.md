### Seq2Label

* 编码器

Token：先将每个词提取token，将每个token转换为字典中的数字。

Embedding：将浓缩的评论输入到embedding中，随后将多个评论flatten到一维数组，

GRU：通过双向GRU进行处理，获得时间序列特征。将双向GRU的输出进行合并（相加），

Dropout：通过dropout层避免过拟合

* 解码器

传入目标向量：将目标向量词嵌入后经过dropout和GRU处理，并且引入隐藏层。

计算注意力权重：通过当前的GRU输出和解码器的输出计算注意力权重，进行加权平均(获得上下文向量)

向量拼接：将GRU的输出和上下文向量拼接，转化为最终的结果

最终计算带有掩码的交叉熵损失loss，计算梯度下降，进行反向传播

### 股票预测：交叉验证，回测

采用了净化-隔离交叉验证，隔离掉可能对验证集造成信息泄露的数据。

采用了历史型单路径回测算法(HSP)，假设模型在曾经的历史数据中执行一遍，假设历史重演时模型的表现效果。并且规避使用后视数据

### 协同过滤

首先利用余弦相似度计算用户和其它用户的相似度。$\text{cosine similarity} = \frac{\mathbf{A} \cdot \mathbf{B}}{|\mathbf{A}| \cdot |\mathbf{B}|}$

首先利用基于用户的协同过滤：利用最近邻算法筛选出N个用户，并且给用户推荐自己没有浏览过的k个景点。

当用户选择这个景点之后，再利用item-based的协同过滤，给用户推荐这个物品最相似的k个景点



**矩阵分解算法则是期望为每一个用户和视频生成一个隐向量**，将用户和视频定位到隐向量的表示空间上

实际上协同过滤就是一个embedding的过程，通过矩阵分解可以提取出用户和物品的嵌入向量，再进行向量化搜索。



### LSTM 股票

解决了RNN长时间依赖的梯度爆炸的问题，从序列输入到单个输出

遗忘门($f_i$点乘)；输入门(相加)；输出门($C_t$到下一层)；隐藏状态($h_t$传递给下一层)



- **特征提取**：CNN通过卷积层捕捉市场数据中的局部特征，而Bi-LSTM则捕捉时间序列的长期依赖性，即如何根据过去的市场行为来预测未来。
- **注意力机制**：通过强调重要的时间步，使模型能够在做出预测时更加关注于重要的市场事件或指标，从而提高预测的准确性。
- **量化投资策略**：模型输出的预测可以用于制定投资策略，比如预测股价上涨的概率，从而指导买卖决策。

相比LSTM，GRU更简洁，只用一个门控即可进行遗忘和选择记忆



Adam优化器：动态调整每个参数的学习率





### 自我介绍-EN

Hello, my name is Liu Yuchen. I'm currently pursuing dual degrees in Computer Science and Finance at East China University of Science and Technology. This coming September, I'll be enrolling in a Master's program in Computer Science at Rice University. It's an honor to be here for this interview.

Previously, I had the opportunity to intern at Bilibili for four months, where I engaged in backend development using Go. My responsibilities included maintaining and updating the company's microservices framework.

In terms of deep learning, my undergraduate thesis involved developing a Seq2Seq model using a Bidirectional GRU approach, as well as developing applications for the RAG large language model using the langchain framework. Moreover, I've been self-studying deep learning recently. I believe my background aligns well with the position at your company and I'm eager to contribute my skills and learnings.

