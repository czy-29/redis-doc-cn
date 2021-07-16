Redis快速入门
===

这是一个快速入门文档，面向没有
Redis 经验的人。阅读本文档将帮助您：

* 下载并编译Redis以开始体验。
* 使用 **redis-cli** 访问服务器。
* 在您的应用程序中使用Redis。
* 了解Redis持久性的工作原理。
* 更正确地安装Redis。
* 了解接下来要阅读的内容以了解有关Redis的更多信息。

安装Redis
===

安装Redis的建议方法是从源代码编译它，因为
除了能正常工作的GCC编译器和libc之外没有任何依赖项。
不鼓励使用Linux发行版的包管理器安装它，
因为通常可用的版本不是最新的。

您可以从 [redis.io](https://redis.io) 网站下载最新的Redis tar ball，也可以使用这个始终指向最新稳定Redis版本的特殊URL，即：[http://download.redis.io/redis-stable.tar.gz](http://download.redis.io/redis-stable.tar.gz).

为了编译Redis，请遵循以下简单步骤：

    wget http://download.redis.io/redis-stable.tar.gz
    tar xvzf redis-stable.tar.gz
    cd redis-stable
    make

此时，您可以通过键入 **make test** 来测试您的构建是否正常工作，但这是一个可选步骤。编译后，Redis发行版中的 **src** 目录填入了作为Redis一部分的不同可执行文件：

* **redis-server** 是Redis服务器本身。
* **redis-sentinel** 是Redis Sentinel可执行文件（监控和故障转移）。
* **redis-cli** 是与Redis通信的命令行实用程序。
* **redis-benchmark** 用于检查Redis性能。
* **redis-check-aof** 和 **redis-check-rdb**（在3.0及以下版本中是**redis-check-dump**) 在数据文件损坏的罕见事件中很有用。

最好将Redis服务器和命令行程序都复制到适当的位置，要么使用以下命令手动复制：

* sudo cp src/redis-server /usr/local/bin/
* sudo cp src/redis-cli /usr/local/bin/

要么只是使用 `sudo make install`。

在以下文档中，我们假设/usr/local/bin位于您的 PATH 环境变量中，以便您可以在不指定完整路径的情况下执行这两个二进制文件。

启动Redis
===

启动Redis服务器的最简单方法是不带任何参数执行 **redis-server** 二进制文件。

    $ redis-server
    [28550] 01 Aug 19:29:28 # Warning: no config file specified, using the default config. In order to specify a config file use 'redis-server /path/to/redis.conf'
    [28550] 01 Aug 19:29:28 * Server started, Redis version 2.2.12
    [28550] 01 Aug 19:29:28 * The server is now ready to accept connections on port 6379
    ... more logs ...

在上面的示例中，Redis是在没有任何显式配置文件的情况下启动的，因此所有参数都将使用内部默认值。
如果您启动Redis只是为了玩玩或用于开发，这完全没问题，但对于生产环境，您应该使用配置文件。

为了使用配置文件启动Redis，请使用配置文件的完整路径作为第一个参数，如下例所示：**redis-server /etc/redis.conf**。您应该使用Redis源代码分发的根目录中包含的 `redis.conf` 文件作为模板来编写您的配置文件。

检查Redis是否正常工作
=========================

外部程序使用TCP套接字和Redis特定协议与Redis通信。该协议在Redis客户端库中针对不同的编程语言都有实现。然而，为了能更简单地体验Redis，Redis提供了一个命令行实用程序，可用于向Redis发送命令。这个程序叫做 **redis-cli**。

为了检查Redis是否正常工作，首先要做的是使用redis-cli发送 **PING** 命令：

    $ redis-cli ping
    PONG

运行 **redis-cli** 后跟一个命令名称及其参数会将此命令发送到在localhost上运行的Redis实例的6379端口。您可以更改redis-cli使用的主机和端口，只需尝试使用 --help 选项查看命令行的使用信息即可。

运行redis-cli的另一种有趣方式是不带参数：程序将以交互模式启动，您可以键入不同的命令并查看它们的回复。

    $ redis-cli
    redis 127.0.0.1:6379> ping
    PONG
    redis 127.0.0.1:6379> set mykey somevalue
    OK
    redis 127.0.0.1:6379> get mykey
    "somevalue"

到这里您可以与Redis对话了。现在是时候暂停本教程并开始 [Redis数据类型的15分钟介绍](/topics/data-types-intro.md)，以便学习一些Redis命令。如果您已经知道一些基本的Redis命令，则可以继续往下阅读。

保护Redis
===

默认情况下，Redis绑定到 **所有网络接口** 并且根本没有
身份验证。如果您在一个非常受控制的环境中使用Redis，与
外部互联网分开，并且通常与攻击者分开，那很好。但是如果
没有任何加固的Redis暴露在互联网上，那就是一个很大的安全
问题。如果您不能100%确定您的环境是否得到正确保护，请
检查以下步骤以提高Redis的安全性，这些步骤是
按照提高安全性的顺序排列的。

