+++
title = "Plotting Word cloud in R"
date = 2021-01-02
draft = false
tags = ["Plotting Word cloud in R"]
categories = []
+++

## Plotting Word Cloud in R

```R
text <- readLines("martin-luther-king-i-have-a-dream-speech.txt")
```


```R
library("tm")
library("SnowballC")
library("wordcloud")
library("RColorBrewer")
```

    Loading required package: NLP
    
    Loading required package: RColorBrewer
    



```R
docs <- Corpus(VectorSource(text))
```


```R
toSpace <- content_transformer(function (x , pattern ) gsub(pattern, " ", x))
docs <- tm_map(docs, toSpace, "/")
docs <- tm_map(docs, toSpace, "@")
docs <- tm_map(docs, toSpace, "\\|")
```




```R
# Convert the text to lower case
docs <- tm_map(docs, content_transformer(tolower))
# Remove numbers
docs <- tm_map(docs, removeNumbers)
# Remove english common stopwords
docs <- tm_map(docs, removeWords, stopwords("english"))
# Remove your own stop word
# specify your stopwords as a character vector
docs <- tm_map(docs, removeWords, c("blabla1", "blabla2")) 
# Remove punctuations
docs <- tm_map(docs, removePunctuation)
# Eliminate extra white spaces
docs <- tm_map(docs, stripWhitespace)
# Text stemming
# docs <- tm_map(docs, stemDocument)
```




```R
dtm <- TermDocumentMatrix(docs)
m <- as.matrix(dtm)
v <- sort(rowSums(m),decreasing=TRUE)
d <- data.frame(word = names(v),freq=v)
head(d, 10)
```


<table>
<caption>A data.frame: 10 × 2</caption>
<thead>
	<tr><th></th><th >word</th><th >freq</th></tr>
	<tr><th></th><th >&lt;chr&gt;</th><th >&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><th >will</th><td>will    </td><td>17</td></tr>
	<tr><th >freedom</th><td>freedom </td><td>13</td></tr>
	<tr><th >ring</th><td>ring    </td><td>12</td></tr>
	<tr><th >dream</th><td>dream   </td><td>11</td></tr>
	<tr><th >day</th><td>day     </td><td>11</td></tr>
	<tr><th >let</th><td>let     </td><td>11</td></tr>
	<tr><th >every</th><td>every   </td><td> 9</td></tr>
	<tr><th >one</th><td>one     </td><td> 8</td></tr>
	<tr><th >able</th><td>able    </td><td> 8</td></tr>
	<tr><th >together</th><td>together</td><td> 7</td></tr>
</tbody>
</table>




```R
set.seed(1234)
wordcloud(words = d$word, freq = d$freq, min.freq = 1,
          max.words=200, random.order=FALSE, rot.per=0.35, 
          colors=brewer.pal(8, "Dark2"))
```


![png](https://raw.githubusercontent.com/balakuntlaJayanth/Stats/master/images/06_12_2020/output_6_0.png)


### References

- https://www.rdocumentation.org/packages/wordcloud/versions/2.6/topics/wordcloud