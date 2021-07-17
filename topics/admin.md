Redis管理
===

此页面包含与Redis实例管理相关的主题。
每个主题都以FAQ的形式自包含。将来会创建新的主题。

Redis设置提示
-----------------

+ 我们建议使用 **Linux操作系统** 部署Redis。Redis也在OS X上进行了大量测试，并不时在FreeBSD和OpenBSD系统上进行测试。然而，Linux是我们进行所有主要压力测试的地方，也是大多数生产部署运行的地方。
+ 确保将Linux内核 **overcommit memory设置为1**。将 `vm.overcommit_memory = 1` 添加到 `/etc/sysctl.conf` 然后重新启动或运行命令 `sysctl vm.overcommit_memory=1` 使其立即生效。
+ 确保Redis不会受到Linux内核 *透明大页面* 特性的影响，否则会对内存使用和延迟产生负面影响。这是通过以下命令完成的：`echo madvise > /sys/kernel/mm/transparent_hugepage/enabled`。
+ 确保在您的系统中 **设置一些交换**（我们建议与内存一样多）。如果Linux没有swap并且你的Redis实例不小心消耗了太多内存，要么Redis在内存不足时崩溃，要么Linux内核OOM杀手会杀死Redis进程。启用交换后，Redis的工作方式会很糟糕，但您可能会注意到延迟高峰，并在为时已晚之前采取行动。
+ 在您的实例中设置一个显式的 `maxmemory` 选项限制，以确保当接近达到系统内存限制时，实例将报告错误而不是崩溃。请注意，`maxmemory` 的设置应计算Redis除数据之外的开销以及碎片开销。因此，如果您认为您有10GB的可用内存，请将其设置为8或9+。如果你在一个非常write-heavy的应用程序中使用Redis，在磁盘上保存一个RDB文件或重写AOF日志时 **Redis可能会使用通常使用的内存的2倍**。使用的额外内存与保存过程中写入修改的内存页数成正比，因此通常与这段时间内接触的键（或聚合类型项）的数量成正比。确保相应地调整内存大小。
+ 在daemontools下运行时使用 `daemonize no`。
+ 确保设置一些不小的replication backlog，必须与Redis使用的内存量成比例地设置。在20GB的实例中，只有1MB的backlog是没有意义的。backlog将允许副本轻松地与主实例重新同步。
+ 即使您禁用了持久性，如果您使用replication，Redis也需要执行RDB保存，除非您使用新的无盘复制功能。如果master上没有磁盘空间可以使用，请确保启用无盘复制。
+ 如果您使用replication，请确保您的master启用了持久性，或者它不会在崩溃时自动重新启动：副本将尝试成为master的精确拷贝，因此如果master以空数据集重新启动，副本的数据也会被抹掉。
+ 默认情况下，Redis不需要 **任何身份验证并侦听所有网络接口**。如果您将Redis暴露在互联网或攻击者可以访问的其他地方，这是一个很大的安全问题。例如，请参阅 [此攻击](http://antirez.com/news/96) 以了解其危险程度。请查看我们的 [安全页面](/topics/security.md) 和 [快速入门](/topics/quickstart.md)，了解有关如何保护Redis的信息。
+ `LATENCY DOCTOR` 和 `MEMORY DOCTOR` 是您的朋友。

在EC2上运行Redis
--------------------

+ 使用基于HVM的实例，而不是基于PV的实例。
+ 不要使用旧实例系列，例如：将m3.medium与HVM一起使用，而不要将m1.medium与PV一起使用。
+ 将Redis持久性与 **EC2 EBS卷** 一起使用需要小心处理，因为有时EBS卷具有高延迟特性。
+ 如果在副本与主服务器同步时遇到问题，您可以尝试新的 **无盘复制**。

无需停机即可升级或重启Redis实例
-------------------------------------------------------

Redis被设计为可以在您的服务器中运行很长时间的进程。
例如，可以使用 [CONFIG SET命令](/commands/config-set.md) 修改许多配置选项，而无需任何类型的重新启动。

从Redis 2.2开始，甚至可以在不重新启动Redis的情况下从AOF切换到RDB快照持久性或者反过来。查看 `CONFIG GET *` 命令的输出以获取更多信息。

但是有时需要重新启动，例如为了将Redis进程升级到更新的版本，或者当您需要修改一些当前不受 CONFIG 命令支持的配置参数时。

以下步骤提供了一种非常常用的方法，以避免任何停机时间。

* 将新Redis实例设置为当前Redis实例的slave。为此，您需要一台不同的服务器，或者一台具有足够RAM以保持两个Redis实例同时运行的服务器。
* 如果使用单台服务器，请确保slave在与master实例不同的端口上启动，否则slave将根本无法启动。
* 等待复制初始同步完成（检查slave的日志文件）。
* 使用 INFO 确保master和slave中的键数相同。使用 redis-cli 检查slave是否按您的意愿工作并正在回复您的命令。
* 使用 **CONFIG SET slave-read-only no** 以允许slave执行写操作。
* 配置所有客户端以使用新实例（即slave）。请注意，您可能需要使用 `CLIENT PAUSE` 命令以确保在切换期间没有客户端可以写入旧master服务器。
* 一旦您确定master不再接收任何查询（您可以使用 [MONITOR命令](/commands/monitor.md) 检查这一点），请使用 **SLAVEOF NO ONE** 命令将slave选举为master，然后关闭您的master。

如果您使用的是 [Redis Sentinel](/topics/sentine.md) 或 [Redis Cluster](/topics/cluster-tutorial.md)，升级到新版本的最简单方法是依次升级每一个slave，然后执行手动故障转移以将升级后的副本之一提升为master，并最终提升最后一个slave。

但请注意，Redis Cluster 4.0在集群总线协议级别与Redis Cluster 3.2不兼容，因此在这种情况下需要大规模重启。但是Redis 5集群总线向后兼容Redis 4。
