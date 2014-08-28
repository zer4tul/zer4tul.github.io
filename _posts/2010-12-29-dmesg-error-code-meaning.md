---
layout: post
title: "dmesg error code 的含义"
category: posts
---

dmesg 里报出来的最后一个 error 的意思是：

| BIT位 | 0 | 1 |
| :---: | :---: | :---: |
| 0 | no page found | protection fault |
| 1 | read | write|
| 2 | kernel | user-mode|
| 3 | NULL | fault was an instruction fetch |

dmesg 的错误号就是这四个位的组合，例如：

>  segfault at 0000000000000010 rip 000000552aaaf8ac rsp 0000000041408a80 error 6

| 0 | 1 | 1 | 0 |
| :----: | :----: | :----: | :----: |
| NULL | user-mode | write | no page found |

是指用户空间写 page 时 page not found
