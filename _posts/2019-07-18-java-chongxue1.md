---
layout: post
category: java
title: 重学Java(一)：与《Java编程思想》的不解之缘
tagline: by 沉默王二
tag: [java,重学Java]
---

说起来非常惭愧，我在 2008 年的时候就接触了 Java，但一直到现在（2018 年 10 月 10 日），基础知识依然非常薄弱。用一句话自嘲就是：十年 IT 老兵，Java 菜鸡一枚。

于是，我想，不如静下心来，重新读一遍那些经典的 Java 技术书，并且没读完一章就输出一篇原创技术文章。从哪一本开始呢？想了一想，还是从《Java 编程思想》开始吧！毕竟这本书赢得了全球程序员的广泛赞誉，从 Java 的基础语法到最高级特性，都能指导我们 Java 程序员轻松掌握。


<!--more-->

记得刚上大学那会，就买了一本影印版的《Java 编程思想》，但由于初学 Java，对编程极度缺乏信心，导致看这本书有一种看天书的感觉。后来，去苏州参加工作的时候把它作为最宝贵的纪念品带了过去。

2014 年回洛阳的时候它送给了一位关系不错的同事，权当是分别的礼物吧。2016 年的时候，我又重新买了一本，希望自己能够夯实一下基础。但事与愿违，它被我束之高阁了——又两年过去了，我重新捧起它，总觉得有一种负罪感。

读一本书，最好能从它的前言开始。那么我们就来看看 Bruce Eckel 在前言里都说了些什么吧。

# 01、Java 的核心目的是“为程序员减少复杂性”。

James Gosling 创建 Java 语言的初衷是：“减少开发健壮代码所需的时间和困难”。尽管这个目标导致 Java 的运行效率偏慢，但与用 C++ 开发相同的程序相比，Java 只需要一半甚至更少的时间。

作为程序员，这是我们希望看到的。少敲代码省下来的那一部分时间，可以约个妹子去看场电影，放松一下，对吧？况且，Java 一直在更新，性能也不断地被优化。

记得上大学那会，我们专业只有两个班，一个班学 Java，一个班级学 C++。结果大学毕业后，C++ 的同学几乎都转了行，有些同学反馈说因为 C++ 的指针太飘忽不定了，难学难懂难掌握（C++ 表示不服，怎么能这样莫名其妙地泼脏水呢）。



# 02、并发编程确实很难。

Bruce Eckel 吐露心声说自己也曾深陷“并发”泥潭，但经过“数月的努力，还是走了出来”。所以，各位，千万不要丧失驾驭并发编程的信心啊，尽管并发编程是真的难。

并发是什么呢？通常情况下，并发是指“系统能够同时并行处理很多请求”。我们来看一下并发常用的一些指标。

1）**响应时间**（Response Time）：系统从接收请求到做出回应所花费的时间。

2）**吞吐量**（Throughput）：单位时间内处理的请求数量。最明显的例子就是高速通道上的 ETC 和普通车道，显然 ETC 的吞吐量更大，因为不需要在进站的时候从窗口取卡，在出站的时候还卡缴费。

3）**并发用户数**：同时承载正常使用系统功能的用户数量。

如何提升系统的并发能力呢？

1）**提升单机硬件配置**。比如说增加 CPU 核数（从 2 个到 4 个，从 4 个到 8 个），升级网卡到万兆，升级硬盘为 SSD（固态硬盘，比普通硬盘读写更快、质量更轻、能耗更低、体积更小），扩充系统内存（从 64G 到 128G）。

2）**改善单机架构配置**。比如使用内存读写而不是每次都读写数据库。

3）**增加服务器数量**。单机性能总是有极限的，但服务器集群数量可以很庞大。

好了，本篇文章到此就要结束了。我从《Java 编程思想》的前言里读到了以上这些内容，你呢？


PS：这篇文章写于 2018 年 10 月 10 日，现在读起来感觉当时写得太烂了，但很适合作为《重学Java》系列文章的第一篇（毕竟开局嘛）。
