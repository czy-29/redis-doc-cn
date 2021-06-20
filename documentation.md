文档
===

注意：Redis文档也在[redis-doc github仓库](http://github.com/redis/redis-doc)中以原始（计算机友好）的格式提供。Redis文档是在[Creative Commons Attribution-ShareAlike 4.0国际许可](https://creativecommons.org/licenses/by-sa/4.0/)下发布的。

使用Redis编程
---

* Redis实现的[命令的完整列表](https://redis.io/commands)，以及每个命令的完整文档。
* [管线](/topics/pipelining.md)：了解如何一次发送多个命令，
节省RTT时间。
* [Redis发布/订阅](topics/pubsub.md)：Redis是一个快速稳定的发布/订阅消息系统！一探究竟。
* [Redis Lua脚本](/commands/eval.md)：Redis Lua脚本功能文档。
* [调试Lua脚本](/topics/ldb.md)：Redis 3.2为Redis脚本引入了原生Lua调试器。
* [内存优化](/topics/memory-optimization.md)：了解Redis如何
使用RAM并学习一些技巧以减少它的使用。
* [过期](/commands/expire.md)：Redis允许为每个键设置不同的生存时间，以便键在到期时自动从服务器中删除。
* [Redis作为LRU缓存](/topics/lru-cache.md)：如何配置和使用Redis来作为具有固定内存量和自动淘汰键的缓存。
* [Redis事务](/topics/transactions.md)：可以将命令组合在一起，以便它们作为单个事务执行。
* [客户端缓存](/topics/client-side-caching.md)：从版本6开始，Redis支持服务器辅助客户端缓存。本文档介绍了如何使用它。
* [大量数据插入](/topics/mass-insert.md)：如何在短时间内将大量预先存在或生成的数据添加到Redis实例中。
* [分区](/topics/partitioning.md)：如何在多个Redis实例之间分配数据。
* [分布式锁](/topics/distlock.md)：使用Redis实现分布式锁管理器。
* [Redis键空间通知](/topics/notifications.md)：通过发布/订阅（Redis 2.8或更高版本）获得键空间事件的通知。
* [使用Redis创建二级索引](/topics/indexes.md)：使用Redis数据结构创建二级索引、组合索引和遍历图。

Redis模块API
---

* [Redis模块介绍](/topics/modules-intro.md)。开始学习Redis 4.0模块编程的好地方。
* [实现原生数据类型](/topics/modules-native-types.md)。模块可以实现看起来像内置数据类型的新数据类型（数据结构等）。本文档涵盖了用于实现它的API。
* 模块的[阻塞操作](topics/modules-blocking-ops.md)。这仍然是一个实验性的 API，但它非常强大，可以编写命令，阻塞客户端（不阻塞Redis），并且可以在其他线程中执行任务。
* [Redis模块API参考](topics/modules-api-ref.md)。直接从`src/module.c`源代码的顶部注释生成。包含许多关于API使用的底层细节。

教程 & FAQ
---

* [Redis数据类型介绍](/topics/data-types-intro.md)：这是学习Redis API和数据模型的一个很好的起点。
* [Redis流介绍](/topics/streams-intro.md)：Redis 5新数据类型Stream的详细描述。
* [使用PHP和Redis编写一个简单的Twitter克隆](/topics/twitter-clone.md)
* [Redis的自动完成](http://autocomplete.redis.io)
* [数据类型简短摘要](/topics/data-types.md)：Redis支持的不同类型值的简短摘要，不像本节中列出的第一个教程那样更新和信息丰富。
* [FAQ](/topics/faq.md)：关于Redis的一些常见问题。

管理
---
* [Redis-cli](/topics/rediscli.md)：了解如何掌握Redis命令行接口，您将经常使用它来管理、排除故障和试验Redis。
* [配置](/topics/config.md)：如何配置Redis。
* [Replication](/topics/replication.md)：设置
主-从复制需要了解的内容。
* [持久化](/topics/persistence.md)：了解配置
Redis持久性时的选项。
* [Redis管理](/topics/admin.md)：精选的管理相关话题。
* [安全](/topics/security.md)：Redis安全性概述。
* [Redis访问控制列表](/topics/acl.md)：从版本6开始，Redis支持ACL。可以配置用户只能运行选定的命令并只能访问特定的键模式。
* [加密](/topics/encryption.md)：如何加密Redis客户端-服务器通信。
* [信号处理](/topics/signals.md)：Redis如何处理信号。
* [连接处理](/topics/clients.md)：Redis如何处理客户端连接。
* [高可用性](/topics/sentinel.md)：Redis Sentinel是Redis的官方高可用解决方案。
* [延迟监控](/topics/latency-monitor.md)：Redis集成的延迟监控和报告功能有助于为低延迟工作负载调整Redis实例。
* [基准](/topics/benchmarks.md)：看看Redis在不同平台上有多快。
* [Redis发布](/topics/releases.md)：Redis开发周期和版本编号。

嵌入式和物联网
---

* [ARM和树莓派上的Redis](/topics/ARM.md)：从Redis 4.0开始，ARM和树莓派是官方支持的平台。此页面包含一般信息和基准测试。
* [可在此处找到适用于IoT和边缘计算的Redis参考实现](https://redislabs.com/redis-enterprise/redis-edge/)。

故障排除
---

* [Redis遇到问题？](/topics/problems.md)：Bug？高延迟？其他问题？使用[我们的问题疑难解答页面](/topics/problems.md)作为起点来查找更多信息。

Redis集群
---

* [Redis集群教程](/topics/cluster-tutorial.md)：Redis集群的简单介绍和设置指南。
* [Redis集群规范](/topics/cluster-spec.md)：Redis Cluster中使用的行为和算法的更正式的描述。

其他基于Redis的分布式系统
---

* [Redis CRDTs](https://redislabs.com/redis-enterprise/technology/active-active-geo-distribution/) Redis的active-active地理分布解决方案。
* [Roshi](https://github.com/soundcloud/roshi)是一个基于Redis的时间戳事件的大规模CRDT集实现，并使用Go实现。它最初是为[SoundCloud流](http://developers.soundcloud.com/blog/roshi-a-crdt-system-for-timestamped-events)开发的。

SSD和持久内存上的Redis
---

* [Redis on Flash](https://redislabs.com/redis-enterprise/technology/redis-on-flash/) by Redis Labs使用SSD和持久内存扩展了DRAM容量。

规范
---

* [Redis设计草案](/topics/rdd.md)：新提案的设计草稿。
* [Redis协议规范](/topics/protocol.md)：如果您正在实现
客户端，或者出于好奇，请从底层学习
如何与Redis通信。
* [Redis RDB格式](https://github.com/sripathikrishnan/redis-rdb-tools/wiki/Redis-RDB-Dump-File-Format)规范，以及[RDB版本历史](https://github.com/sripathikrishnan/redis-rdb-tools/blob/master/docs/RDB_Version_History.textile)。
* [内幕](/topics/internals.md)：了解有关Redis如何实现的幕后详细信息。

资源
---

* [Redis Cheat Sheet](http://www.cheatography.com/tasjaevan/cheat-sheets/redis/)：Redis的在线或可打印的函数参考。

用例
---
* [谁在使用Redis](/topics/whos-using-redis.md)

书籍
---

以下是已经出版的涉及Redis的书籍列表。图书按发行日期排序（较新的图书优先）。

* [Mastering Redis (Packt, 2016)](https://www.packtpub.com/big-data-and-business-intelligence/mastering-redis) by [Jeremy Nelson](https://www.packtpub.com/books/info/authors/jeremy-nelson).
* [Redis Essentials (Packt, 2015)](http://www.amazon.com/Redis-Essentials-Maxwell-Dayvson-Silva-ebook/dp/B00ZXFCFLO) by [Maxwell Da Silva](http://twitter.com/dayvson) and [Hugo Tavares](https://twitter.com/hltbra)
* [Redis in Action (Manning, 2013)](http://www.manning.com/carlson/) by [Josiah L. Carlson](http://twitter.com/dr_josiah) (early access edition).
* [Instant Redis Optimization How-to (Packt, 2013)](http://www.packtpub.com/redis-optimization-how-to/book) by [Arun Chinnachamy](http://twitter.com/ArunChinnachamy).
* [Instant Redis Persistence (Packt, 2013)](http://www.packtpub.com/redis-persistence/book) by Matt Palmer.
* [The Little Redis Book (Free Book, 2012)](http://openmymind.net/2012/1/23/The-Little-Redis-Book/) by [Karl Seguin](http://twitter.com/karlseguin) is a great *free* and concise book that will get you started with Redis.
* [Redis Cookbook (O'Reilly Media, 2011)](http://shop.oreilly.com/product/0636920020127.do) by [Tiago Macedo](http://twitter.com/tmacedo) and [Fred Oliveira](http://twitter.com/f).

以下书籍有Redis相关内容，但不是专门关于Redis的：

* [Seven databases in seven weeks (The Pragmatic Bookshelf, 2012)](http://pragprog.com/book/rwdata/seven-databases-in-seven-weeks).
* [Mining the Social Web (O'Reilly Media, 2011)](http://shop.oreilly.com/product/0636920010203.do)
* [Professional NoSQL (Wrox, 2011)](http://www.wrox.com/WileyCDA/WroxTitle/Professional-NoSQL.productCd-047094224X.html)

鸣谢
---

Redis由[Redis社区](/community.md)开发和维护。

该项目由[Salvatore Sanfilippo](http://twitter.com/antirez)创建、开发和维护，直到[2020年6月30日](http://antirez.com/news/133)。过去，[Pieter Noordhuis](http://twitter.com/pnoordhuis)和[Matt Stancliff](https://matt.sh)为Redis核心库和客户端库提供了大量代码和想法。

Redis贡献者的完整列表可以在[Github的Redis贡献者页面](https://github.com/redis/redis/graphs/contributors)中找到。然而，还有其他形式的贡献，例如想法、测试和错误报告。如果可能，贡献会在提交消息中得到确认。[邮件列表档案](http://groups.google.com/group/redis-db)和[Github问题页面](https://github.com/redis/redis/issues)是寻找活跃在Redis社区中提供想法和帮助其他用户的人的好来源。

赞助商
---

[Salvatore Sanfilippo](http://antirez.com)为开发Redis所做的工作由[Redis Labs](http://redislabs.com)赞助。Redis项目的其他赞助商和过去的赞助商列在[赞助商页面](/topics/sponsors.md)中。

许可证、商标和Logo
---

* Redis是在three clause BSD许可下发布的。您可以[在我们的许可页面中找到更多信息](/topics/license.md)。
* Redis商标和徽标归Redis Labs Ltd. 所有，请阅读[Redis商标指南](/topics/trademark.md)，了解我们有关使用Redis商标和徽标的政策。
