## Context-Sensitive Lexicon Features for Neural Sentiment Analysis

Zhiyang Teng, Duy-Tin Vo, Yue Zhang, EMNLP, 2016

### Summary
- 作者認為情感字典中的字 (sentiment lexicon words)，會根據不同 context 而有不同的極性，極性並不固定
- 模型主體用兩個 GRU (一正一反) 來 encode 句子，但只算有出現在情感字典中的字的 hidden state
- 在本篇的模型中，一個文章的情感分數會是 λ * score(sentiment lexicon words) + (1-λ) * score(bias)
- 求 score(sentiment lexicon words)
  - 每個 sentiment lexicon word 會把這個字對應到的 GRU hidden state 拿出來，過兩層 NN，得到一個 [-2, 2] 的值，這個值就是該 sentiment lexicon word 在此句中的極性。作者有提到把輸出變成 [-2, 2] 是為了之後的五分類任務
  - 用 GRU 得到的極性會乘以該 sentiment lexicon word 在情感字典中的分數，來得到這個 sentiment lexicon word 最終的分數 (例如 good 這個字在情感字典是 +3，在 GRU 是 -0.5，good 在此文章中的分數就是 -1.5)
  - 如果句中有 m 個 sentiment lexicon words，會取其平均
- 求 score(bias)
  - bias 分數是 concat(正向 GRU 的最後一個 hidden state, 反向 GRU 的最後一個 hidden state) 再通過兩層 NN 算出來的
- λ 不是固定值，會根據不同的輸入而改變。λ 的求法是把 sentiment lexicon word 的 hidden state 以及兩個方向 GRU 的 last hidden state 綜合考慮求出來的

### Strengths / Novelties
- 一般 NN 結合 sentiment lexicon 的方式，都是把兩者得到的 feature concat 在一起，然後丟進 classifier
- 這篇從不同角度去思考，只考慮情感辭典的 hidden state，並根據不同 context 算出不同分數。

### Weaknesses / Notes
- 如果句中沒包含任何情感字典中的字，等於只看 score(bias)，不知道這樣結果會不會壞掉
- 在 Twitter 這種詞彙很豐富的論壇，會出現更多不在情感字典中的字，在這種情況下本篇的方法可能不 work
