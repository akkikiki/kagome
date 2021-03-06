[![Build Status](https://travis-ci.org/ikawaha/kagome.svg?branch=master)](https://travis-ci.org/ikawaha/kagome) [![Coverage Status](https://coveralls.io/repos/ikawaha/kagome/badge.png?branch=master)](https://coveralls.io/r/ikawaha/kagome?branch=master) [![GoDoc](https://godoc.org/github.com/ikawaha/kagome?status.svg)](https://godoc.org/github.com/ikawaha/kagome) [![Go Walker](http://gowalker.org/api/v1/badge)](https://gowalker.org/github.com/ikawaha/kagome)  [![BuildStatus(Windows)](https://ci.appveyor.com/api/projects/status/k4g4bpy1ijqoasbe/branch/master?svg=true)](https://ci.appveyor.com/project/ikawaha/kagome)

Kagome Japanese Morphological Analyzer
===

Kagome is an open source Japanese morphological analyzer written in pure golang.
The MeCab-IPADIC dictionary/statiscal model is used and packaged in Kagome binary.

```
% kagome
すもももももももものうち
すもも	名詞,一般,*,*,*,*,すもも,スモモ,スモモ
も	助詞,係助詞,*,*,*,*,も,モ,モ
もも	名詞,一般,*,*,*,*,もも,モモ,モモ
も	助詞,係助詞,*,*,*,*,も,モ,モ
もも	名詞,一般,*,*,*,*,もも,モモ,モモ
の	助詞,連体化,*,*,*,*,の,ノ,ノ
うち	名詞,非自立,副詞可能,*,*,*,うち,ウチ,ウチ
EOS
```

Install
---

```
% go get github.com/ikawaha/kagome/...
```

Usage
---

```
$ kagome -h
usage: kagome [-file input_file | --http addr] [-udic userdic_file] [-mode (search|extended)]
  -file="": input file
  -http="": HTTP service address (e.g., ':6060')
  -mode="": tokenize mode
  -udic="": user dic
```

#### Segmentation mode for search

Kagome has segmentation mode for search such as [Kuromoji](http://www.atilika.com/en/products/kuromoji.html).

* Normal: Regular segmentation
* Search: Use a heuristic to do additional segmentation useful for search
* Extended: Similar to search mode, but also unigram unknown words

|Untokenized|Normal|Search|Extended|
|:-------|:---------|:---------|:---------|
|関西国際空港|関西国際空港|関西　国際　空港|関西　国際　空港|
|日本経済新聞|日本経済新聞|日本　経済　新聞|日本　経済　新聞|
|シニアソフトウェアエンジニア|シニアソフトウェアエンジニア|シニア　ソフトウェア　エンジニア|シニア　ソフトウェア　エンジニア|
|デジカメを買った|デジカメ　を　買っ　た|デジカメ　を　買っ　た|デ　ジ　カ　メ　を　買っ　た|

#### HTTP service

##### Web API

```
$ kagome -http=":8080" &
$ curl -XPUT localhost:8080 -d'{"sentence":"すもももももももものうち"}'
{"status":true,"tokens":[{"id":36163,"start":0,"end":3,"surface":"すもも","class":"KNOWN","features":["名詞","一般","*","*","*","*","すもも","スモモ","スモモ"]},{"id":73244,"start":3,"end":4,"surface":"も","class":"KNOWN","features":["助詞","係助詞","*","*","*","*","も","モ","モ"]},{"id":74989,"start":4,"end":6,"surface":"もも","class":"KNOWN","features":["名詞","一般","*","*","*","*","もも","モモ","モモ"]},{"id":73244,"start":6,"end":7,"surface":"も","class":"KNOWN","features":["助詞","係助詞","*","*","*","*","も","モ","モ"]},{"id":74989,"start":7,"end":9,"surface":"もも","class":"KNOWN","features":["名詞","一般","*","*","*","*","もも","モモ","モモ"]},{"id":55829,"start":9,"end":10,"surface":"の","class":"KNOWN","features":["助詞","連体化","*","*","*","*","の","ノ","ノ"]},{"id":8024,"start":10,"end":12,"surface":"うち","class":"KNOWN","features":["名詞","非自立","副詞可能","*","*","*","うち","ウチ","ウチ"]}]}
```

##### Demo

Launch a server and access `http://localhost:8080/_demo`.
(To draw a lattice, demo application uses [graphviz](http://www.graphviz.org/) . You need graphviz installed.)


```
$ kagome -http=":8080" &
```

![lattice](https://raw.githubusercontent.com/wiki/ikawaha/kagome/images/demoapp.png)

#### User Dictionary
User dictionary format is same as Kuromoji. There is a sample in `_sample` dir.

```
% kagome
第68代横綱朝青龍
第	接頭詞,数接続,*,*,*,*,第,ダイ,ダイ
68	名詞,数,*,*,*,*,*
代	名詞,接尾,助数詞,*,*,*,代,ダイ,ダイ
横綱	名詞,一般,*,*,*,*,横綱,ヨコヅナ,ヨコズナ
朝青龍	カスタム人名,朝青龍,アサショウリュウ
EOS
```
### Utility

A debug tool of tokenize process outputs a lattice in graphviz dot format.

```
$ lattice -v すもももももももものうち  |dot -Tpng -o lattice.png
すもも	名詞,一般,*,*,*,*,すもも,スモモ,スモモ
も	助詞,係助詞,*,*,*,*,も,モ,モ
もも	名詞,一般,*,*,*,*,もも,モモ,モモ
も	助詞,係助詞,*,*,*,*,も,モ,モ
もも	名詞,一般,*,*,*,*,もも,モモ,モモ
の	助詞,連体化,*,*,*,*,の,ノ,ノ
うち	名詞,非自立,副詞可能,*,*,*,うち,ウチ,ウチ
EOS
```
![lattice](https://raw.githubusercontent.com/wiki/ikawaha/kagome/images/lattice.png)

License
---
Kagome is licensed under the Apache License v2.0 and uses the MeCab-IPADIC dictionary/statistical model. See NOTICE.txt for license details. 
