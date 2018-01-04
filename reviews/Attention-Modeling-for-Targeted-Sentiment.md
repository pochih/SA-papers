## Attention Modeling for Targeted Sentiment

Jiangming Liu, Yue Zhang, EACL, 2017

### Summary
- 比較三種 attention model，三種 model 都採用雙向 LSTM 加上 attention
    1. BILSTM-ATT: 直接把整句 input 丟進 LSTM，對 LSTM 做 attention，得到 hidden state 的 weighted sum，之後丟進 classufuer
    2. BILSTM-ATT-C: 把整句 input 依照 target phrase 的位置，分成左 context 跟右 context，然後兩個 context 會分開做 attention，做完後會一起丟進 classifier
    3. BILSTM-ATT-G: 這個方法是前兩個方法的綜合，一共會考慮左 context，右 context，以及 whole sentence。這三個部分都會分別丟進一個 LSTM，做 attention。做完 attention 後，三個 weighted sum 會分別通過一個 gate (會保證三個 gate 和是 1)，再 weighted sum 起來。這個想法是在第二作者的前作 ‘[Gated Neural Networks for Targeted Sentiment Analysis](https://www.aaai.org/ocs/index.php/AAAI/AAAI16/paper/download/12074/12065)’ 這篇論文中提出的，該論文發現分成左右 context 時，如果直接 concat 起來，可能會被其中一邊 dominant 掉，所以加上了 gate 來控制
    - 這三種 model 中，每個 hidden state 都會先跟 average of target hidden state 串在一起後，才做 attention
- 實驗結果
    - 對 BILSTM-ATT-G 而言，如果沒做 attention，不加 gate 比較好。有做 attention，配上 gate 更好 (因為能更好的把兩個量級不同的東西 concat 起來)
    - 作者分析在 positive, neutral, negative 三個類別上的準確率。對 positive 以及 negative 而言，BILSTM-ATT-G 表現最好。對 neutral 而言，BILSTM-ATT-C 表現最好。

### Strengths / Novelties
- 要把多個量級不同的東西一起考慮，例如文中把多個 LSTM 的產物一起考慮，用 gate 來控制每個 input 的比重，會比直接 concat 起來更好。二作 Yue Zhang 已經在兩篇論文中用了這個技巧

### Weaknesses / Notes
- 這篇的中 target representation 是直接用 target hidden state 的平均，但平均這種東西，怕的就是遇到太長的 target。如果對 target hidden state 做 attention，用 attend 後的結果當成 target representation，可能會更 robust。
- 為了驗證 BILSTM-ATT-G 對 [OOV (out of vocabulary)](http://www.festvox.org/bsv/x1407.html) 的效果很好，作者額外切了一組不包含 OOV 的 test data，並切了一組包含很多 OOV 的 test data。對這兩個 test data 做實驗，得出 OOV 很多的時候，對 BILSTM-ATT-G 影響較小。不過作者在 OOV 實驗中的圖表畫反了，算是一個小 bug
