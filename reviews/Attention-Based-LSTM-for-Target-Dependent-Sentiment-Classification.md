## Attention-Based LSTM for Target-Dependent Sentiment Classification

Min Yang, Wenting Tu, Jingxuan Wang, Fei Xu, Xiaojun Chen, AAAI, 2017

### Summary
- 這篇非常短，只有兩頁......
- 架構也並不新穎，就只是對 LSTM 的 hidden state 做 attention，這樣也能上 AAAI？
  - 第一種 attention 方式，是把 hidden state 跟 target vector 做內積之後，丟進 attention network，得到該 hidden state 的權重
    - target vector 沒講怎麼算的，如果是取平均，那效果一定很爛
    - attention network 也沒講，只用一行帶過
  - 第二種 attention 方式，是 bilinear term，比上一個方法多了一個矩陣 W，attention network 的 input 是 hidden state x W x target vector


### Weaknesses / Notes
- 很多地方描述得十分不清楚，performance 也沒有非常高
- 這篇 paper 的質量很糟，讓人懷疑 AAAI 的選擇標準