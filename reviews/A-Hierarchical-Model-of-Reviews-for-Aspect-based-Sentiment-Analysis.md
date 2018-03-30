## A Hierarchical Model of Reviews for Aspect-based Sentiment Analysis

Sebastian Ruder, Parsa Ghaffari, John G. Breslin, EMNLP, 2016

### Summary
- 本篇實作 SemEval2016 Aspect-based Sentiment Analysis task，每一個 review 都有數個句子，每個句子都會標註 aspect。aspect 由 entity 和 attribute 組成。
- 做法很簡單，每一句先用 sentence-level bi-LSTM encode (取最後一個 hidden state 當作句子 representation)
- entity 和 attribute 各有一個 lookup table
- 把剛 sentence-level bi-LSTM encode 出來的東西跟 entity embedding 還有 attribute embedding 串起來，餵進 review-level bi-LSTM，最後丟進 softmax 做分類
- 實驗
    - 考慮句與句之間的關係對結果有幫助
    - 在 11 個 dataset 拿到 5 個 state-of-the-art (其他 6 個都是特徵工程或加入語言學規則)
    - 此方法在 low-resource language 比較有用

### Weaknesses / Notes
- 創新度不高，整體效果還是輸基於語言學規則的方法
- 把 hierarchical 發揚光大的論文應該是 NAACL2016 的 [HAN](https://www.cs.cmu.edu/~hovy/papers/16HLT-hierarchical-attention-networks.pdf)。這篇也是用了類似的方法，不過沒用 attention mechanism
- 在前處理時，每個 sentence 都會 pad 到 max-len of sentence，然後每個 review 都會 pad 到 max-len of review，感覺 padding 充斥著整個 training data。會對結果有影響。不知是為了方便做 batch training 還是有其他目的。根據我的經驗，padding 太多對整體不是個好現象。
