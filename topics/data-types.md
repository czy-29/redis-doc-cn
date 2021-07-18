数据类型
===

<a name="strings"></a>
字符串
---

字符串是最基本的Redis值。Redis字符串是二进制安全的，这意味着Redis字符串可以包含任何类型的数据，例如
JPEG 图像或序列化的Ruby对象。

字符串值的最大长度为512MB。

你可以在Redis中使用字符串做很多有趣的事情，例如你可以：

* 使用INCR系列命令（[INCR](/commands/incr.md)、[DECR](/commands/decr.md)、[INCRBY](/commands/incrby.md)）将字符串用作原子计数器。
* 使用 [APPEND](/commands/append.md) 命令追加字符串。
* 使用 [GETRANGE](/commands/getrange.md) 和 [SETRANGE](/commands/setrange.md) 将字符串用作随机访问向量。
* 在很小的空间内编码大量数据，或者使用 [GETBIT](/commands/getbit.md) 和 [SETBIT](/commands/setbit.md) 创建一个Redis支持的布隆过滤器。

查看所有 [可用的字符串命令](https://redis.io/commands#string) 以获取更多信息，或阅读 [Redis数据类型简介](/topics/data-types-intro.md)。

<a name="lists"></a>
列表
---

Redis列表只是字符串列表，按插入顺序排序。
可以将元素添加到Redis列表，将新元素推送到列表的头部（左侧）或尾部（右侧）。

[LPUSH](/commands/lpush.md) 命令在头部插入一个新元素，而
[RPUSH](/commands/rpush.md) 命令在尾部插入一个新元素。当对空键
执行这些操作之一时，会创建一个新列表。
类似地，如果列表操作将清空列表，则键将从键空间中
被删除。这些是非常方便的语义，因为如果使用不存在的键作为
参数调用，所有列表命令的行为将与使用空列表调用时
完全相同。

列表操作和结果列表的一些示例：

    LPUSH mylist a   # 现在列表是“a”
    LPUSH mylist b   # 现在列表是“b”，“a”
    RPUSH mylist c   # 现在列表是“b”、“a”、“c”（这次使用了 RPUSH）

列表的最大长度为2^32 - 1个元素（4294967295，每个列表超过40亿个元素）。

从时间复杂度的角度来看，Redis Lists的主要特点是
支持常量时间插入和删除靠近头部和尾部的
元素，即使插入了数百万个条目。
访问列表两端附近的元素非常快，但
如果您尝试访问非常大的列表的中间部分，则速度很慢，因为它是
O(N) 操作。

你可以用Redis列表做很多有趣的事情，例如你可以：

* 为社交网络中的时间线建模，使用 [LPUSH](/commands/lpush.md) 以在用户时间线中添加新元素，并使用 [LRANGE](/commands/lrange.md) 以检索一些最近插入的条目。
* 您可以将 [LPUSH](/commands/lpush.md) 与 [LTRIM](/commands/ltrim.md) 一起使用来创建一个列表，该列表永远不会超过给定的元素数量，但只记住最新的N个元素。
* 列表可以用作消息传递原语，例如参见众所周知的用于创建后台作业的 [Resque](https://github.com/resque/resque) Ruby 库。
* 你可以用列表做更多的事情，这种数据类型支持许多命令，包括像 [BLPOP](/commands/blpop.md) 这样的阻塞命令。

请查看所有 [可用的操作列表的命令](https://redis.io/commands#list) 以获取更多信息，或阅读 [Redis数据类型简介](/topics/data-types-intro.md)。

<a name="sets"></a>
集合
---

Redis Sets是一个无序的字符串集合。可以在O(1)时间复杂度内
添加、删除和测试成员的存在（恒定时间，
与Set中包含的元素数量无关）。

Redis集合具有不允许重复成员的理想属性。多次添加相同的元素将导致集合仍然具有该元素的单个副本。实际上，这意味着添加成员不需要 *检查是否存在然后添加* 的操作。

Redis Sets非常有趣的一点是，它们支持许多
服务器端命令用于从现有集合开始计算集合，因此您
可以在很短的时间内完成集合的并、交、差。

一个集合中的最大成员数为2^32 - 1（4294967295，每个集合超过40亿个成员）。

你可以使用Redis Sets做很多有趣的事情，例如你可以：

* 您可以使用Redis Sets跟踪唯一的事物。想知道访问给定博客文章的所有唯一IP地址吗？您只需要在每次处理页面视图时使用 [SADD](/commands/sadd.md)。您可以确定重复的IP不会被插入。
* Redis Sets可以很好地表示关系。您可以使用Redis创建一个标签系统，使用一个Set来表示每个标签。然后，您可以使用 [SADD](/commands/sadd.md) 命令将具有给定标签的所有对象的所有ID添加到表示此特定标签的Set中。您是否希望所有对象的所有ID同时具有三个不同的标签？只需使用 [SINTER](/commands/sinter.md)。
* 您可以通过 [SPOP](/commands/spop.md) 或 [SRANDMEMBER](/commands/srandmember.md) 命令使用Sets随机提取元素。


像往常一样，查看 [Set命令的完整列表](https://redis.io/commands#set) 以获取更多信息，或阅读 [Redis数据类型简介](/topics/data-types-intro.md)。

<a name="hashes"></a>
哈希
---

Redis哈希是字符串字段和字符串值之间的映射，因此它们是表示对象的完美数据类型（例如，具有多个字段的用户，如姓名、姓氏、年龄等）：

    HMSET user:1000 username antirez password P1pp0 age 34
    HGETALL user:1000
    HSET user:1000 password 12345
    HGETALL user:1000

包含很少几个字段的散列（很少意味着最多一百个左右）以占用很少空间
的方式存储，因此您可以在一个小型Redis实例中存储
数百万个对象。

虽然哈希主要用于表示对象，但它们能够存储许多元素，因此您也可以将哈希用于许多其他任务。

每个哈希最多可以存储2^32 - 1个字段-值对（超过40亿）。

查看 [Hash命令的完整列表](https://redis.io/commands#hash) 以获取更多信息，或阅读 [Redis数据类型简介](/topics/data-types-intro.md)。

<a name="sorted-sets"></a>
有序集合
---

Redis Sorted Sets类似于Redis Sets，是非重复的字符串
集合。不同之处在于，Sorted Set的每个成员都与分数
相关联，用于使有序集合从最小
到最大分数排序。虽然成员是独一无二的，但分数可能会
重复。

使用有序集合，您可以非常快速地添加、删除或更新元素
（时间与元素数量的对数成正比）。由于
元素是 *按顺序获取的*，而不是在之后排序，因此您还可以通过
分数或排名（位置）以非常快的方式获取一个范围的元素。
访问有序集合的中间也非常快，因此您可以将
有序集合用作没有重复元素的智能列表，您可以在其中快速访问
所需的一切：按顺序排列的元素，快速存在性测试，中间
元素的快速访问！

简而言之，使用有序集合，您可以以出色的性能完成许多在
其他类型的数据库中很难建模的任务。

使用有序集合，您可以：

* 在大型在线游戏中获取排行榜，每次提交
新分数时，您都使用 [ZADD](/commands/zadd.md) 对其进行更新。您可以使用
[ZRANGE](/commands/zrange.md) 轻松获取排名靠前的用户，您还可以根据
用户名，使用 [ZRANK](/commands/zrank.md) 返回其在列表中的排名。
将 ZRANK 和 ZRANGE 一起使用，您可以显示与给定用户
分数相似的用户。一切都非常 *快*。
* 有序集合经常用于索引存储在Redis中的数据。
例如，如果您有许多表示用户的散列，则可以使用以用户年龄作为分数，以用户ID作为值的有序集合。因此，使用 [ZRANGEBYSCORE](/commands/zrangebyscore.md) 检索具有给定年龄间隔的所有用户将是举手之劳且快速的。


Sorted Sets可能是最高级的Redis数据类型，所以花一些时间查看 [Sorted Set命令的完整列表](https://redis.io/commands#sorted_set)，以发现你可以用Redis做什么！您可能还想阅读 [Redis数据类型简介](/topics/data-types-intro.md)。

Bitmaps和HyperLogLogs
---

Redis还支持Bitmaps和HyperLogLogs，它们实际上是基于
String 基础类型的数据类型，但具有自己的语义。

有关这些类型的信息，请参阅 [Redis数据类型简介](/topics/data-types-intro.md)。
