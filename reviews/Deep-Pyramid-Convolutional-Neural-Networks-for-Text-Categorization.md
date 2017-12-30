## Deep Pyramid Convolutional Neural Networks for Text Categorization

Rie Johnson, Tong Zhang, ACL, 2017

### Summary
- 作者之前的研究 [Convolutional Neural Networks for Text Categorization: Shallow Word-level vs. Deep Character-level](https://arxiv.org/abs/1609.00718) 指出 shallow word-level CNN 大於 deep char-level CNN
- 作者提出的 deep pyramid CNN 跟大部分的 deep CNN 有三點不一樣
	1. pyramid CNN
  	- 一般的 deep CNN，越上層 conv filter 越多
  	- 作者發現增加上層 filter 會讓運算變慢 (feature map 很厚) 而結果沒比較好，因此本篇則是把每一層的 filter 設成一樣多
  	- 而在每兩個 conv 後會接一層 max-pooling，讓 feature map 越變越窄，整體來看很像 pyramid
  2. pre-activation
  	- 模型當中的 conv layer 使用 pre-activation
  	- 假設 X 是經過 conv 運算過後的值，最終的 output 會是 W x a(X) + b，而不是一般的 a(W x X + b)，其中 a 是 Relu
  	- 作者 argue 說這樣的 pre-activation 會增加 'linearity'，可以減輕模型的訓練難度
  3. no dimension matching
  	- 本篇借鑒了 ResNet 當中的 skip-connection，公式為 x = z + f(z)
  	- 其中 z 是 input，f 是兩層 conv with pre-activation
  	- 在 ResNet 中 z 跟 f(z) 的長寬高都有可能不同，需要額外的 projection matrix
  	- 本篇會保證 z 與 f(z) 大小一樣 (長寬厚度都相同)，就可以省去額外做 projection 的計算量。
  	- 由於每層 conv filter 數量固定，"厚度" 也會固定。作者會在經過 max-pooling 之前就完成 shortcut connection，就可以保證長寬也一樣。
- 實驗結果在數據量大的 dataset 上表現是 state-of-the-art，在小數據上表現接近 state-of-the-art
- 使用最常出現的 30K word 效果就很好了

### Strengths / Novelties
- 很多人嘗試把 word-level CNN 加深，但效果都不好。作者對經典的 deep CNN 做了些微調整，包括 fix 住 conv filter 數量，以及 pre-activation，就能把 deep word-level CNN train 起來。
- pyramid CNN 跟 pre-activation 跟用來處理影像的 model 不太一樣，說不定能成為 CNN for NLP 的主流
