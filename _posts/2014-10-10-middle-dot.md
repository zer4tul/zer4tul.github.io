---
layout: post
title: "中点（·）在UTF-8和GBK转换中的问题"
---

在翻译外国人名的时候会用到中点（“·”，大陆和台湾称为“间隔号”），例如“理查德·斯托曼”。偶尔也会用在广告词、电影名或者书籍章节名中，例如：“如果·爱”。

在实际使用过程进行字符集转换时，经常会出现转换失败的情况。这种情况通常出现于以下场景：

* UTF-8 -> GBK
* GB2312 <-> GBK
* UTF-8 -> BIG5

# 转换失败的原因

通常情况下，编码转换失败是由于文本使用的字符在目标编码中没有对应的字符造成的。

| 符号 | UNICODE | GB2312 | GBK | GB18030 | BIG5 |
| :--: | :--: | :------: | :--------: | :--------: | :------------ |
| · | U+00B7 middle dot | - | A1 A4 | A1 A4 | A1 50	|
| • | U+2022 bullet | - | - | 81 36 A6 31 | A1 45	|
| ． | U+FF0E fullwidth full stop | A3 AE | A3 AE | A3 AE | A1 44	|
| ・ | U+30FB katakana middle dot  | A1 A4	| - | 81 39 A7 39 | - |

**注意**：

* GB2312 中“A1 A4”代表的是 **U+30FB katakana middle dot**
* GBK、GB18030 中“A1 A4”代表的是 **U+00B7 middle dot**
* GBK 中 **没有** U+30FB katakana middle dot 对应的字符空间

基于以上原因，GBK <-> GB2312 字符集转换的时候如果包含看起来像是“中点”的东西，**转换失败的概率极高**。

<!--more-->

# 形似中点的符号

在 UNICODE 中有多个外形类似的符号，列表如下：

| 符号 | UNICODE | UTF-8 | 备注 |
| :--: | :------: | :--------: | :------------ |
| · | U+00B7 middle dot       | C2 B7 | 中点，中国大陆[《标点符号用法》][标点符号用法]（GB/T 15834-2011）规定的中文间隔号 |
| · | U+0387 greek ano teleia | CE 87 | 希腊文分号（ánō stigmē）|
| ּ	| U+05BC hebrew point dagesh or mappiq | D6 BC | 希伯来文点号（dagesh或mapiq），希伯来语是 **从右自左书写** 的，这个字符使用在大量文本编辑器、字处理器（例如 Microsoft Word ）中与从左自右书写的文字（例如英文、中文）混用会出现奇怪的行为 |
| ᛫ | U+16EB runic single punctuation | E1 9B AB | [卢恩字母](http://zh.wikipedia.org/wiki/%E7%9B%A7%E6%81%A9%E5%AD%97%E6%AF%8D)标点，常见字体中不包含这个字符 |
| • | U+2022 bullet | E2 80 A2 | 项目符号，常用于列表项 |
| ‧ | U+2027 hyphenation point | E2 80 A7 | 连字点，台湾地区[《重订标点符号手册》修订版][《重订标点符号手册》修订版]规定的中文间隔号 |
| ∘ | U+2218 ring operator | E2 88 98 | ring 运算符（数学） |
| ∙ | U+2219 bullet operator | E2 88 99 | bullet 运算符（数学） |
| ⋅ | U+22C5 dot operator | E2 8B 85 | dot 运算符（数学） |
| ◦ | U+25E6 white bullet  | E2 97 A6 | 中空的项目符号 |
| ⦁ | U+2981 z notation spot  | E2 A6 81 | 用于Z notation的符号 |
| ⸰ | U+2E30 ring point | E2 B8 B0 | 阿维斯陀文标点 |
| ⸱  | U+2E31 word separator middle dot  | E2 B8 B0 | 分词中点 |
| ． | U+FF0E fullwidth full stop  | EF BC 8E | 全角英文句号，源自 Big5、GB 2312、JIS 等标准 |
| ・ | U+30FB katakana middle dot  | E3 83 BB | 片假名中点（全角） |
| ･ | U+FF65 halfwidth katakana middle dot  | EF BD A5 | 半角片假名中点，源自 JIS 编码 |
| 𐄁 | U+10101 aegean word separator dot  | F0 90 84 81 | 爱琴海地区古文字的分词中点 |

以上字符在 GB18030、UTF-8、UTF-16 中都有对应的空间，而在 GB2312、GBK、BIG5 中，除前表列出的以外，其余均没有对应的空间。

[KATAKANA MIDDLE DOT]:http://www.fileformat.info/info/unicode/char/30fb/index.htm
[MIDDLE DOT]: http://www.fileformat.info/info/unicode/char/b7/index.htm
[标点符号用法]: http://www.sac.gov.cn/SACSearch/search?channelid=160591&templet=gjcxjg_detail.jsp&searchword=STANDARD_CODE=%27GB/T%2015834-2011%27&XZ=T
[《重订标点符号手册》修订版]:http://www.edu.tw/files/site_content/M0001/hau/h14.htm
