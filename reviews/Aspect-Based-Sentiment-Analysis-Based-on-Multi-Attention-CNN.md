## Aspect-Based Sentiment Analysis Based on Multi-Attention CNN

Liang Bin, Liu Quan, Xu Jin, Zhou Qian, Zhang Peng, Journal of Computer Research and Development, 2017

### Summary
- TACL2016 的論文 [ABCNN](https://arxiv.org/abs/1512.05193) 是首篇將 attention mechanism 結合 CNN 的論文。本篇借鑑了這個思路，並提出 multi attention。
- 一般做 attention，都只考慮文本本身。作者額外加了兩種 attention: 詞性 attention & 位置 attention。
- 本篇的三種 attention
  1. word vector attention: 作法很簡單，將 target word embedding 與句中所有字的 word embedding 做內積，然後把所有內積值做一次 softmax，就得到每個字的權重。
  2. part of speech attention: 跟 word embedding 一樣，詞性也有自己的 embedding。
  3. location attention: 與 target word 距離越遠的字權重越小
- 三種注意力機制都會產生出一個 vector，因此句中的每一個字都能得到三個 vectors。本文試了數種方法將這些 vectors 結合起來。
  - AATT-CNN: 三種注意力機制得到的 vector 根據可學習的權重加起來，用這個方式三個 vector 必須維度一樣。
  - CATT-CNN: 三種注意力機制得到的 vector 根據可學習的權重矩陣加起來，用這個方式三個 vector 維度可以不一樣。
  - SATT-CNN: 三種注意力機制得到的 vector 把它 stack 起來，類似 RGB 多通道的概念。用這個方式三個 vector 必須維度一樣。
- 實驗
    - CNN + attention 遠勝於 CNN
    - multi-attention + CNN 勝於 attention + CNN
    - LSTM + attention 在小數據贏過 multi-attention + CNN，但數據量一多就輸。且 LSTM 訓練時間遠大於 CNN。

### Strengths / Novelties
- 第一次在 sentiment analysis 論文中看到 CNN 結合 attention
- 不只對文本本身做 attention，還加上了詞性與位置。

### Weaknesses / Notes
- target word vector 是取平均，作法簡單但遇到長一點的 target word 效果會變差。
- LSTM + attention 勝過 CNN + attention，輸給 CNN + multi-attention。如果 LSTM + multi-attention 不知道效果如何。(但訓練一定更慢)
- 本文是先做 attention 再做 conv。如果對 conv 之後的產物做 attention，不知效果如何。
