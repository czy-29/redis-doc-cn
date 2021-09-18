# Redis开源治理

## 介绍

自2009年以来，Redis开源项目变得非常成功并且非常受欢迎。

在此期间，Salvatore Sanfillipo领导、管理和维护了该项目。虽然来自Redis Ltd.和其他地方的贡献者做出了重大贡献，但该项目从未采用正式的治理结构，事实上，它是作为 [BDFL](https://en.wikipedia.org/wiki/Benevolent_dictator_for_life) 风格的项目运行的。

随着Redis的成长、成熟和其用户群的不断扩大，为Redis的持续开发和维护形成可持续的结构变得越来越重要。我们希望确保项目的连续性并投射出更大的社区。

## 新的治理结构，自2020年6月30日起适用

Redis采用了 _轻量级治理_ 模型，该模型与项目当前的规模相匹配，并最大限度地减少了对其早期模型的变更。治理模式旨在成为一种任人唯贤，旨在赋予表现出长期承诺并做出重大贡献的个人权力。

## Redis核心团队

Salvatore Sanfilippo已辞去该项目负责人的职务，并任命了两位继任者接替并领导Redis项目：Yossi Gottlieb（[yossigo](https://github.com/yossigo)）和 Oran Agra（[oranagra](https://github.com/oranagra)）

在Redis Ltd.的支持和加持下，我们希望借此机会，打造一个更加开放、可伸缩、社区驱动的“核心团队”架构来运行项目。核心团队将由这样选择出来的成员组成，他们是经过时间证明的长期个人参与和贡献者。

核心团队包括：

* 项目负责人：来自Redis Ltd.的 Yossi Gottlieb（[yossigo](https://github.com/yossigo)）
* 项目负责人：来自Redis Ltd.的 Oran Agra（[oranagra](https://github.com/oranagra)）
* 社区负责人：来自Redis Ltd.的 Itamar Haber（[itamarhaber](https://github.com/itamarhaber)）
* 成员：来自阿里巴巴的 Zhao Zhao（[soloestoy](https://github.com/soloestoy)）（译者注：不知道这位大佬的中文姓名，怕翻译错，有认识这位大佬的朋友可以补充下^_^）
* 成员：来自Amazon Web Services的 Madelyn Olson（[madolson](https://github.com/madolson)）

Redis核心团队成员服务于Redis开源项目和社区。他们应根据项目采用的 [行为准则](https://www.contributor-covenant.org/) 在行为、文化和语气方面树立良好榜样。他们还应该考虑项目和社区的最大利益并采取行动，不受外来利益或利益冲突的影响。

核心团队将负责Redis核心项目，这是Redis的一部分，托管在主Redis仓库中并使用BSD许可。它还旨在与构成Redis生态系统的其他项目保持协调和协作，包括Redis客户端、卫星项目、依赖Redis的主要中间件等。

#### 核心团队的角色和职责

* 管理核心Redis代码和文档
* 管理新的Redis版本
* 维护高层次的技术方向/路线图
* 提供快速响应，包括修复/补丁，以解决安全漏洞和其他主要问题
* 项目治理决策和变更
* Redis核心与Redis生态系统其他部分的协调
* 管理核心团队的成员

核心团队的目标是通过进一步将任务委派给表现出承诺、专业知识和技能的个人，来形成和授权一个贡献者社区。我们特别希望看到社区更多地参与以下领域：

* 对报告问题的支持、故障排除和bug修复
* 对贡献或pull requests的分类

#### 决策

* **常规决策** 将由核心团队成员基于lazy consensus方法做出：每个成员可以投票+1（同意）或-1（反对）。反对票必须包括彻底的推理并最好包含一个更好的替代提案。核心团队将始终尝试达成完全共识而不是少数服从多数。常规决策的例子：
    * pull requests和关闭issues的日常批准
    * 打开新issues供讨论
* **重大决策** 包含对Redis架构、设计或理念以及核心团队结构或成员变更有重大影响的决策，最好由完全共识决定。如果团队无法达成完全共识，则需要多数票。重大决策示例：
    *   Redis核心的根本性变化
    *   添加新的数据结构
    *   新版RESP（Redis序列化协议）
    *   影响向后兼容性的更改
    *   核心团队成员的添加或变更
* 项目负责人有权否决重大决策

#### 核心团队成员

* 核心团队预计不会终身服务，但需要长期参与以提供Redis编程风格和社区的稳定性和一致性。
* 如果必须更换由Redis Ltd.资助的核心团队成员，则由Redis Ltd.与其余核心团队成员协商后指定替代人员。
* 如果非Redis Ltd.资助的核心团队成员不再参与，无论出于何种原因，其他团队成员将选择替代者。

## 社区论坛和交流

我们希望Redis社区尽可能热情和包容。为此，我们采用了一项 [行为准则](https://www.contributor-covenant.org/)，我们要求所有社区成员阅读并遵守该准则。

我们鼓励所有重要的交流都是公开的、异步的、存档的，并开放给社区使用 [这里](/community.md) 描述的渠道积极参与。例外情况是需要在公开披露之前解决的敏感安全问题。

如需就敏感问题（例如不当行为或安全问题）联系核心团队，请发送电子邮件至 [redis@redis.io](mailto:redis@redis.io)。

## 新的Redis仓库和提交审批流程

Redis核心源代码库托管在 [https://github.com/redis/redis](https://github.com/redis/redis) 下。我们的目标是最终将一切（Redis核心源代码和其他生态系统项目）托管在Redis GitHub组织（[https://github.com/redis](https://github.com/redis)）下。向Redis源码库提交将需要代码审查、至少一名不是提交作者的核心团队成员的批准，并且没有异议。

## 项目和开发更新

与项目和社区保持联系！有关项目和社区的更新，请关注项目 [频道](/community.md)。开发公告将通过 [Redis邮件列表](https://groups.google.com/forum/#!forum/redis-db) 发布。

## 这些治理规则的更新

这些规则的任何实质性更改都将被视为重大决策。细微的变化或ministerial corrections（译者注：不知道怎么翻译了，如有好翻译欢迎pr）将被视为常规决策。
