## Attention-based LSTM for Aspect-level Sentiment Classification

Yequan Wang, Minlie Huang, Li Zhao, Xiaoyan Zhu, EMNLP, 2016

### Summary
- 很多 paper 在 model aspect word 時，都是直接對 aspect word 所有的 hidden state 取平均。作者覺得直接取平均不好，因此額外用了一個 aspect embedding matrix，用 aspect embedding 來表示每個 aspect
- 作者提出三種 model
  1. AE-LSTM
    - 沒有詳細寫怎麼做，可能是把 input 丟進 LSTM 之後，把最後一個 hidden state 跟 aspect embedding 串在一起丟進 classifier 吧?
  2. AT-LSTM
    - 把 input 丟進 LSTM，接著把 aspect embedding 跟所有 hidden state 串在一起做 attention
    - 要注意的地方是，雖然 attention 是 hidden state + aspect embedding 一起算的，但算出來的 weight 只會作用在 hidden state 上，得到 hidden state 的 weighted sum
  3. ATAE-LSTM
    - 作者說 AT-LSTM 的 hidden state 並不會帶有 aspect 的資訊，因此這個方法是在 input 階段就把 word embedding 跟 aspect embedding 串在一起，丟進 LSTM，然後再做 attention。attention 的算法跟 AT-LSTM 一樣
- *一個 trick 是經由 attention 得到的 hidden state weighted sum，跟 last hidden state 一起丟進 classifier 會更好*
- 實驗結果 ATAE-LSTM > AT-LSTM > AE-LSTM

### Strengths/novelties
- 本篇給 aspect 建了專屬的 aspect embedding matrix，是第一篇提出這個想法的。有另一篇論文是額外建一個 word embedding，aspect embedding 是從這個 word embedding 得到的。這個 word embedding 跟原本的 word embedding 初始值一樣，但分開更新。

### Weaknesses
- 因為在本篇用的 SemEval2014 中，aspect 很少，在 test case 中不會出現沒看過的 aspect。但如果想把本篇的方法擴展到別的 dataset，這會是個無法解決的問題。