1. 确保Redis用于侦听连接的端口（默认为6379，如果您在集群模式下运行Redis，则为16379，加上Sentinel为26379）已设置防火墙，以便无法从外部联系上Redis。
2. 使用设置了 `bind` 指令的配置文件，以确保Redis仅侦听您正在使用的网络接口。例如，如果您仅从同一台计算机本地访问Redis，则仅使用环回接口 (127.0.0.1)，依此类推。
3. 使用 `requirepass` 选项以添加额外的安全层，以便客户端需要使用 `AUTH` 命令进行身份验证。
4. 如果您的环境需要加密，请使用 [spiped](http://www.tarsnap.com/spiped.html) 或其他SSL隧道软件来加密Redis服务器和Redis客户端之间的流量。

请注意，在没有任何安全性的情况下暴露在互联网上的Redis [很容易被利用](http://antirez.com/news/96)，因此请确保您了解上述内容并 **至少** 应用防火墙层。防火墙就位后，尝试从外部主机使用 `redis-cli` 连接，以证明您自己的实例在事实上无法访问。

在您的应用程序中使用Redis
===

当然，仅从命令行接口使用Redis是不够的，因为
目标是在您的应用程序中使用它。为此，您需要
为您使用的编程语言下载并安装Redis客户端库。
您可以 [在此页面中找到不同语言的完整客户端列表](https://redis.io/clients)。

例如，如果您碰巧使用Ruby编程语言，我们最好的建议
是使用 [Redis-rb](https://github.com/redis/redis-rb) 客户端。
您可以使用命令 **gem install redis** 来安装它。

下面的指南是特定于Ruby的，但实际上许多
流行语言的客户端库看起来非常相似：您创建一个Redis对象并执行
发送命令的方法。一个使用Ruby的简短交互式示例：

    >> require 'rubygems'
    => false
    >> require 'redis'
    => true
    >> r = Redis.new
    => #<Redis client v2.2.1 connected to redis://127.0.0.1:6379/0 (Redis v2.3.8)>
    >> r.ping
    => "PONG"
    >> r.set('foo','bar')
    => "OK"
    >> r.get('foo')
    => "bar"

Redis持久化
=================

您可以 [在此页面上了解Redis持久化的工作原理](/topics/persistence.md)，但是对于快速入门而言，重要的是要了解默认情况下，如果您使用默认配置启动Redis，Redis只会不时自发地保存数据集（例如如果您的数据至少有100次更改，则至少间隔五分钟），因此如果您希望数据库在重启后持久保存并重新加载，请确保每次要强制执行数据集快照时手动调用 **SAVE** 命令。否则请确保使用 **SHUTDOWN** 命令关闭数据库：

    $ redis-cli shutdown

这样Redis将确保在退出之前将数据保存在磁盘上。
强烈建议阅读 [持久性页面](/topics/persistence.md)，以便更好地了解Redis持久性的工作原理。

更正确地安装Redis
==============================

如果只是为了体验Redis或用于开发，从命令行运行它
是可以的。但是，在某些时刻，您将有一些实际的应用程序
在真实服务器上运行。对于这种用途，您有两种不同的选择：

* 使用screen运行Redis。
* 使用init脚本以正确的方式在您的Linux机器中安装Redis，以便在系统重启后一切将重新正常地启动。

强烈建议使用init脚本来正确地安装。
以下说明可用于在基于Debian或Ubuntu的发行版中使用Redis 2.4附带的init脚本执行正确安装。

我们假设您已经在/usr/local/bin下复制了 **redis-server** 和 **redis-cli** 可执行文件。

* 创建一个目录来存储您的Redis配置文件和您的数据：

        sudo mkdir /etc/redis
        sudo mkdir /var/redis

* 将您在Redis发行版中的 **utils** 目录下找到的init脚本复制到 /etc/init.d 中。我们建议使用运行此Redis实例的端口号来命名它。例如：

        sudo cp utils/redis_init_script /etc/init.d/redis_6379

* 编辑init脚本。

        sudo vi /etc/init.d/redis_6379

确保根据您使用的端口修改 **REDISPORT**。
pid文件路径和配置文件名都取决于端口号。

* 使用端口号作为名称，将您将在Redis发行版的根目录中找到的模板配置文件复制到/etc/redis/中，例如：

        sudo cp redis.conf /etc/redis/6379.conf

* 在/var/redis中创建一个目录，作为此Redis实例的数据和工作目录：

        sudo mkdir /var/redis/6379

* 编辑配置文件，确保执行以下更改：
    * 将 **daemonize** 设置为 yes (默认设置为 no).
    * 将 **pidfile** 设置为 `/var/run/redis_6379.pid`（如果需要，修改端口）。
    * 相应地更改 **port**。在我们的示例中不需要改它，因为默认端口已经是6379。
    * 设置您希望的 **loglevel**。
    * 将 **logfile** 设置为 `/var/log/redis_6379.log`
    * 将 **dir** 设置为 `/var/redis/6379`（非常重要的一步！）
* 最后，使用以下命令将新的Redis init脚本添加到所有默认运行级别：

        sudo update-rc.d redis_6379 defaults

大功告成！现在您可以尝试使用以下命令运行您的实例：

    sudo /etc/init.d/redis_6379 start

确保一切都按预期工作：

* 尝试使用redis-cli ping您的实例。
* 使用 **redis-cli save** 进行测试保存并检查转储文件是否正确存储到了/var/redis/6379/（您应该找到一个名为dump.rdb的文件）。
* 检查您的Redis实例是否正确记录日志到日志文件中。
* 如果它是一台可以用来试验的新机器，请确保在重新启动后一切仍然正常。

注意：在上面的说明中，我们跳过了许多您可能想要更改的Redis配置参数，例如为了使用AOF持久性而不是RDB持久性，或设置复制等。
请务必阅读示例 `redis.conf` 文件（有大量注释）以及您可以在此网站中找到的其他文档以获取更多信息。
