## Efficient Character-level Document Classification by Combining Convolution and Recurrent Layers

Yijun Xiao, Kyunghyun Cho, arXiv, 2016

### Summary
- CNN as feature extractor
  - 在 EMNLP 2015 的論文 '[Document modeling with gated recurrent neural network for sentiment classification](http://aclweb.org/anthology/D15-1167)'，是用 CNN 抽 word-level 資訊，構成 sentence，再把每一個 sentence feature 丟進 RNN。
    - 層級：word -> sentence -> document
  - 本篇使用 CNN 抽 char-level feature，接著把一個 document 抽出的所有 feature 餵進 bi-LSTM
    - 層級：character -> document
- all CNN vs CNN+RNN
  - NIPS 2015 的論文 [Character-level convolutional networks for text classification](https://arxiv.org/abs/1509.01626) 使用全 convolution layer 來捕捉 char 資訊
  -   因為一篇文章的 character 的數量很多，必須疊非常多層 CNN，receptive field 才夠大，才能捕捉到 long-term 的訊息。
  - 所以本篇在疊了幾層 CNN 後，把得到的東西丟進 RNN，利用 RNN 能捕捉 long-term 資訊的特性來補足這點。不僅能節省參數以及層數，也能做較少次 pooling，減少訊息流失。
- 實驗結果
  - 當 class 數目越多 (fine-grained)，本篇效果比 fully CNN 那篇好越多
  - 在小 dataset 上贏，大 dataset 上輸 (可能參數比較少比較不會 overfitting?)
  - 增加 convolution layer 中 filter 數量帶來的好處極為有限

### Strengths / Novelties
- 先用幾層 CNN 抽取 local 資訊，再用 RNN 把整個序列的資訊統整。這套架構在很多地方都很好用

### Weaknesses / Notes
- 本篇直接對整個 document 抽 feature，但 document 長的時候，丟給 RNN 的序列會過長。如果多考慮 word/sentence 層級，把 word 層級或 sentence 層級餵給 RNN，就能解決問題。
- 這篇的概念跟 EMNLP 2015 的論文 '[Document modeling with gated recurrent neural network for sentiment classification](http://aclweb.org/anthology/D15-1167)' 幾乎一樣，差別是本篇多考慮 char-level
