## Aspect Level Sentiment Classification with Deep Memory Network

Duyu Tang, Bing Qin, Ting Liu, EMNLP, 2016

### Summary
- '[End-To-End Memory Networks](https://arxiv.org/abs/1503.08895)' 這篇 paper 正式的把 memory network 的架構完善，memory network 引起的浪潮，也導致 NIPS 2015 專門為其建立了 RAM workshop (reasoning, attention, memory network)
- memory network vs attention
	- memory net 跟 attention 其實就是同一個概念，都是為了求出 input sequence 的 weighted sum。
	- 根據個人的理解，其中一個差別是兩者的 input。attention 通常是對 RNN 的 hidden state 做 weighted sum。而多數 memory network 的 paper 都是直接對 word embedding 做 weighted sum，而不會先丟進 LSTM。
	- 另一個差異是 memory network 必然會有個 query，會用 query 給 input 分數。而 attention 則不一定會有 query。
	- 在 attention 中，因為是對 RNN hidden state 做 weighted sum，而 RNN 是包含著位置的資訊的。但 memory network 的 input 卻不會有位置信息，因此 memory network 在算每個 input 的權重時，加上位置資訊後的表現幾乎都會提升
- 本文中用了兩種方式去算 input 的 weighted sum
    - content attention: 
    	- 將 input sentence 扣掉 target words，得到 context。把 context 用 word embedding 表示。
    	- target representation 用 target word embedding 的平均來表示。
    	- 每個 hop 都會把 word embedding + target representation 一起丟進 dense，算出 weighted sum。
    	- *這裡的一個 trick 是 weighted sum 會與 target representation 經過一層 dense 後的值相加，當作下一個 hop 的 input*
    - location attention: 除了 content attention，加上位置資訊會讓結果更好。作者比較了四種位置資訊表示法
      	- method1: [End-To-End Memory Networks](https://arxiv.org/abs/1503.08895) 這篇提出的方法
      	- method2: 第一種方法的簡化版本，只取其前半段
      	- method3: word embedding 會加上 location vector，location vector 會跟著 network 一起學習
      	- method4: word embedding 會乘上 sigmoid 過後的 location vector，相當於 input gate 的概念，location vector 一樣用學的
- 實驗
    - hop 多次可以學到更抽象的知識，結果也相對好。跟 end-to-end memory network 結論一致
    - location 資訊對結果有幫助，method2 最容易算，method4 表現最好，但對 gate 的選擇很敏感，拿掉 gate 會掉 5%
    - 作者發現錯誤幾乎都發生在語意不清楚、target 太長 (取 average 會糊掉)、反諷、比較、條件等等 case

### Strengths/novelties
- 普通的 sentiment analysis 沒辦法用 memory network，是因為沒有 query。但 targeted sentiment analysis 跟 aspect-level sentiment analysis，因為有 target/aspect，可以把其當成 query。這篇是第一個把 memory network 用在 targeted sentiment analysis 的論文
- multiple hop 的好處十分強大，以 restaurant 這個類別為例，第一次 hop 準確率是 76%，多次 hop 之後可以到 81%。IJCAI 2017 的論文 [Interactive Attention Networks for Aspect-Level Sentiment Classification](https://arxiv.org/abs/1709.00893) 有對 target 做 attention，準確率為 78%，的確比本篇用平均來的好，但因為只做一次 attention，雖然贏了只做一次 hop 的版本，但輸給多次 hop 的版本
- 以後我們在 attention 時，可以借鑑 multiple hop 的概念。一般對 hidden state 做 attention 時，都只做一次，那為何不做多次一點呢？

### Weaknesses
- target 用 hidden state 取平均並不好，作者自己也說了，錯誤幾乎都發生在 target 長的時候。簡單的做法，是對 target hidden state 做 attention，得到更好的表示法
