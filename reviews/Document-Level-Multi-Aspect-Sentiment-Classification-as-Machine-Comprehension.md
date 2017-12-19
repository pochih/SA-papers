## Document-Level Multi-Aspect Sentiment Classification as Machine Comprehension

Yichun Yin, Yangqiu Song, Ming Zhang, EMNLP, 2017

### Summary
- 把 sentiment analysis 當做 reading comprehension 問題來解。會先手工選擇跟 aspect 有關的 keyword，接著把每個 keyword 都當成一個 question，去算 document 跟 keyword 的關係
- 會把 document 跟 keyword (question) 一起丟進 model，model 中包含 word-level & sentence-level，兩個 level 都有 encode 以及 interactive attention 兩階段
  - word-level encode
    - sentence 每個字的 word embedding 丟進 bi-LSTM，把 hidden state stack 起來得到 doc representation for word (Mdw)
    - question 每個字的 word embedding stack 起來丟進 dense 得到 question representation for word (Mqw)
    - 注意 word embedding for document 跟 for question 都是 pre-trained 好的值當起點，但後面是分開 update 
  - word-level interactive attention
    - Mdw 與 Mqw 會交互做 attention。首先用 select vector P 與 Mqw 得到第一版的 summary for question Q，用 Q 更新 P (不過在這邊直接讓 P = Q)，再用更新過的 P 與 Mdw 得到第一版的 summary for doc D，用 D 更新 P (在這邊直接讓 P = D)，再用 P, Mqw 去得到第二版的 Q，以此類推。
    - 把每個版本的 D concat 起來，得到 sentence representation
  - sent-level encode
    - 把剛才得到的 sentence representation 丟進 bi-LSTM，把 hidden state stack 起來得到 doc representation for sent (Mds)
    - question 每個字的 word embedding stack 起來丟進 dense 得到 question representation for sent (Mqs)，這裡的 dense 跟 word-level 是不同個
  - sent-level interactive attention
    - 跟 word-level 用同樣的方式，select vector P 會分別去看 Mds 以及 Mqs，並 update P

### Strengths/novelties
- 把 sentiment analysis 當成 reading comprehension 問題來解，可以套用 reading comprehension 的架構

### Weaknesses
- 需要手工去選每個 aspect 對應的 keyword，如果 aspect 很多，就沒辦法用本篇的方法。況且 keyword 的挑選沒有一定標準
- 實驗沒做在常見的 Twitter dataset 上，有點可惜。
- 文中沒特別說 select vector P 怎麼 initial，attention 機制的好壞很大程度取決於一開始 P 的值。如果是 random initial，感覺不會比用 target word 去 weight sentence 的方法好 (ex: 用 target 來對 sentence attention 或 memory network)。