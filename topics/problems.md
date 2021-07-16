使用Redis遇到问题？这里是一个很好的起点。
===

如果您在使用Redis时遇到问题，本页面会尝试帮助您解决问题。Redis项目的一部分是帮助遇到问题的人，因为我们不喜欢让人们独自解决他们的问题。

* 如果您在使用Redis时遇到 **延迟问题** ，并且Redis似乎以某种方式空闲了一段时间，请阅读我们的 [Redis延迟故障排除指南](/topics/latency.md)。
* Redis稳定版本通常非常可靠，但是在您 **遇到崩溃** 的极少数情况下，如果您提供调试信息，开发人员可以提供更多帮助。请阅读我们的 [调试Redis指南](/topics/debugging.md).
* 我们有用户遇到Redis崩溃，实际上竟然是服务器 **RAM损坏** 的悠久历史。请使用 **redis-server --test-memory** 测试您的RAM，以防Redis在您的系统中不稳定。Redis内置的内存测试速度快且相当可靠，但如果可以，您应该重新启动服务器并使用 [memtest86](http://memtest86.com)。

对于所有其他问题，请向 [Redis Google Group](http://groups.google.com/group/redis-db) 发送消息。我们将很乐意提供帮助。

Redis 3.0.x、2.8.x 和 2.6.x 中已知的严重bug列表
===

要查找关键bug列表，请参阅更新日志：

* [Redis 3.0更新日志](https://raw.githubusercontent.com/redis/redis/3.0/00-RELEASENOTES).
* [Redis 2.8更新日志](https://raw.githubusercontent.com/redis/redis/2.8/00-RELEASENOTES).
* [Redis 2.6更新日志](https://raw.githubusercontent.com/redis/redis/2.6/00-RELEASENOTES).

检查每个补丁版本中的 *升级紧急* 程度，以便更轻松地发现
包含重要修复的版本。

影响Redis的已知Linux相关bug列表。
===

* Ubuntu 10.04 和 10.10 存在严重的bug（尤其是 10.10），如果不仅仅是实例挂起，则会导致速度变慢。请远离此发行版附带的默认内核。[10.04 bug的链接](https://blog.librato.com/posts/2011/5/16/ec2-users-should-be-cautious-when-booting-ubuntu-1004-amis)。[10.10 bug的链接](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/666211)。在EC2实例的上下文中多次报告了这两个bug，但其他用户确认本机服务器也受到影响（至少受到两者之一的影响）。
* 已知某些版本的 Xen 虚拟机管理器具有非常糟糕的 fork() 性能。有关更多信息，请参阅 [延迟页面](/topics/latency.md)。
