## Lexicon Integrated CNN Models with Attention for Sentiment Analysis

Bonggun Shin, Timothy Lee, Jinho D. Choi, WASSA, 2017

### Summary
- 利用情感字典增加 CNN 的表現，模型非常的簡單
- basic CNN model 參考 Yoon Kim 2014 的 paper [Convolutional Neural Networks for Sentence Classification](http://aclweb.org/anthology/D14-1181)
- 本篇會把 lexicon embedding 加進去考慮，lexicon embedding 算法如下：把所有情感字典串在一起得到 lexicon embedding。把值 normalize 到 [-1, 1]，缺失的值補 0
- 作者嘗試三種把 lexicon embedding 加入 CNN 的方式
  - 第一種是把 lexicon embedding (N x e) 跟 word embedding (N x d) 串起來，成為 N x (e+d)，再去做 convolution
  - 第二種是 multi-channel，因為 e << d，所以 lexicon embedding 會 pad 0，成為 (N x d x 2)，再去做 convolution
  - 第三種是兩個 embedding 分開做 convolution，作完 max-of-time pooling 後再串起來
  - convolution 完的結果會經過 max-of-time pooling，最後丟進 softmax layer
- 由於 CNN 只考慮了 local feature，忽略了 global feature，作者還加入了 embedding attention
  - 會分別對 word embedding 以及 lexicon embedding 做 attention
  - 以 word embedding 為例，對 N x d 的 matrix 做 filter size=1 的 convolution，如果有 m 個 filter，就會得到 N x m 的 output
  - 接著對每個 row 做 max-pooling，得到長度為 N 的 vector，用它來代表每個 row 的權重，接著用這個權重對 word embedding 做矩陣相乘 (1 x N) x (N x d)，會得到一個 d 維的 vector，也就是 word embedding 的 weighted sum
  - 算出來的 embedding attention vector，會一起丟進 softmax layer
- 實驗結果
  - 做在 SemEval16 以及 SST 上，加入 lexicon embedding 會讓結果增加 1%
  - embedding attention 的加入也能讓結果增加 1%
  - embedding attention 的加入會加速收斂

### Strengths / Novelties
- 用非常簡單的手法增加 CNN model 的 performance
- 一般而言在 fully convolutional model 中不會有 attention，作者試著做了 attention，也達到些微的效能提升

### Weaknesses / Notes
- 作者做 attention 的方式不是那麼直覺，kernel size=1 的 convolotion 喪失了 n-gram 的性質。如果能使用不同 kernel size，經過適當的 padding，再對其 output 做 attention，應該會更加 robust
