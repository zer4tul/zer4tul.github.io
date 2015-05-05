---
layout: post
title: "Mozilla 宣布淘汰 HTTP"
---

[Mozilla][mozilla] 在4月30日通过[官方博客](https://blog.mozilla.org/security/2015/04/30/deprecating-non-secure-http/)发布消息，计划淘汰 HTTP。

> There’s pretty broad agreement that HTTPS is the way forward for the web.  In recent months, there have been statements from [IETF](https://tools.ietf.org/html/rfc7258), [IAB](https://www.iab.org/2014/11/14/iab-statement-on-internet-confidentiality/) (even the [other IAB](http://www.iab.net/iablog/2015/03/adopting-encryption-the-need-for-https.html)), [W3C](https://w3ctag.github.io/web-https/), and the [US Government](https://https.cio.gov/) calling for universal use of encryption by Internet applications, which in the case of the web means HTTPS.

Mozilla 的计划具体包括两个方面：

1. 在某个特定日期（日期待定）之后，所有的新特性将只提供给 HTTPS 网站；
2. 逐步禁止使用 HTTP 协议的网站访问浏览器特性，尤其是那些与用户安全和隐私相关的特性。


## FAQ

由于消息发布之后，有大量的回帖。Mozilla 整理了一个 [FAQ][HTTPS FAQ] 以回答各种常见问题。译文稍晚放出。

## Let's Encrypt

2014年11月18日，Mozilla、思科、Akamai、IdenTrust、电子前哨基金会（EFF）和密歇根大学研究人员联合成立的 Internet Security Research Group（“ISRG”） [宣布](https://letsencrypt.org/2014/11/18/announcing-lets-encrypt.html)了 [Let's Encrypt CA][Let's Encrypt] 项目，该项目计划在2015年5月中旬正式发布。

[Let's Encrypt][Let's Encrypt] 的核心目标如下：

* **免费：** 任何域名所有者都可以零费用申请到一个针对其域名的有效证书。
* **自动：** 证书注册过程在服务器安装或配置过程能够简单的实现（对于配置过 HTTPS 的人来说这应该很值得期待）。更新过程将在后台自动执行。
* **安全：** Let’s Encrypt 将会提供业界先进的安全技术和最佳实践。
* **透明：** 所有关于证书发放、撤销的记录都会向任何需要查看的人开放。
* **开放：** 自动化发放和更新协议将会是开放标准，相应的软件也会尽可能开源软件。
* **合作：** 与现有的互联网协议相似，Let’s Encrypt 是一个社区联合行动，将助益于整个社区，不由任何一个单一组织控制。

从目前公布的 [SPEC](https://github.com/letsencrypt/acme-spec) 和 [Tech Review](https://letsencrypt.org/howitworks/technology/) 来看，与普通的免费 CA （例如 [StartSSL](https://www.startssl.com/?app=1) 和 [WoSign](https://buy.wosign.com/free/)）不同， [Let's Encrypt][Let's Encrypt] 还将极大的改善 HTTPS 部署难的问题。感觉 Mozilla 在下很大的一盘棋。

[mozilla]: https://www.mozilla.org
[HTTPS FAQ]: https://blog.mozilla.org/security/files/2015/05/HTTPS-FAQ.pdf
[Let's Encrypt]: https://letsencrypt.org
