# Redis为什么这么快

Redis之所以快速的原因主要包括以下几点：

1. **内存存储：**Redis将数据存储在内存中，实现了快速的读写操作。
2. **单线程模型：**Redis采用单线程处理请求，避免了多线程的竞争和上下文切换开销。
3. **高效的数据结构：**Redis内部使用了高效的数据结构，如哈希表、跳跃表等，提供了快速的数据访问和操作。
4. **异步IO：**Redis利用异步IO来处理网络请求，能够同时处理多个请求，提高并发性能。
5. **事件驱动架构：**Redis基于事件驱动的模型，通过事件循环机制处理请求和操作，提高系统的效率。
6. **优化的操作：**Redis对常用操作进行了优化，如批量操作和管道技术，减少了网络通信开销。

综上所述，Redis之所以快速在于内存存储、单线程模型、高效的数据结构、异步IO、事件驱动架构和优化的操作等因素的综合作用。这使得Redis能够以高性能和高响应速度处理各类数据操作请求。





详：

在高并发下， 大家都知道要用多线程，  为什么Redis反其道而行之用单线程。  那单线程不是会导致串行从而影响吞吐量吗？

首先，  redis的瓶颈不在 CPU，  它直接从内存中拿完数据就直接通过网络来传输，中间不需要任何计算。



![1732265354980-5677deb2-f2d5-474f-924a-d890437940b1.png](img/TzJ5gU6_myZQKQLB/1732265354980-5677deb2-f2d5-474f-924a-d890437940b1-328877.png)





高效的数据结构，进一步提升操作内存的性能

![1732258925728-7d6d5a81-96c4-4078-b7fd-7502d7124386.gif](img/TzJ5gU6_myZQKQLB/1732258925728-7d6d5a81-96c4-4078-b7fd-7502d7124386-592326.gif)



所以对于 **<font style="color:#DF2A3F;">Redis 这种高频小操作的场景，单线程的效率反而更高</font>**。它的性能更多取决于内存访问速度和网络 I/O，而不是 CPU 的多核能力。那这样的话，单线程的设计就显得非常合理了。 因为对于内存操作来说，速度本身就非常快。 多线程反而会带来了额外的复杂性，上下文切换等问题造成额外的性能开销。 



<font style="color:rgb(24, 25, 28);">Redis为什么不用b+树？MySQL为什么不用跳表？ </font>

1.redis没有这么多数据。

2.B+树是自平衡树， 每次插入数据都会自动维护树和排序，为了就是减少树的高度和将数据排好序。

2.mysql需要通过B+树减少磁盘ioc次数， redis直接操作内存，内存比磁盘上万倍，多跳几次问题不大。 

![1732265462209-32360405-a69f-4660-af27-62c2998e9e11.png](img/TzJ5gU6_myZQKQLB/1732265462209-32360405-a69f-4660-af27-62c2998e9e11-393125.png)



+ **单线程是怎么处理多个连接的？**

![1732258344616-63966f50-f9f4-42cd-a7f9-771d8453fe8e.gif](img/TzJ5gU6_myZQKQLB/1732258344616-63966f50-f9f4-42cd-a7f9-771d8453fe8e-331147.gif)

通过IO多路复用，     把连接都管理起来， 谁需要执行命令就执行谁的。

![1732258576904-eaf01fcd-cbdb-4d10-8041-b5b6f498b1c0.gif](img/TzJ5gU6_myZQKQLB/1732258576904-eaf01fcd-cbdb-4d10-8041-b5b6f498b1c0-544759.gif)



![1732265614403-016b5175-da5b-4b8e-9203-4552c7708678.png](img/TzJ5gU6_myZQKQLB/1732265614403-016b5175-da5b-4b8e-9203-4552c7708678-389006.png)

# 短视频

Redis为什么这么快？这个问题面试已经被**问烂了**，但是真正答得好的人并不多，大多数同学能回答三四个点已经不错了，其实完整的原因有**五个点，**看看你知道几个

<font style="color:rgb(38, 38, 38);">第一，Redis的数据是基于</font>**<font style="color:rgb(38, 38, 38);">内存存储</font>**<font style="color:rgb(38, 38, 38);">的，读写操作也是基于内存，那性能当然高了。</font>

<font style="color:rgb(38, 38, 38);">第二，Redis采用</font>**<font style="color:rgb(38, 38, 38);">单线程</font>**<font style="color:rgb(38, 38, 38);">处理请求，避免了多线程的竞争和上下文切换开销。  
</font><font style="color:rgb(38, 38, 38);">第三，Redis内部使用了</font>**<font style="color:rgb(38, 38, 38);">高效的数据结构</font>**<font style="color:rgb(38, 38, 38);">，如哈希表、跳跃表等等，基于这些数据结构能实现快速的数据访问和操作。  
</font><font style="color:rgb(38, 38, 38);">第四，Redis采用</font>**<font style="color:rgb(38, 38, 38);">IO多路复用模型</font>**<font style="color:rgb(38, 38, 38);">，能够同时处理大量连接请求，提高并发性能，当然这里说的并发是指处理客户端连接以及对连接的响应，内部命令执行还是单线程的。  
</font><font style="color:rgb(38, 38, 38);">第五，Redis对</font>**<font style="color:rgb(38, 38, 38);">常用命令</font>**<font style="color:rgb(38, 38, 38);">操作进行了很多</font>**<font style="color:rgb(38, 38, 38);">优化</font>**<font style="color:rgb(38, 38, 38);">，如批量操作命令以及管道操作等，大大减少了网络通信的开销。  
</font><font style="color:rgb(38, 38, 38);">当然，对于以上回答里面的一些细节，面试官可能会深挖，比如</font>**<font style="color:rgb(38, 38, 38);">跳跃表的实现</font>**<font style="color:rgb(38, 38, 38);">，</font>**<font style="color:rgb(38, 38, 38);">IO多路复用底层原理</font>**<font style="color:rgb(38, 38, 38);">等，</font>关于这几个深挖问题想听的同学可以点赞关注，后面马上安排！

> 更新: 2024-12-17 15:31:26  
> 原文: <https://www.yuque.com/tulingzhouyu/db22bv/teinr2zgfcc96u97>