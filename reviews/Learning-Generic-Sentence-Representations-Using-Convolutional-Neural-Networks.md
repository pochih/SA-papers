## Learning Generic Sentence Representations Using Convolutional Neural Networks

Zhe Gan, Yunchen Pu, Ricardo Henao, Chunyuan Li, Xiaodong He, Lawrence Carin, EMNLP, 2017

### Summary
- 文中使用了 encoder-decoder 架構，CNN as encoder, LSTM as decoder。其中 CNN 的 kernel-size = 3 or 4 or 5
- 實做了四種模型
  1. auto-encoder (encode sentence N, decode sentence N)
  2. predict next sentence (encode sentence N, decode sentence N+1)
  3. combination (encode sentence N, decode sentence N and N+1)
  4. hierarchical (encode sentence N, decode sentence N+1, 同時考慮之前之前丟進 LSTM 的 encoded sentence)
- 解決 OOV 方式
  1. pre-trained word embedding 質量比較好，但常有 OOV。因此需要一個辦法來把自己 train 的 word embedding map 到 pre-trained word embedding。
    - 做法是如果某個 word 有出現在 pre-trained embedding，就把自己 train 的 word embedding 當 feature，pre-trained word embedding 當 target，去訓練一個 linear regression 模型。
    - 由於 vocabulary 量級差太多，或是 corpus 量級差太多，效果沒有第二種好
  2. 直接用 word2vec，且不 fine-tuning。OOV 的字就直接忽略。
- 實驗做在許多 sentiment analysis dataset，甚至做在 MS COCO 上
- 大量且多面向實驗，參數也有參考價值

### Strengths / Novelties
- skip-though 比較像文中的 "predict next sentence" 模型。本篇使用的 "hierarchical" 架構，會考慮 long-term dependency，因此略勝一籌。不過比不上 task-specific model

### Weaknesses / Notes
- MS COCO 實驗中本篇用 VGG 抽 image feature，用 resnet 應該能偷點 accuracy