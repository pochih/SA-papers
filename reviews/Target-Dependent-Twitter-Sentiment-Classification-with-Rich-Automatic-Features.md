## Target-Dependent Twitter Sentiment Classification with Rich Automatic Features

Duy-Tin Vo and Yue Zhang, IJCAI, 2015

### Summary
- 這篇是 feature engineering 的很好範例，手工抽取 target-dependent & target-independent feature
- 把句子拆解成 left context, target, right context，用了 word2vec & SSWE 兩種 word embedding 來表示 word
- 除此之外，借助了三個 sentiment lexicon 來濾掉 word embedding 中不在詞典內的字
- 對兩種 word embedding 的左 context、右 context、target、整句話、左 sentiment context、右 sentiment context 抽 feature，抽法是 dim-wise pooling，有 max, min, avg, std, product 五種
- 最後的 feature 進入 liblinear 中分類
- 實驗結果
    - 五種 pooling 都用會有最好結果
    - lexicon 辭典只用 MPQA + HL 效果最好
    - SSWE+word2vec > SSWE > word2vec

### Strengths/novelties
- 在大 deep learning 時代，用手工提取特徵的 paper 越來越少。本篇結合新舊方法，得到不錯的 performance

### Weaknesses
- 沒有很好利用 context 與 target 之間的關係，pooling 過後的東西也不太有 syntatic 的關聯。用 LSTM 來 model 句子應該會是更好的做法，然後用 target 來對 context 做 attention