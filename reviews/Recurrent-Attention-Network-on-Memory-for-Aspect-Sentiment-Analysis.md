## Recurrent Attention Network on Memory for Aspect Sentiment Analysis

Peng Chen, Zhongqian Sun, Lidong Bing, Wei Yang, EMNLP, 2017

### Summary
- 這篇的賣點，是對 LSTM 的產物做多次 attention
- 本篇的任務，是要預測句子中某一個 target word 的情感
- 模型主體
  - 模型主體是一個雙向 LSTM，每個 input 中的 word 都會對應到一個 hidden state
  - 作者認為離 target word 越遠的字，對 target word 的影響就越小，因此每個 hidden state 都會乘上一個 scalar，距離 target word 越近，scalar 越大
- recurrent attention
  - 接著對這些 hidden state 做 attention，不過不像一般的模型只做一次 attention，本篇的方法會有多次 attention，跟 memory network 的概念很像，每次 attention 專注的點會不一樣
  - 首先以一個全是 0 的向量當成 attention 的 query，來對所有 hidden state 做 attention
  - 一次 attention 後會得到一個 hidden state 的 weighted sum，把當下的 query 以及 weighted sum 丟進 GRU，得到下一輪的 query
  - 在 memory network 中也會有複數次 attention，只是算下一輪 query 的方式，是用線性的方式去算，這裡則是用一個 GRU 去算，作者認為這樣的非線性算法，可以有更強的表述力
  - 做完多次 attention 後，把最後一次得到的 query 當成 feature，丟進 softmax classifier

### Strengths / Novelties
- 我之前也有想過在 LSTM 之上做複數次 attention，不過算下一輪 query 的方法是用 memory network 的方式 (線性)。本篇用 GRU (非線性) 來算 query，頗為巧妙

### Weaknesses / Notes
- 要做幾次 attention 是一個 hyper-parameter，是個固定值。但不同的 input 所包含的資訊豐富度不一樣，資訊量高的可以做更多次 attention，反之則應該做更少次 attention。需要一個能自動決定何時停止 attention 的機制。
- 本篇會把每個 hidden state 做加權，加權方式是該 word 離 target 的距離。這樣簡單粗暴的加權，會把一些離 target 較遠但重要的訊息忽略了。
