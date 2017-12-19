## Gated Neural Networks for Targeted Sentiment Analysis

Meishan Zhang and Yue Zhang and Duy-Tin Vo, AAAI, 2016

### Summary
- 在二作、三作前一篇論文 [Target-Dependent Twitter Sentiment
Classification with Rich Automatic Features](https://www.ijcai.org/Proceedings/15/Papers/194.pdf) 中，沒好好利用 target & context 之間的關係。因此本作著眼點在此。
- 利用 RNN 來 model sentence，並使用 gate 來判斷左 context, 右 context, 左+右 context 的比重。
- gate 是利用 context 自身的 hidden 跟 target hidden 綜合算出的
- context hidden 跟 target hidden 是對 context / target 做 pooling 得到，paper 並沒有寫很清楚怎麼做 pooling

### Strengths/novelties
- 比前作更好地利用 target 的訊息

### Weaknesses
- 感覺用 gate 來控制 input 的部分做得有些複雜了，用 LSTM 加 attention 的架構能更容易的選擇每個 input 的權重，也不用像 paper 一樣每個 connection 都加一個 gate
- 沒寫如何對 hidden state 做 pooling (如果跟前作用一樣的 pooling 方式，那就是對 hidden state 做 max, min, avg, std, product 五種 pooling)




