## A Deeper Look into Sarcastic Tweets Using Deep Convolutional Neural Networks

Soujanya Poria, Erik Cambria, Devamanyu Hazarika, Prateek Vij, COLING, 2016

### Summary
- 在多數情感分析的論文中，都指出包含反諷 (sarcasm) 的情感分析是很困難的一件事
- 作者認為一句話是否包含反諷，除了分析文本本身之外，如果能知道文本中的情感 (sentiment)、情緒 (emotion)、作者人格特質 (personality)，可以讓反諷語偵測更準
	- 訓練了四個 CNN，主要的 CNN 是做用在句子本身，做後一層做二元分類，判斷此句是否反諷，除此之外，作者還使用了其他三個輔助 CNN 來抽 feature
    - 三個輔助 CNN 包括： CNN for sentiment, CNN for emotion, CNN for personality
    - 輔助 CNN 會分別先用額外的 dataset 來 train，例如 CNN for sentiment 是先找一個 sentiment analysis 的 dataset 來 train
- 在使用主要 CNN 對文本做反諷語偵測時，用其他三個 pretrain 好的 CNN 來抽 feature (取倒數第二層)，然後一起丟進主要 CNN 的分類器中
- 實驗結果比之前基於 feature engineering 和 sentiment lexicon 的方法好
- 值得一提的是，除了用主要 CNN 的分類器來分類之外，作者還會用 CNN 抽出來的 feature 再去 train 一個 SVM，結果通常會好個 2%，不知道原因何在

### Strengths / Novelties
- 第一次用 deep method 來做 sarcasm detection
- 想到用不同面向的 feature 來幫忙做分類，可以輔助只看文本本身的不足

### Weaknesses / Notes
- 作者說這篇的方法不需要領域知識，但選定 sentiment、emotion、personality 這三個面向來幫助分類，也是需要一定程度上的領域知識。
