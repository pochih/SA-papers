## Linguistically Regularized LSTM for Sentiment Classification

Qiao Qian, Minlie Huang, Jinhao Lei, Xiaoyan Zhu, arXiv, 2017

### Summary
- 利用語言學規則來幫助語意分類, 將所有字分成四種
  - Non-Sentiment Regularizer (NSR)，代表不帶有語意的字，例如 movie, book, water
  - Sentiment Regularizer (SR)，包含語意的字，例如 interesting, good, disaster，從 sentiment lexicon 得到
  - Negation Regularizer (NR)，帶有語氣轉折的字，例如 not, nothing, never，手工提取
  - Intensity Regularizer (IR)，程度副詞，例如 very, terribly, too，手工提取
- 除了 classification loss，還會加上 Linguistically Regularized loss，主要概念是用 KL-diverenge 來衡量下一個字帶來的 distribution 變化。
  - 加上 NSR 後的 distribution 應該跟沒加差不多
  - 加上 SR 後的 distribution 會和原本很不同，不同程度的情緒詞會帶來不同的影響，這個現象叫做 sentiment drift
    - sentiment drift 是預先定義好的東西，在情感詞典中會有以下幾類：strong positive、weakly positive、weakly negative、strong negative，每一類都有自己的 sentiment drift
    - sentiment drift 大小等於 class 數量
    - 加上 SR 後的 distribution 要盡量跟原本差一個 sentiment drift
  - NR 跟 IR 比較複雜，例如 "good" 跟 "bad" 加上 "not" 後，前者由正轉負，後者是由負轉中性
    - 因此每個 NR 與 IR 都有自己的 transformation matrix，描述 distribution 的變化
    - transformation matrix 是一個 CxC 的矩陣，C 是 class 數量
    - 如果 p 是原本的 distribution，p 加上 NR/IR 後的 distribution，要跟 p 乘上 transformation matrix 差不多
  - sentiment drift & transformation matrix 用 prior 定義好，可一起訓練也可不要
- 因為情緒詞可能會修飾前面，也可能會修飾後面，因此用雙向 LSTM
- 實驗結果顯示少了四種中任意一種，都會讓結果變差，其中 NSR 跟 SR 帶來的效益最大，NR 跟 IR 效益不多，作者說因為 NSR 跟 SR 的數量遠大於 NR 跟 IR

### Strengths / Novelties
- 在 neural network 橫行的今日，傳統語言學規則逐漸被遺忘。如果能結合兩者優勢，一定會比只用其中之一來得好
- 本篇的方法跟 parse tree based 的概念很像，都是去探討某一個字對之前語意的影響。不過不會像 tree-based 方法需要良好的 parser 以及精細標注的 data，比較貼近現實需求

### Weaknesses / Notes
- performance 並沒有超越之前的論文。如果把語言學概念加到不同的 neural model 中，會不會更好呢？
