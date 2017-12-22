## Neural Sentiment Classification with User and Product Attention

Huimin Chen, Maosong Sun, Cunchao Tu, Yankai Lin, Zhiyuan Liu, EMNLP, 2016

### Summary
- 本篇的賣點，在於做 attention 時不僅考慮了 hidden state，還考慮了 user 以及 product。
- 整體的架構跟 [Hierarchical Attention Network](http://www.cs.cmu.edu/~./hovy/papers/16HLT-hierarchical-attention-networks.pdf) 一樣，包含四個部分
  1. word encoder layer
  2. word attention layer
  3. sentence encoder layer
  4. sentence attention layer
- 如果 user A 評論了商品 B，會把 hidden state 與代表 user A 的 vector 以及代表 product B 的 vector 一起丟進 fully-conected layer，取 tanh，再用 softmax 得到每一個 hidden state 的權重。
- [Learning Semantic Representations of Users and Products
for Document Level Sentiment Classification](http://www.aclweb.org/anthology/P15-1098) 提出類似概念，也是要利用 user/product 資訊。它會去建一個 user-product preference matrix，但會因為某些 user or product 資料太少而學不好，參數也比本篇方法多。
- 實驗的部分比較了 user-product attention 的好處
  - [Hierarchical Attention Network](http://www.cs.cmu.edu/~./hovy/papers/16HLT-hierarchical-attention-networks.pdf) 的 attention 的確會幫助結果，但把 user-product 考慮進去會更強，特別是 user 資訊
  - 可視化 user attention: 不同 user 對同一句子會產生不同的偏好
  - 對 word 做 attention 比對 sentence 做 attention 帶來更多效能提升，或許是因為 sentence-level attention 粒度較大，所帶的語意量過多

### Strengths / Novelties
- 在做 attention 時，把 user 跟 product 的資訊考慮進來，很有趣的想法
- 不同 user 在評分時確實會有不同偏好，實驗也證明了這點

### Weaknesses / Notes
- 實際使用時，可能會遇到不在 training data 中的 user/product。在這種情況下，新的 user vector / product vector，就必須先經過 fine-tuning，直到 user vector / product vector 收斂。Inference 速度慢的問題，跟 [doc2vec](https://arxiv.org/abs/1405.4053) 遇到的問題是一樣的。
- 做 attention 時，如果能把 last hidden state 一起考慮進來會更好 (因為它包含了整句 sentence 的資訊)。
