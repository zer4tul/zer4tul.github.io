---
layout: post
title: "中点（·）在UTF-8和GBK转换中的问题"
tags: ["encoding"]
---

在翻译外国人名的时候会用到中点（“·”，大陆和台湾称为“间隔号”），例如“理查德·斯托曼”。偶尔也会用在广告词、电影名或者书籍章节名中，例如：“如果·爱”。

在实际使用过程中，对含有中点的文件进行字符集转换时，经常会出现转换失败的情况。这种情况通常出现于以下场景：

* UTF-8 -> GBK
* GB2312 <-> GBK
* UTF-8 -> BIG5

# 转换失败的原因

通常情况下，字符集转换失败是由于文本使用的字符在目标字符集中没有对应字符的编码造成的。

| 符号 | UNICODE | UNICODE 名称 | GB2312 | GBK | GB18030 | BIG5 |
| :--: | :--: | :------: | :------: | :--------: | :--------: | :------------ |
| · | U+00B7 | [MIDDLE DOT][MIDDLE DOT] | - | A1 A4 | A1 A4 | A1 50	|
| • | U+2022 | [BULLET][BULLET] | - | - | 81 36 A6 31 | A1 45	|
| ． | U+FF0E | [FULLWIDTH FULL STOP][FULLWIDTH FULL STOP] | A3 AE | A3 AE | A3 AE | A1 44	|
| ・ | U+30FB | [KATAKANA MIDDLE DOT][KATAKANA MIDDLE DOT]  | A1 A4	| - | 81 39 A7 39 | - |

**注意：**

* GB2312 中“A1 A4”代表的是 **U+30FB katakana middle dot**
* GBK、GB18030 中“A1 A4”代表的是 **U+00B7 middle dot**
* GBK 中 **没有** U+30FB KATAKANA MIDDLE DOT 对应的字符编码

基于以上原因，GBK <-> GB2312 字符集转换的时候如果包含看起来像是“中点”的东西，**转换失败的概率极高**。

<!-- more -->

# 形似中点的符号

在 UNICODE 中有多个外形类似的符号，列表如下：

