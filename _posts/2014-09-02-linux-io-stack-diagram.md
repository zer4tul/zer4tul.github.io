---
title: Linux I/O Stack Diagram
layout: post
---

[Brendan Gregg](https://twitter.com/brendangregg) 在他的 [Linux Performance Tools][linux_performance_tools] 中给出了分析 Linux 性能问题的工具集图并在上述链接中维持更新。

![Linux Performance Tools](/images/linux_observability_tools.png)

图中基本涵盖了常见问题的常用分析工具。与 IO 相关的性能分析由于明显的瓶颈点和各种复杂的关系（cache、buffer、io scheduler 等等）是各种数据处理类应用的重中之重。

[Thomas Krenn Wiki][thomas_krenn_wiki] 发布了基于3.17版内核的 [Linux I/O Stack Diagram][linux_io_stack_diagram]，在进行 IO 性能分析时非常有参考价值。

![Linux I/O Stack Diagram v3.17](/images/Linux-storage-stack-diagram_v3.17.png)

[thomas_krenn_wiki]: http://www.thomas-krenn.com/en/wiki/Main_Page
[linux_io_stack_diagram]: http://www.thomas-krenn.com/en/wiki/Linux_I/O_Stack_Diagram
[linux_performance_tools]: http://www.brendangregg.com/linuxperf.html
