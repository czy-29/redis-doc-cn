社区
===

要获得帮助或提供任何反馈，主要渠道是Redis邮件列表：

* 加入[邮件列表](http://groups.google.com/group/redis-db)（通过[电子邮件](mailto:redis-db+subscribe@googlegroups.com)订阅）。

对于bug报告，请使用[Github](https://github.com/redis/redis)。

其他可以找到对Redis感兴趣的人的地方：

* 在[Redis Discord服务器](https://discord.gg/redis)上结识对Redis感兴趣的人。
* [Stack Overflow上的Redis标签](http://stackoverflow.com/questions/tagged/redis?sort=newest&pageSize=30)。
* 在Twitter上关注[Redis新闻提要](http://twitter.com/redisfeed)。
* Redis社区使用Reddit sub来发布新闻和某些公告（也总是转到ML上）：[/r/redis sub on Reddit](https://www.reddit.com/r/redis/)。

项目治理
---

Redis采用了一种旨在成为精英管理的轻型治理模型，旨在赋予表现出长期承诺并做出重大贡献的个人权力。

有关更多信息，请参阅[治理页面](/topics/governance.md)。

Conferences and meetups
---

* [Redis Live](https://meetups.redislabs.com/redis-live/)
* [RedisConf Annual Conference](https://redislabs.com/redisconf)
* [Redis Day Seattle](https://connect.redislabs.com/redisdayseattle/mktg)
* [Redis Day Bangalore](https://connect.redislabs.com/redisdaybangalore)
* [London Redis Meetup Group](https://www.meetup.com/Redis-London)
* [San Francisco Meetup Group](http://sfmeetup.redis.io)
* [New York Meetup Group](https://www.meetup.com/New-York-REDIS-Meetup)
* [#RedisTLV (Tel Aviv Redis) Meetup Group](https://www.meetup.com/Tel-Aviv-Redis-Meetup)
* [Paris Redis Meetup](https://www.meetup.com/Paris-Redis-Meetup/)

为Redis做贡献
---

您想为Redis贡献一个功能吗？

1. 将您的提案发送到[邮件列表](http://groups.google.com/group/redis-db)。确保您解释了用例是什么以及API会是怎样的。

2. 如果您得到良好的反馈，请执行以下操作以提交补丁：

    1. Fork[官方Github仓库](http://github.com/redis/redis)。
    2. Clone您的fork：`git clone git@github.com:<your-username>/redis.git`
    3. 确保您能通过所有的测试：`make && make test`
    4. 创建一个主题分支：`git checkout -b new-feature`
    5. 添加测试并实现您的更改。
    6. 完成后，确保所有测试仍然可以通过：`make && make test`
    7. 提交并push到您的fork上。
    8. [创建一个带有您补丁链接的issue](https://github.com/redis/redis/issues)。
    9. 坐下来享受吧。

还有其他方法可以提供帮助：

* [修复bug或分享您在问题上的经验](https://github.com/redis/redis/issues)

* 改进[文档](http://github.com/redis/redis-doc)

* 帮助维护或创建新的[客户端库](https://redis.io/clients)

* 改进[这个网站](http://github.com/redis/redis-io)
