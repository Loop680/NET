---
date: 2012-04-02 14:13:20+00:00
layout: post
title: '[转载]逆向一条 MD5-32 大概需要多少运算能力'
thread: 640
categories: 文档
tags: 逆向工程
---

起因大家都懂得，这里只是做个简单的计算，判断什么的我不会写的。
大前提是：MD5 是迄今为止没有算法上的太大漏洞，而且要求是逆向（不是寻找碰撞）。

1. 彩虹表有用吗？

在这个特定的情形下，没有。解释如下：

MD5 是一种摘要散列算法，任意大小的原文都会转化为定长的密文。
彩虹表是一种预先计算的原文到密文的映射，如果密文在彩虹表的覆盖空间里，那么就可以反查到原文。而原文空间理论上无限大，完全运算并存储这些映射非常消耗资源，所以，彩虹表更多的是一种“空间时间消耗都可以接受”的算法加上数据集合。

很显然，某个 MD5 运算结果不可能在常见的彩虹表覆盖空间里。
（注，一个9位以内数字组合的彩虹表大概要接近 1TB 而由于算法链式验证的原因，实际计算次数要数倍于穷举运算，参见[ http://project-rainbowcrack.com/table.htm]( http://project-rainbowcrack.com/table.htm) ）

2. 爆破怎么做？<!-- more -->

爆破就是拼运算力了。
手机号字段大概有 1e7 种组合，而后面还有随机字符串，不知道有多长，姑且算作 5 位吧，字母数字组合，就简化作 1e8 种组合好了。
现在假设你运气足够好，其他位全都猜对了，比如你知道要把 v2ex 逆序之类的，需要验证 1e15 次吧。
（这是一个非常简单的估算，纯粹估算下限）

3. GPU 通用运算能力到底有多强？

最强的通用计算单元 ATI HD6990 大概运算力是 1e10 次，cpu 相对于 gpu 小太多数量级，所以可以不考虑了。
假如你能够调用 100 显卡的某超算集群，一个小时的运算力大概是 1e15 次验证。
（参见[ http://golubev.com/gpuest.htm]( http://golubev.com/gpuest.htm) ）

4. 一些细节

￥ 这个字符是 unicode 字符，而 md5 并没有针对 unicode 的特定实现。
实际的实现是先转换成 utf8/utf16/ansi 等等编码形式再进行计算。
（某些特定程序支持传入 unicode 字符串，但依旧需要内部进行编码转换）
这一条的意义是说，如果加密者的编码是 utf8 而你的运算程序是 ansi 编码，那就不用想了，永远都不会有结果。

MD5 算法是 16-byte 分段，可以认为运算时间正比于原文长度，目测待解密的原文是 32-byte 左右，即实际运算能力可能要减半。

（假设非常多的时候，参见[ http://en.wikipedia.org/wiki/Occam's_razor ]( http://en.wikipedia.org/wiki/Occam's_razor )）

转自：[http://www.v2ex.com/t/29152](http://www.v2ex.com/t/29152)

点评：技术贴，有兴趣看看原文下面的评论