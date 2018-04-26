## Refining Word Embeddings for Sentiment Analysis

Liang-Chih Yu, Jin Wang, K. Robert Lai, Xuejie Zhang, EMNLP, 2017

### Summary
- 多數 pre-trained word vector 會把 context 類似的字 train 的很像，例如 'good' & 'bad' 的 word vector 會很像，這現象在情感分析中是很不好的 
- 這是一篇簡單有效的 paper，利用情感字典去修正 pre-trained word embedding，不需要從 labeled data 中學
- 本篇希望做到兩件事
	1. 在情感字典中相似的字，去拉近他們的 word vector
	2. 在情感字典中不相似的字，去分開他們的 word vector
- 作法
	1. 現在要更改 target word 的 word vector，先用 cosine similarity 在 pre-trained word vector 上 找出 top-K 最像的 words
	2. 情感字典中會給每個字一個情感分數，這裏用情感字典中，分數差距的絕對值來對 top-K words 做 re-rank，在情感字典中分數跟 target word 越像的，排名越高
	3. 這 K 個字都會有一個分數，我們這裡用第 j 名來解釋：分數算法是 Wij * distance(Vi, Vj)
		- Wij 是權重，是 re-rank 排名的 reciprocal (ex: re-rank 後排名第 j 的字，權重就是 1/j)
		- distance 是 Euclidean distance
        - Vi 是 target word 的 word vector
        - Vj 是第 j 名的 word vector
    4. 會希望最終的 word vector 跟原本不要差太多，所以會加上一個限制：distance(Vi, Vi')
        - Vi 是更新前的 vector
        - Vi' 是更新過後的 vector
    5. 最終的 Loss: 
    <img align='center' style="border-color:gray;border-width:2px;border-style:dashed"   src='https://github.com/brianhuang1019/SA-papers/blob/img/loss.png' padding='5px' height="300px"></img>
- 實驗
	- 實驗在 SST 上，結果比原本的 Word2Vec 和 GloVe 好

### Strengths / Novelties
- 有別於一般利用 sentiment analysis data 去訓練適合情感分析的 word vector，本篇從另一種角度去訓練 word vector，頗為有趣

### Weaknesses / Notes
- 仔細想想，作者提出的公式只會拉近在情感字典中相像的字，而不會去遠離在情感字典中不相像的字，等於作者的願望只實現了一半
- Wij 的選擇似乎很重要，如果把它當成模型的參數去學，而不是簡單的取倒數，不知可不可行 (可以觀察模型自己學出來的 Wij 會長什麼樣子，說不定會有神奇的發現)
