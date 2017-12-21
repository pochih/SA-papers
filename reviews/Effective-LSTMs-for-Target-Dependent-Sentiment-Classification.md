## Effective LSTMs for Target-Dependent Sentiment Classification

Duyu Tang, Bing Qin, Xiaocheng Feng, Ting Liu, COLING, 2016

### Summary
- target 的語意會因為上下文而不同，作者使用了三種 model 來驗證這點
    - LSTM
        - 最基本的方法，把 input sequence 丟進 LSTM，取最後 hidden state 做分類
    - TD-LSTM
        - 假設 input sequence 長這樣：W1, W2, ..., T1, T2, ..., Tn, ..., Wn-1, Wn
        - 正向 LSTM 從 sentence 第一個字開始，直到遇見 target 最後一個字 (從 W1, W2, ..., 到 Tn)
        - 反向 LSTM 從 sentence 最後一個字開始，直到遇見 target 的第一個字 (從 Wn, Wn-1, ..., 到 T1)
        - 把兩個 LSTM 最後一個 hidden state 拿出來，丟進 softmax
        - 跟雙向 LSTM 有些差異，雙向 LSTM 的 input，是從 W1 到 Wn，以及從 Wn 到 W1
    - TC-LSTM
        - 跟 TD-LSTM 很像，但每個 input 不只是 word embedding，還會加上 target embedding
        - target embedding 是 target word 的 word embedding 平均值
- 實驗結果
    - TD-LSTM 比 vanilla LSTM 好一點點而已，而 TC-LSTM 在 accuracy 上是 state-of-the-art。但 F1-score 輸給之前一篇手工提取特徵的[Paper](https://www.ijcai.org/Proceedings/15/Papers/194.pdf)
    - word embedding 影響很大，glove 200d >= glove 100d > glove 50d > SSWE
    - 用範例顯示 vanilla LSTM 幾乎都忽略 target 本身的訊息，而後兩個 model 的錯誤幾乎都發生在 neutral class
    - 作者實作了 attention 效果不好


### Weaknesses / Notes
- 對 target 的處理太過於簡陋，取 word embedding 的平均不是很好的做法
- 照理來說 attention 的結果應該會比較好。一個可能性是，對兩個 LSTM 分別做 attention 再 concat 起來，可能會被其中一邊 dominate 掉。concat 前先經過一個 gate，就能避免。