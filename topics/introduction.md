Redis简介
===

Redis是一个开源（BSD协议）的内存**数据结构存储**，可用作数据库、缓存和消息中间件。Redis提供了诸如
[字符串](/topics/data-types-intro.md#strings)、[散列](/topics/data-types-intro.md#hashes)、[列表](/topics/data-types-intro.md#lists)、[集合](/topics/data-types-intro.md#sets)、带范围查询的[有序集合](/topics/data-types-intro.md#sorted-sets)、[位图](/topics/data-types-intro.md#bitmaps)、[hyperloglogs](/topics/data-types-intro.md#hyperloglogs)、[地理空间索引](/commands/geoadd.md)和[流](/topics/streams-intro.md)等数据结构。Redis内置[replication](/topics/replication.md)、[Lua脚本](/commands/eval.md)、[LRU淘汰](/topics/lru-cache.md)、[事务](/topics/transactions.md)和不同级别的[磁盘持久化存储](/topics/persistence.md)，并通过[Redis哨兵](/topics/sentinel.md)和可自动分区的[Redis集群](/topics/cluster-tutorial.md)提供高可用性。

您可以对这些类型执行**原子操作**，
例如[附加到字符串](/commands/append.md)；
[增加散列中的值](/commands/hincrby.md)；[将元素推入列表](/commands/lpush.md)；
[计算集合交集](/commands/sinter.md)，
[并集](/commands/sunion.md)和[差集](/commands/sdiff.md)；
或
[获取有序集中排名最高的成员](/commands/zrangebyscore.md)。

为了获得最佳性能，Redis使用
**内存数据集**。根据您的用例，您可以通过
定期[将数据集转储到磁盘](/topics/persistence.md#snapshotting)
或
[将每个命令附加到基于磁盘的日志](/topics/persistence.md#append-only-file)来保留数据。如果您只需要一个功能丰富的网络内存缓存，您也可以禁用持久性。

Redis还支持[异步复制](/topics/replication.md)，具有非常快的非阻塞首次同步、网络断开时可自动重连并部分地重新同步。

其他功能包括：

* [事务](/topics/transactions.md)
* [发布/订阅](/topics/pubsub.md)
* [Lua脚本](/commands/eval.md)
* [具有有限TTL的键](/commands/expire.md)
* [键的LRU淘汰](/topics/lru-cache.md)
* [自动故障转移](/topics/sentinel.md)

您可以在[大多数编程语言](https://redis.io/clients)中使用Redis。

Redis是用**ANSI C**编写的，可在大多数POSIX系统（如Linux、
\*BSD和OS X）中运行，无需外部依赖。Linux和OS X是Redis开发和测试最多的两个操作系统，我们**推荐使用Linux进行部署**。Redis可以在SmartOS等Solaris派生系统中工作，但支持是*尽力而为*的。
没有对Windows构建的官方支持。
