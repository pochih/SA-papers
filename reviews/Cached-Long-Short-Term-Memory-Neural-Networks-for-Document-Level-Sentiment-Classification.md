## Cached Long Short-Term Memory Neural Networks for Document-Level Sentiment Classification

Jiacheng Xu, Danlu Chen, Xipeng Qiu, Xuanjing Huang, EMNLP, 2016

### Summary
- LSTM background
  - 在 LSTM 中有記憶體 C，以及三個控制記憶體的 gates: input gate, forget gate, output gate
  - 在之前的研究也指出，LSTM 雖然比 RNN 更能記住長期資訊，但面對 document-level 的長度還是有所不足 (畢竟 timestep 多，梯度就會越來越小)
- 為了彌補 LSTM 的不足，related work 中有論文對 LSTM 做了一些改動
  - 在之前的研究中發現 input gate & forget gate 同時作用會造成一些 redundant，因此把兩者合併成一個 gate，讓兩個 gate 相加等於 1，只要算出一個，另一個就能直接得出 (也就是 GRU 的概念，不過在 paper 中作者把它叫做 CIFG-LSTM)
  - 先前有人嘗試把 LSTM memory 分成數個 group，每個 group 更新不同次數，例如每兩個 input 才更新一次 (變相讓某些 group 的 time step 變少)。但作者認為此舉會讓 group 間缺乏互動。
- proposed method (cached LSTM)
  - 作者的思路是，要讓模型同時保有短期記憶與長期記憶的特質。
  - 作者觀察到 forget gate 如果很大的時候，之前的資訊會被 drop 掉很多，變得比較像短期記憶，在長文本中會讓梯度傳不回去
  - 因此讓 LSTM 中每個部分有不同 forget gate 就行了
  - 一樣把 LSTM memory 分成數個 group，但每個 group 給他不同程度的 forget rate，讓每個 group 的 forget gate bound 在 [k-1/N, k/N] 之間 (k 是 group 數，N 是 hyper-parameter)
  - 可以保證第 k 個 group 其 forget gate 必定不小於第 k-1 個 group
  - forget gate 小的 group，負責記憶長期資訊，資訊更新比較慢
  - forget gate 大的 group，負責記憶短期資訊，資訊更新比較快
  - 要注意的地方是，在最後一個 timestep 中，作者只會取 forget gate 最小的那個 group，因為包含最多長期的資訊
- 實驗
  - 作者比較了 vanilla RNN, vanilla LSTM, CIFG-LSTM, cached LSTM
  - cached LSTM 收斂最快，證明了本篇的方法的確能幫助梯度傳遞
  - 雙向的版本總是比單向好
  - 作者還比較了各種模型在不同長度文本的準確度，cached LSTM 在長短文本表現都明顯較好

### Strengths / Novelties
- 作者很仔細的觀察 LSTM 中每個結構，透過對 forget gate 的控制，得到很容易實作的加強版 LSTM
- 本篇的做法相當於把 LSTM 的 gates 向量化，而不是原本的 scalar，這樣就能給不同 element 不同的值
- cached LSTM 是一個能運用在非常多任務的概念，不知道為何 citation 這麼少?

### Notes
- 實驗中用的是細粒度的情感分類。個人感覺把文本分成 1~10 分，有點太細了。一般人其實很難指出 7 分跟 8 分的差別。假如模型預測某篇文本是 7 分，而實際上是 8 或 6 分，我都會覺得模型已經蠻準的。蠻想知道在能容忍正負一的情況下，模型的準確率會是多少。
- 好在作者在分類準確度外也有衡量 MSE，通常分類準確率高的，MSE 也會低，10-ways classification 情況下，準確率高跟 MSE 小這兩個事件還算是正相關。