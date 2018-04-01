## Relation Classification via Multi-Level Attention CNNs

Linlin Wang, Zhu Cao, Gerard de Melo, Zhiyuan Liu, ACL, 2016

### Summary
- Relation Classification 是要判斷句中兩個 entity 的關係
- 模型中同時包含兩種 attention
	- input 階段的 attention
	- 做完 convolution 之後的 attention
- input attention
	- input sentence 中每個字的 feature 是 concat(word embedding, position embedding)
	- position embedding 是透過與兩個 entity 之間的距離得到 (每個距離都有自己的 embedding vector)
	- attention 是 entity 對每個字做內積，然後取 softmax 
- convolution attention
	- 做完 attention 後，會把 convolution output 跟 relation embedding 做矩陣相乘，然後取 softmax，得到 convolution output 中每個 element 的重要程度
	- 每個 row 挑一個最大值，當成模型的輸出
- loss function 是用 negative sampling，希望模型的輸出會跟正確的 relation embedding 越像越好，然後要跟錯誤的 relation embedding 越遠越好

### Strengths / Novelties
- 在 input 做 attention，可以讓後面的 layer 接收到更豐富的訊息，特別是包含 entity 的句子，如果能考慮 entity 的位置，會對之後的預測有幫助
- loss function 相當於 negative sampling 的概念，對結果頗有幫助，比單純的 multi-class classification 效果更好
- 實驗中把最容易判別某個 relationship 的 tri-gram 列了出來，實驗很有意思，不過不知道作者是怎麼求出這些 tri-gram 的

### Weaknesses / Notes
- convolution attention 意義不明，在這個步驟可以有很多的變形，作者沒說方法的思路，而且這個東西效果普通
