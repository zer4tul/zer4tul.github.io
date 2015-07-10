---
layout: post
title: "Mozilla 宣布计划逐步淘汰 HTTP"
---

[Mozilla][mozilla] 在4月30日通过[官方博客](https://blog.mozilla.org/security/2015/04/30/deprecating-non-secure-http/)发布消息，计划淘汰 HTTP。

> There’s pretty broad agreement that HTTPS is the way forward for the web.  In recent months, there have been statements from [IETF](https://tools.ietf.org/html/rfc7258), [IAB](https://www.iab.org/2014/11/14/iab-statement-on-internet-confidentiality/) (even the [other IAB](http://www.iab.net/iablog/2015/03/adopting-encryption-the-need-for-https.html)), [W3C](https://w3ctag.github.io/web-https/), and the [US Government](https://https.cio.gov/) calling for universal use of encryption by Internet applications, which in the case of the web means HTTPS.

Mozilla 的计划具体包括两个方面：

1. 在某个特定日期（日期待定）之后，所有的新特性将只提供给 HTTPS 网站；
2. 逐步禁止使用 HTTP 协议的网站访问浏览器特性，尤其是那些与用户安全和隐私相关的特性。


## Let's Encrypt

2014年11月18日，Mozilla、思科、Akamai、IdenTrust、电子前哨基金会（EFF）和密歇根大学研究人员联合成立的 Internet Security Research Group（“ISRG”） [宣布](https://letsencrypt.org/2014/11/18/announcing-lets-encrypt.html)了 [Let's Encrypt CA][Let's Encrypt] 项目，该项目计划在2015年9月正式发布。

[Let's Encrypt][Let's Encrypt] 的核心目标如下：

* **免费：** 任何域名所有者都可以零费用申请到一个针对其域名的有效证书。
* **自动：** 证书注册过程在服务器安装或配置过程能够简单的实现（对于配置过 HTTPS 的人来说这应该很值得期待）。更新过程将在后台自动执行。
* **安全：** Let’s Encrypt 将会提供业界先进的安全技术和最佳实践。
* **透明：** 所有关于证书发放、撤销的记录都会向任何需要查看的人开放。
* **开放：** 自动化发放和更新协议将会是开放标准，相应的软件也会尽可能开源软件。
* **合作：** 与现有的互联网协议相似，Let’s Encrypt 是一个社区联合行动，将助益于整个社区，不由任何一个单一组织控制。

从目前公布的 [SPEC](https://github.com/letsencrypt/acme-spec) 和 [Tech Review](https://letsencrypt.org/howitworks/technology/) 来看，与普通的免费 CA （例如 [StartSSL](https://www.startssl.com/?app=1) 和 [WoSign](https://buy.wosign.com/free/)）不同， [Let's Encrypt][Let's Encrypt] 还将极大的改善 HTTPS 部署难的问题。感觉 Mozilla 在下很大的一盘棋。

## FAQ

由于消息发布之后，有大量的回帖。Mozilla 整理了一个 [FAQ][HTTPS FAQ] 以回答各种常见问题。

译文如下：

<!-- more -->

在我们宣布了我们[计划推进 HTTPS](https://blog.mozilla.org/security/2015/04/30/deprecating-non-secure-http/) 之后，收到了许多关于 HTTPS 现状以及 Web 开发者可能面临的相关后果的问题。这份 FAQ 旨在回答其中最常见的疑问。

**Q. 这是否意味着我的未加密网站将无法工作？**

相当长一段时间内，未加密网站仍然可以工作。将 Web 转换到 HTTPS 需要一些时间。我们要做的第一件事情是让新特性只支持 HTTPS。所以，无论你的网站现在啥样，它仍然可以在未来的几个月甚至几年内正常工作。

长远来说，现在已经有关于移除或限制某些已有浏览器特性在未加密网站上使用的讨论。这些改变将会在真正执行前公布，以使你有时间来更新你的网站，使其不依赖于这些特性，或者使用 HTTPS（我们更希望如此）。这些变更只会在 Web 社区在功能性和安全性上找到了平衡点，并且做出确切结论的时候才会开始执行。

**Q. 你们为什么强制我去买证书？这对于小网站来说是很难啊！**

如果你想使用 HTTPS，你需要用到证书。但是这并不代表你必须去买！市面上有好几个免费证书供应商（例如 [StartSSL](https://www.startssl.com/?app=1)， [WoSign](https://buy.wosign.com/free/)，稍后还会有 [Let's Encrypt][Let's Encrypt]）。有些 Web 平台供应商也免费提供证书（例如 [Cloudflare](https://blog.cloudflare.com/introducing-universal-ssl/)）。对于跑在自己服务器上的站点，Mozilla 已经提供了一个 [HTTPS 配置生成器](https://mozilla.github.io/server-side-tls/ssl-config-generator/)。

**Q. HTTPS 会使我的网站变慢吗？**

HTTPS 本质上来说是 HTTP 加上加密，因此使用 HTTPS 会有一定的性能损耗。但是对于现在的平台来说，这个损耗[非常非常小](https://istlsfastyet.com/)。

对许多网站来说，事实上加密是通向 **更好** 性能的必经之路。HTTP/2 相比 HTTP/1.1 有[显著的性能提升](https://http2.akamai.com/)，并且在所有浏览器上 HTTP/2 都只对加密网站可用。

**Q. 为什么你们在 CA 系统这么龊的情况下，对推进 HTTPS 这么上心？**

尽管 CA 系统存在缺陷，它仍然是整个在线交易的基础之一。它在某些时候表现得很龊，但是大多数情况下，还是能用的。

我们一直以来都在尝试通过诸如清除[行为不当的 CA](https://blog.mozilla.org/security/2015/04/27/removing-e-guven-ca-certificate/)，强化信息披露要求等方式来改进 Mozilla CA 方案的质量。我们也关注更新的技术（比如 [DANE](http://en.wikipedia.org/wiki/DNS-based_Authentication_of_Named_Entities) 和 [CT](http://www.certificate-transparency.org/)）以在长期的时间里提升身份验证手段。即使这些改进措施还需要一段时间才能看到成效，现有的 PKI系统（译注：[Public Key Infrastructure，公开密钥基础架构](http://zh.wikipedia.org/wiki/%E5%85%AC%E9%96%8B%E9%87%91%E9%91%B0%E5%9F%BA%E7%A4%8E%E5%BB%BA%E8%A8%AD)）也已经足够推动 HTTPS 了。

**Q. 这不是让人更难过，并且会限制言论自由么？**

如前所述，行业趋势是 HTTPS 越来越容易布署。对于老旧的内容，也有 HSTS 和 upgrade-­insecure­-requests 来保证平滑迁移。

关于言论自由，值得关注的一点是最近的许多反审查制度研究都聚焦在[使通信变得更隐秘](https://cdt.org/files/2015/02/CDT-comments-on-the-use-of-encryption-and-anonymity-in-digital-communcations.pdf)上（例如 [SecureDrop](https://securedrop.org/)）。因此推进使用加密看起来对保护言论自由更有利。

**Q. 开发/协作环境如何？**

你可以配置浏览器以适应这些场景。这类场景中浏览器强制执行的“安全”概念将会通过 W3C 的[定义特权上下文（Privileged contexts）](http://www.w3.org/TR/powerful-features/)规格来定义，我们将会提供本地策略配置 —— 就是说，用户可以通过配置指定一个特定的上下文能够被完全信任。结合现有的[添加信任根节点](https://wiki.wmtransfer.com/projects/webmoney/wiki/Installing_root_certificate_in_Mozilla_Firefox)机制，开发者或者 IT 人员应当能够便捷的设置一个安全环境，就像 [Mozilla 这样](https://wiki.mozilla.org/MozillaRootCertificate)。

**Q. 但是我的网站上没有什么秘密！为什么我要折腾加密？**

HTTPS 并不仅仅是加密。它还提供完整性，使你的站点内容不会被篡改。还有身份验证，使用户知道他们是连接到你的服务而不是其他的攻击者。以上三者任意缺少一个，都会导致问题。对安全机制的合理运用将会阻止：

* [Comcast 在用户的 Web 数据流中注入广告](http://arstechnica.com/tech-policy/2014/09/08/why-comcasts-javascript-ad-injections-threaten-security-net-neutrality/)
* [AT&T 追踪他们的用户浏览习惯](https://gigaom.com/2015/02/19/dont-let-att-mislead-you-about-its-29-privacy-fee/)
* [Great Cannon of China 的攻击使 Github 掉线](https://gigaom.com/2015/02/19/dont-let-att-mislead-you-about-its-29-privacy-fee/)

此外，只要你的网站不安全，它就有可能被利用来攻击你的用户或其他网站。不安全的网站越多，对整个 Web 的安全威胁越大。

**Q. 如果你们这么在乎安全性，为什么还如此排斥自签名证书？**

自签名证书本质并不坏。如果不考虑手工核对签名有效性的开销的话，它可能比 CA 颁发的证书更安全。

为啥浏览器会弹出那么吓人的提示呢？问题在于，浏览器并不知道它什么时候应当接受一个自签名证书，什么时候应当接受一个 CA 颁发的证书。实际上只有很小一部分合法站点使用自签名证书，因为手工验证真的太难了。这就使得浏览器必须假定，默认地认为它需要得到 CA 颁发的证书，而自签名的证书被认为是可疑的。同样的，我们不能指望大多数用户能够认真查看证书的细节并评判证书的合法性，因此浏览器必须尝试代替用户做出一个靠谱的决定。

如果你告诉浏览器某些自签名证书可以接受，那也是可行的。可以通过在高级设定里设置（属性 > 高级 > 查看证书 > 服务器），或者点击弹出的警告提示的方式来进行。当然，如果你对如何提升这方面的用户体验有想法，请[联系我们](https://lists.mozilla.org/listinfo/dev-security)。

**Q. 那我家的路由器咋办？或者我的打印机咋办？**

这里的挑战并不在于这些机器不能通过 HTTPS 方式访问，而是他们根本没有提供证书。大多数情况下，是因为这些设备并没有一个全球唯一的名称，因此不能像网站的证书那样给它们颁发证书。我们需要有一个更好的技术来解决这部分问题，并且我们也正在跟一些设备制造商讨论如何改善。

还需要注意的是，我们的计划本质上是渐进的，因此我们还有时间来进行改进。如之前提到的，现在能够正常使用的所有的东西，在相当长一段时间内仍然能够正常工作，因此我们还有时间来解决这个问题。

**Q. HTTPS 是不是，呃……真的很棒？**

是的。


[mozilla]: https://www.mozilla.org
[HTTPS FAQ]: https://blog.mozilla.org/security/files/2015/05/HTTPS-FAQ.pdf
[Let's Encrypt]: https://letsencrypt.org
