这个README只是一个 *快速入门* 文档。您可以在[redis.io](https://redis.io)上找到更详细的文档。

什么是Redis？
--------------

Redis通常被称为 *数据结构* 服务器。这意味着Redis通过一组命令提供对可变数据结构的访问，这些命令使用带有TCP套接字和简单协议的 *服务器-客户端* 模型发送。所以不同的进程可以以共享的方式查询和修改相同的数据结构。

Redis中实现的数据结构有一些特殊的属性：

* Redis关心如何将它们存储在磁盘上，即使它们总是被提供并修改到服务器的内存中。这意味着Redis速度快，但它也是非易失性的。
* 数据结构的实现强调内存效率，因此与使用高级编程语言建模的相同数据结构相比，Redis内部的数据结构可能使用更少的内存。
* Redis提供了许多在数据库中自然可以找到的功能，例如复制、持久性的可调级别、集群和高可用性。

另一个很好的例子是将Redis视为memcached的更复杂版本，其中的操作不仅仅是 SET 和 GET，而是处理复杂数据类型（如列表、集合、有序数据结构等）的操作。

如果您想了解更多，这些是适合起步的一些精选文章：

* Redis数据类型简介。[topics/data-types-intro.md](/topics/data-types-intro.md)
* 直接在浏览器中尝试Redis。https://try.redis.io
* Redis命令的完整列表。 https://redis.io/commands
* Redis官方文档中还有更多内容。[documentation.md](/documentation.md)

构建Redis
--------------

Redis可以在Linux、OSX、OpenBSD、NetBSD、FreeBSD上编译和使用。
我们支持大端和小端架构，以及32位
和64位系统。

它可以在Solaris派生系统（例如SmartOS）上编译，但我们
对这个平台的支持是 *尽最大努力的*，并且这个平台上的Redis不能保证
像在Linux、OSX和\*BSD中一样好。

它就像这样简单：

    % make

要使用TLS支持进行构建，您需要OpenSSL开发库（例如
Debian/Ubuntu 上的libssl-dev）并运行：

    % make BUILD_TLS=yes

要使用systemd支持进行构建，您需要systemd开发库（例
如Debian/Ubuntu上的libsystemd-dev或CentOS上的systemd-devel）并运行：

    % make USE_SYSTEMD=yes

要将后缀附加到Redis程序名称，请使用：

    % make PROG_SUFFIX="-alt"

您可以使用以下方法构建32位Redis二进制文件：

    % make 32bit

构建Redis后，最好使用以下方法对其进行测试：

    % make test

如果构建了TLS，则在启用TLS的情况下运行测试（您需要
安装 `tcl-tls`）：

    % ./utils/gen-test-certs.sh
    % ./runtest --tls


修复依赖项或缓存构建选项导致的构建问题
---------

Redis有一些依赖项，它们包含在 `deps` 目录中。
即使依赖项的源代码中的某些内容发生更改，
`make` 也不会自动重新构建依赖项。

当您使用 `git pull` 更新源代码或以任何其他方式
修改依赖关系树中的代码时，请确保使用以下
命令以真正清理所有内容并从头开始重建：

    make distclean

这将清理：jemalloc、lua、hiredis、linenoise。

此外，如果您强制某些构建选项，如32位目标、无C编译器
优化（用于调试目的）和其他类似的构建时选项，
这些选项将被无限期缓存，直到您发出 `make distclean`
命令。

修复构建32位二进制文件时的问题
---------

如果在使用32位目标构建Redis后您需要使用
64位目标重新构建它，或者相反，您需要在Redis发行版
的根目录中执行 `make distclean`。

如果在尝试构建32位Redis二进制文件时出现构建错误，请尝试
以下步骤：

* 安装软件包libc6-dev-i386（也可以尝试g++-multilib）。
* 尝试使用以下命令行代替 `make 32bit`：
  `make CFLAGS="-m32 -march=native" LDFLAGS="-m32"`

分配器
---------

在构建Redis时选择非默认内存分配器是通过设置
`MALLOC` 环境变量来完成的。默认系统情况下，Redis是使用libc
malloc编译和链接的，但例外是jemalloc是Linux系统上的
默认值。选择这个默认值是因为jemalloc已被证明比libc malloc具有
更少的碎片问题。

要强制编译libc malloc，请使用：

    % make MALLOC=libc

要在Mac OS X系统上使用jemalloc进行编译，请使用：

    % make MALLOC=jemalloc

单调时钟
---------------

默认情况下，Redis将使用POSIX clock_gettime函数作为
单调时钟源进行构建。在大多数现代系统上，内部处理器时钟
可用于提高性能。可以在此处找到注意事项：
    http://oliveryang.net/2015/09/pitfalls-of-TSC-usage/

要构建支持处理器内部指令时钟的版本，请使用：

    % make CFLAGS="-DUSE_PROCESSOR_CLOCK"

Verbose build（译者注：想不到好翻译，直接不翻译了……）
-------------

默认情况下，Redis将使用用户友好的彩色终端输出进行构建。
如果要查看更详细的输出，请使用以下命令：

    % make V=1

运行Redis
-------------

要使用默认配置运行Redis，只需键入：

    % cd src
    % ./redis-server

如果你想提供你的redis.conf，你必须使用一个额外的
参数（配置文件的路径）来运行它：

    % cd src
    % ./redis-server /path/to/redis.conf

可以通过使用命令行直接将参数作为选项传递
来更改Redis配置。例子如下：

    % ./redis-server --port 9999 --replicaof 127.0.0.1 6379
    % ./redis-server /etc/redis/6379.conf --loglevel debug

redis.conf中的所有选项也都支持作为命令行的选项
来使用，它们具有完全相同的名称。

使用TLS运行Redis：
------------------

请查阅 [TLS.md](https://github.com/redis/redis/blob/unstable/TLS.md) 文件以获取有关如何
将Redis与TLS一起使用的更多信息。

玩转Redis
------------------

您可以使用redis-cli来体验Redis。启动一个redis-server实例，
然后在另一个终端中尝试以下操作：

    % cd src
    % ./redis-cli
    redis> ping
    PONG
    redis> set foo bar
    OK
    redis> get foo
    "bar"
    redis> incr mycounter
    (integer) 1
    redis> incr mycounter
    (integer) 2
    redis>

您可以在 https://redis.io/commands 找到所有可用命令的列表。

安装Redis
-----------------

要将Redis二进制文件安装到/usr/local/bin中，只需使用：

    % make install

如果您希望使用不同的目标目录，您可以使用
`make PREFIX=/some/other/directory install`

Make install只会在您的系统中安装二进制文件，但不会在
适当的位置配置init脚本和配置文件。如果您只
想用一用Redis，则不需要这样做，但是如果您需要
以正确的方式为生产系统安装它，我们有一个脚本可以
为Ubuntu和Debian系统执行此操作：

    % cd utils
    % ./install_server.sh

_注意_：`install_server.sh` 不适用于Mac OSX；它仅针对Linux构建。

该脚本将询问您几个问题，并将设置您需要正确
运行Redis作为后台守护程序所需的一切，该后台守护程序将在系统重新启动时
再次启动。

您将能够使用名为 `/etc/init.d/redis_<portnumber>`
的脚本停止和启动Redis，例如 `/etc/init.d/redis_6379`。

代码贡献
-----------------

注意：通过以任何形式向Redis项目贡献代码，包括通过
Github 发送pull request、通过私人电子邮件或公共讨论组发送
代码片段或补丁，您同意根据BSD许可条款
发布您的代码，您可以 在Redis源代码分发中包含的 [COPYING][1] 文件中
找到该许可。

请参阅此源代码分发中的 [CONTRIBUTING][2] 文件以获取更多
信息。有关安全漏洞和弱点，请参阅 [SECURITY.md][3]。

[1]: https://github.com/redis/redis/blob/unstable/COPYING
[2]: https://github.com/redis/redis/blob/unstable/CONTRIBUTING
[3]: https://github.com/redis/redis/blob/unstable/SECURITY.md

Redis内部结构
===

如果您正在阅读本README，您很可能在Github页面上，
或者您刚刚解压了Redis发行版tar ball。在这两种情况下，
您基本上都离源代码一步之遥，因此我们在这里讲解
Redis源代码的布局，每个文件的大致内容，
Redis服务器内部最重要的函数和结构体等等。
我们将所有讨论保持在高层次上，而不深入研究细节，
因为否则这份文档会很大，而且我们的代码库会
不断变化，但总体思路应该是了解更多信息的
良好起点。此外，大部分代码都经过大量注释且易于
理解。

源代码布局
---

Redis根目录仅包含此README、调用
`src` 目录中的真实Makefile的Makefile，以及
Redis和Sentinel的示例配置。您可以找到一些用于
执行Redis、Redis集群和Redis Sentinel
单元测试的shell脚本，这些单元测试实现在 `tests`
目录中。

根目录内有以下重要子目录：

* `src`：包含用C编写的Redis实现。
* `tests`：包含用Tcl实现的单元测试。
* `deps`：包含Redis使用的依赖库。编译Redis所需的一切都在这个目录中；你的系统只需要提供 `libc`、一个POSIX兼容接口和一个C编译器。值得注意的是 `deps` 包含 `jemalloc` 的副本，它是Linux下Redis的默认分配器。请注意，在 `deps` 下也有从Redis项目开始的东西，但其主存储库不是 `redis/redis`。

还有一些目录，但它们对于我们这里的目标并不是很
重要。我们将主要关注 `src`，其中包含Redis实现，
我们将探索每个文件中的内容。文件展开的
顺序是遵循的逻辑顺序，以便逐步展开不同
层次的复杂性。

注意：最近Redis重构了很多。函数名和文件
名已更改，因此您可能会发现此文档更
接近地反映了 `unstable` 分支。例如，在Redis 3.0中，`server.c`
和 `server.h` 文件被命名为 `redis.c` 和 `redis.h`。但是整体
结构是一样的。请记住，所有新的开发和pull
requests都应该针对 `unstable` 分支执行。

server.h
---

了解程序如何工作的最简单方法是了解
它使用的数据结构。所以我们将从Redis的主头文件
`server.h` 开始。

所有服务器配置和大体上所有的共享状态都
定义在名为 `server` 的全局结构中，类型为 `struct redisServer`。
此结构体中的一些重要字段是：

* `server.db` 是一个Redis数据库数组，用于存储数据。
* `server.commands` 是命令表。
* `server.clients` 是所有连接到服务器的客户端的链表。
* `server.master` 是一个特殊的客户端，the master，如果实例是一个replica。

还有很多其他字段。大多数字段都直接在结构体定义
内写了注释。

另一个重要的Redis数据结构是定义客户端的数据结构。
过去它被称为 `redisClient`，现在只是 `client`。该结构体
有很多字段，这里我们只展示主要的字段：
```c
struct client {
    int fd;
    sds querybuf;
    int argc;
    robj **argv;
    redisDb *db;
    int flags;
    list *reply;
    // ... many other fields ...
    char buf[PROTO_REPLY_CHUNK_BYTES];
}
```
client结构体定义了一个 *已连接的客户端*：

* `fd` 字段是客户端套接字文件描述符。
* `argc` 和 `argv` 用客户端正在执行的命令填充，以便使实现了给定Redis命令的函数可以读取该命令的参数。
* `querybuf` 累积来自客户端的请求，这些请求由Redis服务器根据Redis协议进行解析，并通过调用客户端正在执行的命令的实现来执行。
* `reply` 和 `buf` 是动态和静态缓冲区，用于累积服务器发送给客户端的回复。一旦文件描述符可写，这些缓冲区就会增量地写入套接字。

正如您在上面的client结构体中所见，命令中的参数
被描述为 `robj` 结构体。以下是完整的 `robj`
结构体，它定义了一个 *Redis对象*：

    typedef struct redisObject {
        unsigned type:4;
        unsigned encoding:4;
        unsigned lru:LRU_BITS; /* lru time (relative to server.lruclock) */
        int refcount;
        void *ptr;
    } robj;

基本上这个结构可以表示所有基本的Redis数据类型，如
字符串、列表、集合、有序集合等。有趣的是
它有一个 `type` 字段，这样就可以知道给定对象的类型
是什么，还有一个 `refcount`，这样同一个对象可以在多个
地方被引用，而无需多次分配它。最后，`ptr`
字段指向对象的实际表示，即使对于
相同的类型，它也可能会有所不同，具体取决于所使用的 `encoding`。

Redis对象在Redis内部被广泛使用，但是为了
避免间接访问的开销，最近在许多地方
我们只使用未包装在Redis对象中的纯动态字符串。

server.c
---

这是Redis服务器的入口点，`main()` 函数
就在此定义。以下是启动Redis服务器
最重要的一些步骤。

* `initServerConfig()` 设置 `server` 结构的默认值。
* `initServer()` 分配用于操作、设置监听套接字等所需的数据结构的空间。
* `aeMain()` 启动监听新连接的事件循环。

事件循环会定期调用两个特殊函数：

1. `serverCron()` 被定期调用（根据 `server.hz` 频率），并执行必须不时执行的任务，例如检查已超时的客户端。
2. `beforeSleep()` 会在每次触发事件循环时都被调用，Redis会服务一些请求，然后返回到事件循环中。

在server.c中，您可以找到处理Redis服务器其他重要事项的代码：

* `call()` 用于在给定客户端的上下文中调用给定命令。
* `activeExpireCycle()` 处理通过 `EXPIRE` 命令设置的TTL的键的淘汰。
* `performEvictions()` 会当应执行新的写入命令，但根据 `maxmemory` 指令Redis内存不足时被调用。
* 全局变量 `redisCommandTable` 定义了所有Redis命令，指定命令的名称、实现命令的函数、所需的参数数量以及每个命令的其他属性。

networking.c
---

这个文件定义了所有与客户端、主节点和副本
（在Redis中只是特殊客户端）有关的I/O功能：

* `createClient()` 分配并初始化一个新客户端。
* 命令的实现使用 `addReply*()` 系列函数将数据附加到client结构体，该数据将作为对执行给定命令的回复传输到客户端。
* `writeToClient()` 将输出缓冲区中未决的数据传输到客户端，并由 *可写事件处理程序* `sendReplyToClient()` 调用。
* `readQueryFromClient()` 是 *可读事件处理程序*，它将从客户端中读取的数据累积到查询缓冲区中。
* `processInputBuffer()` 是根据Redis协议解析客户端查询缓冲区的入口点。一旦命令准备好被处理，该函数就会调用在 `server.c` 中定义的 `processCommand()` 以实际执行命令。
* `freeClient()` 解除分配、断开连接并移除客户端。

aof.c and rdb.c
---

从名字可以猜到，这些文件实现了Redis的RDB和AOF
持久化。Redis使用基于 `fork()` 系统调用的
持久性模型来创建一个与主Redis进程具有相同（共享）
内存内容的进程。此辅助进程将内存
内容转储到磁盘上。`rdb.c` 使用它在磁盘上
创建快照，`aof.c` 使用它来在“仅追加文件”变得
太大时执行AOF重写。

`aof.c` 内部的实现具有附加功能，以
实现一个API，该API允许客户端执行可将新命令追加
到AOF文件中的命令。

`server.c` 中定义的 `call()` 函数负责调用
将命令写入AOF的函数。

db.c
---

某些Redis命令对特定数据类型进行操作；其他的则是通用命令。
通用命令的示例是 `DEL` 和 `EXPIRE`。它们对键进行操作，
而不是专门对它们的值进行操作。所有这些通用命令都
在 `db.c` 中定义。

此外，`db.c` 实现了一个API，以便在不直接访问
内部数据结构的情况下对Redis数据集执行某些操作。

`db.c` 中，许多命令实现都用到了的，最重要的一些
函数如下：

* `lookupKeyRead()` 和 `lookupKeyWrite()` 用于获取指向与给定键关联的值的指针，如果键不存在则为 `NULL`。
* `dbAdd()` 及其更高层级的对应 `setKey()` 负责在Redis数据库中创建一个新键。
* `dbDelete()` 删除一个键及其关联的值。
* `emptyDb()` 删除整个的单个数据库或所有定义过的数据库。

文件的其余部分实现了暴露给客户端的通用命令。

object.c
---

定义了Redis对象的 `robj` 结构体已经在前面描述过了。在
`object.c` 中，包含了操作Redis对象的所有
基础级别函数，例如分配新对象、处理引用计数
等的函数。此文件中值得注意的函数有：

* `incrRefCount()` 和 `decrRefCount()` 用于增加或减少对象的引用计数。当它降到0时，对象最终被释放。
* `createObject()` 分配一个新对象。还有专门的函数来分配具有特定内容的字符串对象，例如 `createStringObjectFromLongLong()` 等类似函数。

该文件还实现了 `OBJECT` 命令。

replication.c
---

这是Redis中最复杂的文件之一，建议
在对代码库的其余部分稍微熟悉之后再接触它。
在这个文件中，有Redis的master和replica角色
的实现。

这个文件中最重要的函数之一是 `replicationFeedSlaves()`，它向代表了连接到我们主服务器的副本实例的
客户端写入命令，以便副本实例可以运行客户端执行的写入：
这样他们的数据集将与主服务器中的数据集保持同步。

该文件还实现了 `SYNC` 和 `PSYNC` 命令，
用于在master和replica之间执行第一次
同步，或在断开连接后继续复制。

其它C文件
---

* `t_hash.c`、`t_list.c`、`t_set.c`、`t_string.c`、`t_zset.c` 和 `t_stream.c` 包含Redis数据类型的实现。它们实现了访问给定数据类型的API，以及这些数据类型的客户端命令实现。
* `ae.c` 实现了Redis事件循环，它是一个自包含的库，易于阅读和理解。
* `sds.c` 是Redis字符串库，查看 [sds.md](/sds.md) 了解更多信息。
* `anet.c` 是一个相比内核公开的原始接口，可以以更简单的方式使用POSIX网络的库。
* `dict.c` 是一个非阻塞哈希表的实现，它以增量方式来进行重新哈希。
* `scripting.c` 实现Lua脚本。它是完全独立的并且与Redis实现的其余部分隔离，如果您熟悉Lua API，它很容易理解。
* `cluster.c` 实现了Redis集群。可能只有在非常熟悉Redis代码库的其余部分后才能很好地阅读。如果您想阅读 `cluster.c`，请务必阅读 [Redis Cluster规范][4]。

[4]: /topics/cluster-spec.md

Redis命令剖析
---

所有Redis命令都按以下方式定义：

    void foobarCommand(client *c) {
        printf("%s",c->argv[1]->ptr); /* Do something with the argument. */
        addReply(c,shared.ok); /* Reply something to the client. */
    }

然后在命令表中的 `server.c` 中引用该命令：

    {"foobar",foobarCommand,2,"rtF",0,NULL,0,0,0,0,0},

在上面的示例中，`2` 是命令采用的参数数量，
而 `"rtF"` 是命令标志，如 `server.c` 中的命令表
顶部注释中所述。

命令以某种方式运行后，它会向客户端返回一个回复，
通常使用 `addReply()` 或在 `networking.c` 中定义的类似函数。

Redis源代码中有大量命令实现，
可以作为实际命令实现的示例。编写
一些玩具命令是熟悉代码库的一个很好的练习。

还有很多其他的文件这里没有介绍，但是全部覆盖
也没有用。我们只是想帮助您完成入门。
最终你会在Redis代码库中找到自己的理解 :-)

Enjoy！
