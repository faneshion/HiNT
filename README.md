# HiNT

## Implementation of HiNT (Hierarchical Neural maTching) model proposed in SIGIR'18 for ad-hoc retrieval.


/** Copyright (C) 2016 by Center for CAS Key Lab of Network Data Science and Technology, Institute of Computing Technology, CAS

The package of HiNT is distributed for research purpose, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

More details can be found in the following paper:
Yixing Fan, Jiafeng Guo, Yanyan Lan, Jun Xu, Chengxiang Zhai, Xueqi Cheng, Modeling Diverse Relevance Patterns in Ad-hoc Retrieval. In SIGIR'18: The 41st International ACM SIGIR Conference on Research & Development in Information Retrieval, July 8-12, 2018, Ann Arbor, MI, USA. Full Paper.

Feel free to contact the following people if you find any problems in the package. fanyixing111@hotmail.com

## Introduction

Assessing relevance between a query and a document is challenging in ad-hoc retrieval due to its diverse patterns, i.e., a document could be relevant to a query as a whole or partially as long as it provides sufficient information for users' need. Such diverse relevance patterns require an ideal retrieval model to be able to assess relevance in the right granularity adaptively. Unfortunately, most existing retrieval models compute relevance at a single granularity, either document-wide or passage-level, or use fixed combination strategy, restricting their ability in capturing diverse relevance patterns. In this work, we propose a data-driven method to allow relevance signals at different granularities to compete with each other for final relevance assessment. Specifically, we propose a HIerarchical Neural maTching model (HiNT) which consists of two stacked components, namely local matching layer and global decision layer. The local matching layer focuses on producing a set of local relevance signals by modeling the semantic matching between a query and each passage of a document. The global decision layer accumulates local signals into different granularities and allows them to compete with each other to decide the final relevance score.

## How to run

This code is implemented based on  [textnet](https://github.com/pl8787/textnet-release). Here, we provide the config file for HiNT. We are also preparing an implementation with [MatchZoo](https://github.com/faneshion/MatchZoo), which will coming soon.

```
nohup ./textnet/bin/textnet config/letor.mq2007.hint.fold1.config > log/letor.mq2007.hint.fold1.log & 2>&1
```

Here, we take the mq2007 dataset as an example. While, in testing, you need to run all the five fold iteratively, and average all the performance as the final results.

## Sample Output

```
[01:54:48] [Process] Set Tag to Train.
[01:54:48] [Process] Set Phrase to 0.
[01:54:48] [Process] Reshape network.
[01:55:01] [Train:kTrain]       Iter    0:      Out[loss] =     1.000203
[01:56:59] [Train:kTrain]       Iter    10:     Out[loss] =     0.910032
[01:58:46] [Train:kTrain]       Iter    20:     Out[loss] =     0.771935
[02:00:37] [Train:kTrain]       Iter    30:     Out[loss] =     0.739713
[02:02:27] [Train:kTrain]       Iter    40:     Out[loss] =     0.853702
[02:04:07] [Train:kTrain]       Iter    50:     Out[loss] =     0.787309
[02:05:43] [Train:kTrain]       Iter    60:     Out[loss] =     0.791946
[02:07:15] [Train:kTrain]       Iter    70:     Out[loss] =     0.896669
[02:08:48] [Train:kTrain]       Iter    80:     Out[loss] =     0.728578
[02:10:19] [Train:kTrain]       Iter    90:     Out[loss] =     0.622544
[02:11:40] [Process] Set Tag to Test.
[02:11:40] [Process] Set Phrase to 1.
[02:11:40] [Process] Reshape network.
[02:17:23] [Test:kTest] Iter    100:    Out[P@10]#avg = 0.367857
[02:17:23] [Test:kTest] Iter    100:    Out[nDCG@10]#avg =      0.396150
[02:17:23] [Test:kTest] Iter    100:    Out[MAP]#avg =  0.420513
[Memory] Total Memory Use: 4362.3789 MB          Resident: 4467076 Shared: 0 UnshareData: 0 UnshareStack: 0
```



## Download the Letor Data

Dataset can be download from [LETOR 4.0](https://www.microsoft.com/en-us/research/project/letor-learning-rank-information-retrieval/). The content of the documents can be extract from the GOV2 dataset which need permission from [here](http://ir.dcs.gla.ac.uk/test_collections/access_to_data.html).

#### **Data Example**

```
2 qid:10032 1:0.056537 2:0.000000 3:0.666667 4:1.000000 5:0.067138 … 45:0.000000 46:0.076923 #docid = GX029-35-5894638 inc = 0.0119881192468859 prob = 0.139842
```

## **Preprocess**

In order to run TextNet models, we need prepare files below:

#### ***Word Dictionary File***

> *(eg. word_dict.txt)*

We map each word to a uniqe number, called `wid`, and save this mapping in the word dictionary file.

For example,

```
word   wid
machine 1232
learning 1156

```

#### ***Corpus File***

> *(eg. qid_query.txt and docid_doc.txt)*

We use a value of string identifier (`qid`/`docid`) to represent a sentence, such as a `query` or a `document`. The second number denotes the length of the sentence. The following numbers are the `wid`s of the sentence.

For example,

```
docid  sentence_length  sentence_wid_sequence
GX000-00-0000000 42 2744 1043 377 2744 1043 377 187 117961 ...

```

#### **Relation File**

> *(eg. relation.train.fold1.txt, relation.test.fold1.txt ...)*

The relation files are used to store the relation between two sentences, such as the relevance relation between `query` and `document`.

For example,

```
relevance   qid   docid
1 3571 GX245-00-1220850
0 3571 GX004-51-0504917
0 3571 GX006-36-4612449

```

#### ***Embedding File***

> *(eg. embed_wiki-pdc_d50_norm)*

We store the word embedding into the embedding file.

For example,

```
wid   embedding
13275 -0.050766 0.081548 -0.031107 0.131772 0.172194 ... 0.165506 0.002235

```

## **Config Files**

The example [config files](./config/) are in config directory.

| Config Fields  | File Type      |
| -------------- | -------------- |
| data1_file     | Corpus File    |
| data2_file     | Corpus File    |
| rel_file       | Relation File  |
| embedding_file | Embedding File |

### 

## FAQ 

*Q1: Is it possible for you to share the embeddings you have used in the experiments?*

A1: You can download the 300 dimensional word embedding I have used for my experiments from here:

https://github.com/idio/wiki2vec/

For word embedding with other dimension size, I suggest you to download the [wikipedia data](), and train it use [word2vec](http://code.google.com/archive/p/word2vec/) by yourself.