| 符号 | UNICODE | UNICODE 名称 |  UTF-8 | 备注 |
| :--: | :------: | :--------: | :--------: | :------------ |
| · | U+00B7 | [MIDDLE DOT][MIDDLE DOT]       | C2 B7 | 中点，中国大陆[《标点符号用法》][标点符号用法]（GB/T 15834-2011）规定的中文间隔号 |
| · | U+0387 | [GREEK ANO TELEIA][GREEK ANO TELEIA] | CE 87 | 希腊文分号（ánō stigmē）|
| ּ	| U+05BC | [HEBREW POINT DAGESH OR MAPPIQ][HEBREW POINT DAGESH OR MAPPIQ] | D6 BC | 希伯来文点号（dagesh或mapiq），希伯来语是 **从右自左书写** 的，这个字符使用在大量文本编辑器、字处理器（例如 Microsoft Word ）中与从左自右书写的文字（例如英文、中文）混用会出现奇怪的行为 |
| ᛫ | U+16EB | [RUNIC SINGLE PUNCTUATION][RUNIC SINGLE PUNCTUATION] | E1 9B AB | [卢恩字母](http://zh.wikipedia.org/wiki/%E7%9B%A7%E6%81%A9%E5%AD%97%E6%AF%8D)标点，常见字体中不包含这个字符 |
| • | U+2022 | [BULLET][BULLET]  | E2 80 A2 | 项目符号，常用于列表项 |
| ‧ | U+2027 | [HYPHENATION POINT][HYPHENATION POINT] | E2 80 A7 | 连字点，台湾地区[《重订标点符号手册》修订版][《重订标点符号手册》修订版]规定的中文间隔号 |
| ∘ | U+2218 | [RING OPERATOR][RING OPERATOR] | E2 88 98 | ring 运算符（数学） |
| ∙ | U+2219 | [BULLET OPERATOR][BULLET OPERATOR] | E2 88 99 | bullet 运算符（数学） |
| ⋅ | U+22C5 | [DOT OPERATOR][DOT OPERATOR] | E2 8B 85 | dot 运算符（数学） |
| ◦ | U+25E6 | [WHITE BULLET][WHITE BULLET]  | E2 97 A6 | 中空的项目符号 |
| ⦁ | U+2981 | [Z NOTATION SPOT][Z NOTATION SPOT]  | E2 A6 81 | 用于Z notation的符号 |
| ⸰ | U+2E30 | [RING POINT][RING POINT] | E2 B8 B0 | 阿维斯陀文标点 |
| ⸱  | U+2E31 | [WORD SEPARATOR MIDDLE DOT][WORD SEPARATOR MIDDLE DOT]  | E2 B8 B0 | 分词中点 |
| ． | U+FF0E | [FULLWIDTH FULL STOP][FULLWIDTH FULL STOP]  | EF BC 8E | 全角英文句号，源自 Big5、GB 2312、JIS X 0208 等字符集 |
| ・ | U+30FB | [KATAKANA MIDDLE DOT][KATAKANA MIDDLE DOT]  | E3 83 BB | 片假名中点（全角） |
| ･ | U+FF65 | [HALFWIDTH KATAKANA MIDDLE DOT][HALFWIDTH KATAKANA MIDDLE DOT]  | EF BD A5 | 半角片假名中点，源自 JIS X 0208 字符集 |
| 𐄁 | U+10101 | [AEGEAN WORD SEPARATOR DOT][AEGEAN WORD SEPARATOR DOT]  | F0 90 84 81 | 爱琴海地区古文字的分词中点 |

**注意：**

* 以上字符在 GB18030、UTF-8、UTF-16 中都有对应的编码
* 在 GB2312、GBK、BIG5 中，除前表列出的以外，其余均没有对应的编码。

# Reference

1. [Fileformat.info: Unicode Character Search](http://www.fileformat.info/info/unicode/char/search.htm)
2. [Wikipedia: Interpunct](http://en.wikipedia.org/wiki/Interpunct)（[中文版](http://zh.wikipedia.org/wiki/%E9%97%B4%E9%9A%94%E5%8F%B7)）


[KATAKANA MIDDLE DOT]: http://www.fileformat.info/info/unicode/char/30fb/index.htm
[MIDDLE DOT]:  http://www.fileformat.info/info/unicode/char/b7/index.htm
[BULLET]:  http://www.fileformat.info/info/unicode/char/2022/index.htm
[FULLWIDTH FULL STOP]:  http://www.fileformat.info/info/unicode/char/ff0e/index.htm
[KATAKANA MIDDLE DOT]:  http://www.fileformat.info/info/unicode/char/30fb/index.htm
[GREEK ANO TELEIA]:  http://www.fileformat.info/info/unicode/char/0387/index.htm
[HEBREW POINT DAGESH OR MAPPIQ]:  http://www.fileformat.info/info/unicode/char/05bc/index.htm
[RUNIC SINGLE PUNCTUATION]:  http://www.fileformat.info/info/unicode/char/16eb/index.htm
[HYPHENATION POINT]: http://www.fileformat.info/info/unicode/char/2027/index.htm
[RING OPERATOR]: http://www.fileformat.info/info/unicode/char/2218/index.htm
[BULLET OPERATOR]: http://www.fileformat.info/info/unicode/char/2219/index.htm
[DOT OPERATOR]: http://www.fileformat.info/info/unicode/char/22c5/index.htm
[WHITE BULLET]: http://www.fileformat.info/info/unicode/char/25e6/index.htm
[Z NOTATION SPOT]: http://www.fileformat.info/info/unicode/char/2981/index.htm
[RING POINT]:  http://www.fileformat.info/info/unicode/char/2e30/index.htm
[WORD SEPARATOR MIDDLE DOT]:  http://www.fileformat.info/info/unicode/char/2e31/index.htm
[HALFWIDTH KATAKANA MIDDLE DOT]: http://www.fileformat.info/info/unicode/char/ff65/index.htm
[AEGEAN WORD SEPARATOR DOT]: http://www.fileformat.info/info/unicode/char/10101/index.htm

[标点符号用法]: http://www.sac.gov.cn/SACSearch/search?channelid=160591&templet=gjcxjg_detail.jsp&searchword=STANDARD_CODE=%27GB/T%2015834-2011%27&XZ=T
[《重订标点符号手册》修订版]:http://www.edu.tw/files/site_content/M0001/hau/h14.htm
