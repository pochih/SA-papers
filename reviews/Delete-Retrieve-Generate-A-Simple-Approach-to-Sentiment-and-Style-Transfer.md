## Delete, Retrieve, Generate: A Simple Approach to Sentiment and Style Transfer

Juncen Li, Robin Jia, He He, Percy Liang, NAACL-HLT, 2018

### Summary
- 任務目標
  - Task 很新，2017 年才有的任務。目的是在做 style transfer 的同時，保留句子的主題。
  - 如果以情感分析為例子，style 可以是 positive or negative，那任務就變成給你一個句子，你要把他的情感由正轉負或反過來，但句子中的物件內容要類似。"The chicken is delicious" 這句話的 style 是 positive，我們要保留主題 "chicken" 但把情感扭轉，產生如 "The chicken is raw" 之類的句子。
  - 任務難度在於怎麼把跟 style 有關的字以及跟 style 無關但在描述句子主題的字解耦 (disentangle)。
- 作法
  - 作者發現 style 通常是由幾個很明顯的 phrases 決定，這些 phrases 只佔句子的一小部分。
  - 因此把跟只要把對 style 貢獻較大的字拿掉 (在情感分析中就是拿掉明顯的情感字例如 'good' or 'bad')，剩下的就是在談論主題的字。
  - 這些剩下來的字，用來產生我們想要的 target style。
  - 作者提出四種方式去產生句子，其中用 retrieval-based 的方式，也就是去包含 target style 的句子中 retrieve features 的方式表現最好。

### Strengths / Novelties
- previous works 都是用 GAN 來做 style transfer，本篇用了非常簡單的 rule-based 方法就產生質量更好的句子。
- 本篇以及之前某些 NLG (natural language generation) 的論文提到 retrieval-based 的方式通常能產生更好的句子。

### Weaknesses / Notes
- training 的方式有些奇怪，感覺沒有很好地利用 retrieved 到的東西來算 loss