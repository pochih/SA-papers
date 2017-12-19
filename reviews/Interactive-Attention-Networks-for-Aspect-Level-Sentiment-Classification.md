## Interactive Attention Networks for Aspect-Level Sentiment Classification

Dehong Ma, Sujian Li, Xiaodong Zhang, Houfeng Wang, IJCAI, 2017

### Summary
- 2011 jiang 的 [paper](https://www.aclweb.org/anthology/P/P11/P11-1016.pdf) 說 sentiment analysis 中 40% 的錯誤是沒有考慮到 context 與 target 的關係。因此之後不少 paper 在做這件事。這篇作者覺得其他 paper 對 target 的處理還是太簡陋，因此提出這篇的 model
- target 不再用 word embedding 的 average 來表示。因為對越長的 target 而言，average 效果越不好。此外，target 中的每一個字重要度應該要不一樣，例如 target 中的連接詞所佔的重要度應該很低。所以作者用 attention 機制來算出 target representation
- context 跟 target 都會用 attention 機制，而在算 attention 時都會把對方考慮進去。例如算 context attention 時每個 LSTM hidden state 都會跟 average target hidden 一起考慮，反之亦然。
- attend 過後的 context representation 跟 target representation 會 concat 在一起，經過一層 dense 再做 softmax
- 實驗中提起了之前的一些方法，作者說對 target 付出越多努力的方法，表現也越好
- 做了 ablation study，證明不要用 target 平均而是要用 attention 比較好，以及在 attention 時同時考慮 context/target 都是必要的
- SemEval2014 中，用本篇的概念，laptop 這類會提升得比 restaurant 多，因為 restaurant 中 target 長度比較短，target attention 效果提升較少

### Strengths/novel places
- 算 context attention 跟算 target attention 時都會考慮彼此，跟之前 attention 的方法略有不同

### Weaknesses
- 這篇的結果輸 [Aspect Level Sentiment Classification with Deep Memory Network](https://arxiv.org/abs/1605.08900) 這篇，而且還引用他 XD
- 原因可能是 [Aspect Level Sentiment Classification with Deep Memory Network](https://arxiv.org/abs/1605.08900) 這篇用了很多次 hop，學到更抽象的語意。本篇只做一次 attention，學到的東西較低階