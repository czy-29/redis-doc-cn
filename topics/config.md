Redis配置
===

Redis可以使用内置的默认配置在没有配置文件的情况下
启动，但是此设置仅建议用于测试和
开发目的。

配置Redis的正确方法是提供一个Redis配置文件，
通常称为 `redis.conf`。

`redis.conf` 文件包含许多具有非常简单格式的
指令：

    keyword argument1 argument2 ... argumentN

这是配置指令的示例：

    slaveof 127.0.0.1 6380

可以使用（双或单）引号提供包含
空格的字符串作为参数，如下例所示：

    requirepass "hello world"

单引号字符串可以包含由反斜杠转义的字符，
双引号字符串还可以包含使用反斜杠十六进制符号 "\\xff"
编码的任何ASCII符号。

配置指令列表及其含义和预期用途
可在随附到Redis发行版中的self-documented（译者注：这词不知道怎么翻译了）
示例 redis.conf 中找到。

* The self documented [redis.conf for Redis 6.0](https://raw.githubusercontent.com/redis/redis/6.0/redis.conf).
* The self documented [redis.conf for Redis 5.0](https://raw.githubusercontent.com/redis/redis/5.0/redis.conf).
* The self documented [redis.conf for Redis 4.0](https://raw.githubusercontent.com/redis/redis/4.0/redis.conf).
* The self documented [redis.conf for Redis 3.2](https://raw.githubusercontent.com/redis/redis/3.2/redis.conf).
* The self documented [redis.conf for Redis 3.0](https://raw.githubusercontent.com/redis/redis/3.0/redis.conf).
* The self documented [redis.conf for Redis 2.8](https://raw.githubusercontent.com/redis/redis/2.8/redis.conf).
* The self documented [redis.conf for Redis 2.6](https://raw.githubusercontent.com/redis/redis/2.6/redis.conf).
* The self documented [redis.conf for Redis 2.4](https://raw.githubusercontent.com/redis/redis/2.4/redis.conf).

通过命令行传递参数
---

从Redis 2.6开始，也可以直接使用命令行
传递Redis配置参数。这对于测试目的非常有用。
下面是一个示例，它使用端口6380作为运行在 127.0.0.1
端口 6379 的实例的从属启动一个新的Redis实例。

    ./redis-server --port 6380 --slaveof 127.0.0.1 6379

通过命令行传递的参数格式与 redis.conf 文件中使用
的格式完全相同，只是关键字
以 `--` 为前缀。

请注意，这会在内部生成一个内存中的临时配置文件
（如果有的话，可能会连接用户传递的配置文件），其中的
参数被转换为 redis.conf 的格式。

在服务器运行时更改Redis配置
---

可以在不停止和重新启动服务的情况下动态
重新配置Redis，或者使用特殊命令
[CONFIG SET](/commands/config-set.md) 和
[CONFIG GET](/commands/config-get.md) 以编程方式查询当前配置。

并非所有配置指令都以这种方式被支持，但大多数
都按预期支持。有关更多信息，
请参阅 [CONFIG SET](/commands/config-set.md) 和 [CONFIG GET](/commands/config-get.md)
页面。

请注意，动态修改配置 **对redis.conf文件
没有影响**，因此在下次重新启动Redis时，将使用
旧配置。

确保还根据您使用 [CONFIG SET](/commands/config-set.md) 设置的配置
相应地修改了 `redis.conf` 文件。你可以手动完成，或者从Redis 2.8开始，你可以只使用 [CONFIG REWRITE](/commands/config-rewrite.md)，它会自动扫描你的 `redis.conf` 文件并更新与当前配置值不匹配的字段。不存在但设置为默认值的字段不会被添加。配置文件中的注释会被保留。

将Redis配置为缓存
---

如果您打算将Redis用作缓存，其中每个键都将设置
过期时间，则可以考虑使用以下配置
（作为示例，假设最大内存限制为2MB）：

    maxmemory 2mb
    maxmemory-policy allkeys-lru

在此配置中，应用程序无需使用
`EXPIRE` 命令（或等效命令）设置键的生存时间，因为
只要我们达到2MB的内存限制，就会对所有键
使用近似LRU的算法执行淘汰。

基本上在这种配置中，Redis的行为方式与memcached类似。
我们有更多关于 [使用Redis作为LRU缓存](/topics/lru-cache.md) 的文档。
