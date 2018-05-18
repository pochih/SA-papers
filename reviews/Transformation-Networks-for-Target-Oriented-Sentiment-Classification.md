## Transformation Networks for Target-Oriented Sentiment Classification

Xin Li, Lidong Bing, Wai Lam, Bei Shi, ACL 2018

### Summary
- 本篇是騰訊 AI Lab 的文章，任務是 entity-level sentiment analysis，例如 "The food is good but the service is bad"，對 target "food" 情感為正，對 target "service" 情感為負
- 之前多數的 model，都是把句子丟進 LSTM，然後用 target 的 hidden state 對其 context words 的 hidden states 算 attention score，然後用這個 score 對每個 hidden states 做加權和 (weighted sum)
- 本篇認為之前的 attention 做法，沒有很好的考慮 target 與 context 之間的互動，因此用方法增強他們的互動
- 作法
	1. 首先將句子丟進 BiLSTM，得到 hidden states H
	2. 把 target 的 hidden states 丟進另一個 BiLSTM，得到新的 hidden states Ht
	3. 作者認為 target 對不同的 context 有不同的關注程度，也就是 Ht 中的每個東西對 H 中的每一個東西會有不同的關注強度
	4. j 個 target words 與 i 個 context words 就會算出 j\*i 個關注程度的值，這些值會用來算出新的 context representation H'
	5. 由於新算出來的 H' 與原本 BiLSTM 產生的 H 有不一樣的 mean & variance，因此作者用了 residual network 中的 skip-connection 來輔助。作者試了兩種方法，第一種是直接 H' = H + H'，第二種是算出一個 gate t，然後 H' = t * H + (1-t) * H'
	6. 2~5 的過程可以重複很多次
	7. 經過多次 2~5 之後，用 kernel size=1 的 convolutional filters 來抽 features，每個 conv filter 取 max-pooling，當成最終的 feature
	8. 事實上 convolutional layer 的 input 會先經過位置加權，離 target 越近的字加權越大
- 實驗結果
	- 在三個 datasets 上拿到最好結果
	- "之前的 attention 做法，沒有很好的考慮 target 與 context 之間的互動" 這件事，在高度不規則的 twitter 中比較明顯，但在結構性比較高的 review datasets 上，本篇的 model 沒有贏之前的 attention-based 方法很多

### Strengths / Novelties
- 確實的增加了 target 與 context 之間的互動，在 entity-based 的 tasks 中，這一直是很重要的關鍵
- 本篇考慮了 target words 中的每一個字對 context 的每一個字的關係，比之前用 average of target 的方式好
- 引入 CNN 的方式巧妙，本篇不是要算每個 word representation 的 weighted sum，而是用 CNN 來抽取所有 representation 的 salient part

### Weaknesses / Notes
- 雖然本篇宣稱沒做 attention，但 convolutional max-pooling 本身就是一種 attention 的概念，會 focus 在重要的地方。另外本篇算 H' 的地方也是 attention。
- 本篇與其前作 [RAM](http://www.cs.cmu.edu/~lbing/pub/emnlp17_aspect_sentiment.pdf) 一樣，用 rule-based 方式來算位置的權重，這種方式一直覺得不太好、不夠科學，這個 rule-based 需要根據不同的 dataset 來做修正，很難找到一個 global 的 rule。