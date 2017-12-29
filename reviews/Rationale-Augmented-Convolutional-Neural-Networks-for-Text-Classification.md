## Rationale-Augmented Convolutional Neural Networks for Text Classification

Ye Zhang, Iain Marshall, Byron C. Wallace, EMNLP, 2016

### Summary
- 利用句子級別的 label 來幫助文本情感分類
- training 過程分成兩階段
  1. 利用句子級別的 label，來 train sentence classifier
    - 用 CNN 抽取 word-level 的 feature，經過 max-of-time pooling 得到 sentence feature
    - 接著利用抽出來的 sentence feature 經過 softmax 來預測類別
  2. 用剛才 train 好的 sentnece classifier 來協助預測 document label
    - 用同樣的 CNN 抽取 sentence feature，接著對 document 中所有 sentence 做 weighted sum
    - 作者希望帶有越強極性的句子，有越高的權重，可以幫助分類
    - 利用 train 好的 sentence classifier，可以得到 {positive機率, neutral機率, negative機率}
    - 每個 sentence 的權重就是 max(positive機率, negative機率)，極性強的句子權重越高
- 實驗做在生醫文本以及電影評論上，有利用 sentence-level label 的表現都最好
- 為了證明本篇方法的效果，作者額外用了一般的 attention 來給 sentence 權重，而實驗則證明了不如本篇方法來的好 
- 在 training 過程是兩階段，作者試驗過 alternating training approach，效果不好，因此採用 cascade-like training approach

### Strengths / Novelties
- 在 sentiment analysis 中，大部分的 paper 都是朝改進 model 的思路前進。比較少 paper 專注在利用額外的資訊(包含額外的 label 以及傳統 NLP 的知識)來提升結果。

### Weaknesses / Notes
- not end-to-end
- 有額外 sentence-level label 的 dataset 非常難找。
