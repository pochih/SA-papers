## Deep Sentence Embedding Using Long Short-Term Memory Networks: Analysis and Application to Information Retrieval

Hamid Palangi, Li Deng, Yelong Shen, Jianfeng Gao, Xiaodong He, Jianshu Chen, Xinying Song, Rabab Ward, IEEE/ACM Transactions on Audio, Speech, and Language Processing, 2016

### Summary
- 在 searching 時，會得到很多 query，以及搜尋引擎回應的數個 documents。
- 把 user 點擊的 document 當成 relative，其餘的當成 unrelative。本篇用這個資訊來做 weakly-supervised learning。
- 每個 query 以及 document 都用一個 vector 來代表。希望 (query, clicked document) 的 vector 接近，(query, unclicked document) 的 vector 拉遠
- 用 LSTM 來抽 sentence / document feature。用 negative sampling 來 train。使用logistic loss
- 實驗
  - 可視化輸入 query 後，LSTM cell 對 input sequence 的 activation
    - 發現 LSTM 中的 y 和 c 會隨著 sequence 的 encode 逐漸變強
    - 而 LSTM 中的 input gate 會忽略不重要的字
  - 作者另外做了 keyword 實驗，想證明本篇的做法是有效的。作法如下：
    - 把出現大 activation 的 cell 當作重要 cell
    - 如果多數重要 cell 對某個 word 有反應，就把它當作 keyword (要雙向都有反應才算)
    - 實驗證明的確有抓到 keyword
    - 把每個 query activation 最大的 5 個 cell 列出來，發現不同 cell 會 focus 在不同主題 (仔細想想還頗合理)。這招可以用來驗證現在的 cell 到底是太多還是不足。許多 cell 學到類似概念就表示 cell 太多了，容易 overfitting。

### Strengths / Novelties
- 把 sentence / document modeling 拿來做 IR，是個有趣方向 (但說不定很多人在做了)
- 可視化實驗以及 keyword 實驗做的很有趣。
