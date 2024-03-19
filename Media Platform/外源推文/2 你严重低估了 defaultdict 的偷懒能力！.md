#### 前言

「外源推文」系列致力于分享其他公众号的优质内容，传递知识与价值。

这是本系列的第 2 篇推文，原文链接：

[你严重低估了defaultdict的偷懒能力！](http://mp.weixin.qq.com/s?__biz=MzU0NDc3MjM4Ng==&mid=2247491420&idx=1&sn=88dd13c6e33dae244a978b05b2babc15&chksm=fb764711cc01ce077bc84e739665640aaff3f5585ec647bb58b88b5808b2d1888116bd450629&scene=21#wechat_redirect)

「Python」中的 `defaultdict` 是标准库中提供的一种字典变种，它可以为字典的键指定默认值。

本文从源码的角度解析其多种用法，包括简化 if-else 判断、传入 lambda 以指定默认值、从已有字典构造新的默认字典，助力你的「Python」学习。

来源公众号：