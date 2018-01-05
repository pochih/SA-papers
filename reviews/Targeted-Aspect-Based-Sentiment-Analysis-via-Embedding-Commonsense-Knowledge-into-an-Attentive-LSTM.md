## Targeted Aspect-Based Sentiment Analysis via Embedding Commonsense Knowledge into an Attentive LSTM

Yukun Ma, Haiyun Peng, Erik Cambria, AAAI, 2018

### Summary
- 本篇做的是 targeted aspect-based sentiment analysis，在給定句子中，會有幾個地方被標註成 target word，你必須要分析 target word 在句中的情感，以及分析 target word 帶有的 aspect 
	- 情感包括 positive/neutral/negative/None 四種
	- aspect 根據 dataset 而有不同，一般來說不會太多。例如分析 'WestLondon' 這個 target word 的 'general 傾向'，以及 'safety 傾向')
- 賣點在於用 commonsense knowledge 來輔助純文字的情感分析
- 基本的模型使用雙向 LSTM，並加上 attention
	- target attention: 首先對 target word 的 hidden state 做 attention，得出 target attention vector (終於有 paper 不是取平均了) 
	- sentence attention: 接著把 target attention vector 與所有 hidden state concat 起來，做 attention，得到 sentence vector
	- classifier: 用 sentence vector 經過 dense layer 以及 softmax 來算 cross-entropy
- 在基本模型以外，會加上 Commonsense knowledge。這部分使用了名為 SenticNet 的工具，它包含 50000+ 種 concept，每種 concept 都有多種特性的分數
	- 例如 "win lottery" 這個 concept，在 "Arises-joy" 這個特性的分數很高，在 "KindOf-food" 這個特性的分數很低

	 | concept | KindOf-food | Arises-joy | IsA-pet
	 | ------- | ----------- | ---------- | -------
	 | dog     | 0           | 0.789      | 0.981
	 | cupcake | 0.922       | 0.910      | 0
	 | win lottery | 0       | 0.991      | 0
        
	- 由於 SenticNet 中的 concept 矩陣非常 sparse，很難直接與 NN 結合，因此在 AffectiveSpace (Cambria et al. 2015) 這篇 paper 找出了一個 low-dimension 且保留資訊的表示法，本篇直接把該篇 reduce dimension 後的東西拿來用，因此每個 concept 都能用一個低維的 vector 來表示
    
	- 最直接把 commonsense knowledge 加進基本模型的方式，就是把 concept vector 跟 input sentence 一起丟進 LSTM
	- 此外，本篇還在 LSTM cell 中增加一個 concept gate，用來控制現在的 hidden state 要包含多少 concept vector 資訊
- 實驗做在 SentiHood 以及 SemEval2015 上
	- 在 SentiHood 上，直接把 commonsense knowledge 加上 sentence 丟進 LSTM 就能有非常好的提升，跟加上 concept gate 的效果不相上下
	- 在 SemEval2015 上，concept gate 效果最好
	- word embedding 使用 Amazon review 以及 Yelp 來 pretrain

### Strengths / Novelties
- commonsense knowledge 簡單而強大，即使直接當成額外 feature，跟 text 一起丟進 LSTM，都會有很好效果。在 deep learning 興起後，傳統語言學知識被淡忘。但最近 NLP 的趨勢，是把新舊結合，達到更好的 performance
- 終於有 paper 對 target word 做了額外的處理，而不是直接取 word embedding 平均或是 hidden state 平均

### Weaknesses / Notes
- 在做 sentence attention 時，target 自身的 hidden state 也會跟 target attention vector concat 起來，會不會有點重複考慮？
- 文中的實驗有使用 deep memory network 來替代 sentence attention，效果更差。作者的解釋是 deep memory network 參數太多，但實際上 deep memory network 每一個 hop 是 share weights 的，效果不好不會是參數多
- 在多數時候，直接把 concept vector 加上 text 就能有很好效果。決定效能高低的關鍵之處，似乎在於怎麼將 high-dimension 且 sparse 的 concept vector 降維成 low-dimension。因此模型的強大之處與困難點，在於 concept vector 的學習，而不是本篇提出的架構
