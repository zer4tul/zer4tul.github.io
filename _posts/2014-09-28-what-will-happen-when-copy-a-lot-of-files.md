---
layout: post
title: "cp 大量文件时可能耗光内存"
tags: ["linux", "tools", "operation"]
---

8月11日的时候有用户在 GNU coreutils 邮件组发[邮件][origin_mail]反馈，自己在拷贝大量文件（4.32亿，39 TB）的时候发现所有空闲内存被用光。

这个问题目前暂未修复，预计在GNU coreutils 下一个版本（8.24）修复。当前 [patch][patch]在 8.23版本可用（更早版本没试过，理论上可用）。

**注意：** 这一问题在 mv 命令被转化为 cp （例如跨文件系统 mv 文件）的时候也会有影响。

<!-- more -->
## 原因

cp 的代码中使用如下结构体保存目标信息：

```c
struct Src_to_dest
   {
     ino_t st_ino; // sizeof(ino_t) == 8
     dev_t st_dev; // sizeof(dev_t) == 4
     char *name;
   };
```

其中 st_ino 和 st_dev 占用12字节，目标文件名占用实际长度的内存。

由于 cp 使用了 Hash_tuning （hash.c）的默认值：

```c
/* [...] The growth threshold defaults to 0.8, and the growth factor defaults to 1.414, meaning that the table will have doubled its size every second time 80% of the buckets get used.  */
#define DEFAULT_GROWTH_THRESHOLD 0.8f
#define DEFAULT_GROWTH_FACTOR 1.414f
```

最坏的情况下可能造成 Src_to_dest_hash 的实际占用空间呈约2倍增长，即：

- 通常情况下文件名的长度在 10个字符以内，最坏情况下，Src_to_dest_hash 中，每个文件的平均空间占用将达到约40字节
- 在邮件作者遇到的场景中，内存占用量为：432M * 40 Bytes = 17 GB

## 解决

1. 如前所述，本邮件的作者已经提供了 [patch][patch]。预计 upstream 将在8.24版本中修复
2. 如果想继续用老版本的 coreutils，请使用 xargs 之类的工具将文件分批拷贝（或移动）

```bash
find . -name "foo*" -print0 | xargs -0 cp -t bar/
```

[origin_mail]:http://lists.gnu.org/archive/html/coreutils/2014-08/msg00012.html
[patch]: http://git.savannah.gnu.org/cgit/coreutils.git/commit/?id=65d8e6906ae8752358b4f96153f7a1c5ccec3789
