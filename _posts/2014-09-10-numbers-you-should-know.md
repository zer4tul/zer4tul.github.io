---
layout: post
title : "Numbers You Should Know"
---------------------------------

Latency Numbers
---------------

| Project                            | Time<br>( ns ) | Time<br>( ms ) | Description                 |
|------------------------------------|:--------------:|:--------------:|:---------------------------:|
| L1 cache reference                 |      0.5       |                |                             |
| Branch mispredict                  |       5        |                |                             |
| L2 cache reference                 |       7        |                |        14x L1 cache         |
| Mutex lock/unlock                  |       25       |                |                             |
| Main memory reference              |      100       |                | 20x L2 cache, 200x L1 cache |
| Compress 1K bytes with Zippy       |     3,000      |                |                             |
| Send 1K bytes over 1 Gbps network  |     10,000     |      0.01      |                             |
| Read 4K randomly from SSD*         |    150,000     |      0.15      |                             |
| Read 1 MB sequentially from memory |    250,000     |      0.25      |                             |
| Round trip within same datacenter  |    500,000     |      0.5       |                             |
| Read 1 MB sequentially from SSD*   |   1,000,000    |       1        |          4X memory          |
| Disk seek                          |   10,000,000   |       10       |  20x datacenter roundtrip   |
| Read 1 MB sequentially from disk   |   20,000,000   |       20       |     80x memory, 20X SSD     |
| Send packet CA->Netherlands->CA    |  150,000,000   |      150       |                             |

Cache & Memory Latency in CPU Cycles
------------------------------------

根据 [Intel 官方论坛的讨论](https://software.intel.com/en-us/forums/topic/287236)，Sandy Bridge 架构 Intel CPU 的 Cache 时间可以做如下运算。

| Cache status                              | Cycles costs    |
|:------------------------------------------|:----------------|
| L1 CACHE hit                              | ~4 cycles       |
| L2 CACHE hit                              | ~10 cycles      |
| L3 CACHE hit, line unshared               | ~40 cycles      |
| L3 CACHE hit, shared line in another core | ~65 cycles      |
| L3 CACHE hit, modified in another core    | ~75 cycles      |
| remote L3 CACHE                           | ~100-300 cycles |
| Local main memory reference               | ~60 ns          |
| Remote main memory reference              | ~100 ns         |

Operation Costs
---------------

以下数据基于2.3GHz 双核 Xeon。

| Project                                    | Time<br>( ns ) | Time<br>( ms ) |
|--------------------------------------------|:--------------:|:--------------:|
| int bit                                    |      0.5       |                |
| int add                                    |      0.2       |                |
| int multi                                  |      1.5       |                |
| int div                                    |       10       |                |
| int mod                                    |       11       |                |
| int64 bit                                  |      0.5       |                |
| int64 add                                  |      0.2       |                |
| int64 multi                                |      1.5       |                |
| int64 div                                  |       20       |                |
| int64 mod                                  |       20       |                |
| float add                                  |      1.5       |                |
| float multi                                |      2.0       |                |
| float div                                  |       8        |                |
| double add                                 |      1.5       |                |
| double multi                               |      2.0       |                |
| double div                                 |       12       |                |
| context switch ( 2 processes 16K per one)  |     1,500      |      1.5       |
| context switch ( 2 processes 64K per one)  |     1,600      |      1.5       |
| context switch ( 8 processes 16K per one)  |     2,000      |      2.0       |
| context switch ( 8 processes 64K per one)  |     2,500      |      2.5       |
| context switch ( 16 processes 16K per one) |     2,100      |      2.1       |
| context switch ( 16 processes 64K per one) |     2,700      |      2.7       |

Notes
-----

- Assuming ~1GB/sec SSD
- Context switch 与系统当前负载情况强相关，无法准确测定。以上数值请做参考。

Credits
-------

- [Latency Numbers](#Latency Numbers)
  - By [Jeff Dean](http://research.google.com/people/jeff/)
  - Originally by [Peter Norvig](http://norvig.com/21-days.html#answers)
- Other versions of [Latency Numbers](#Latency Numbers)
  - [Some updates from https://gist.github.com/2843375](https://gist.github.com/2843375)
  - [Great 'humanized' comparison version](https://gist.github.com/2843375)
  - [Nice animated presentation of the data](http://prezi.com/pdkvgys-r0y6/latency-numbers-for-programmers-web-development/)
  - [Interactive version](http://www.eecs.berkeley.edu/~rcs/research/interactive_latency.html)
- [Ping times between cities](https://wondernetwork.com/pings)
- [Performance Analysis Guide for Intel® Core™ i7 Processor and Intel® Xeon™ 5500 processors](https://software.intel.com/sites/products/collateral/hpc/vtune/performance_analysis_guide.pdf)
