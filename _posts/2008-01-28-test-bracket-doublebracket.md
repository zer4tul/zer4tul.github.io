---
title: test，中括号，双中括号的效率
layout: post
---

bash的条件表达式中有三个几乎等效的符号和命令：test，[]和[[]]。通常，大家习惯用if [];then这样的形式。而[[]]的出现，根据ABS所说，是为了兼容><之类的运算符。如果仅仅是这样的话，在[]中加入就好了吧？刚才看见Bash Cures Cancer上的文章，比较了一下它们之间的效率，发现[[]]是最快的。

```
$ time for i in {1..100000}; do test -d .;done

real    0m0.658s
user    0m0.558s
sys     0m0.100s
```

```
$ time for i in {1..100000}; do [ -d . ];done

real    0m0.609s
user    0m0.524s
sys     0m0.085s
```

```
$ time for i in {1..100000}; do [[ -d . ]];done

real    0m0.311s
user    0m0.275s
sys     0m0.036s
```

看来，在不考虑对低版本bash和对sh的兼容的情况下，用[[]]是一个不错的选择。
