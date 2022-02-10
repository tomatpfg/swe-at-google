# 权衡和成本

如果我们了解如何编程，了解我们正在维护的软件的生命周期，并了解如何在我们随着更多工程师生产和维护新功能而扩大规模时对其进行维护，那么剩下的就是做出正确的决策。这似乎很明显：在软件工程中，就像在生活中一样，好的选择会带来好的结果。然而，这种观察的后果很容易被忽视。在谷歌内部，人们强烈反对“因为我这么说”。重要的是有任何主题的决策者和明确的升级路径，当决策似乎错误时，但目标是达成共识，而不是一致。看到一些“我不同意你的指标/估值，但我知道你是如何得出这个结论的”的例子是好的，并且预计会出现。所有这一切的本质是任何事情都需要有一个理由。 “只是因为”、“因为我这么说”或“因为其他人都是这样做的”都是错误决定潜伏的地方。只要这样做是有效的，我们就应该能够在决定两个工程选项的一般成本时解释我们的工作。&#x20;

我们所说的成本是什么意思？我们不仅在这里谈论美元。 “成本”大致转化为努力，可能涉及以下任何或所有因素：&#x20;

* 财务成本（例如，金钱）
* 资源成本（例如 CPU 时间）
* 人员成本（例如，工程工作量）
* 交易成本（例如，采取行动的成本是多少？）
* 机会成本（例如，不采取行动的成本是多少？）&#x20;
* 社会成本（例如，这种选择对整个社会有什么影响？）

从历史上看，忽略社会成本问题特别容易。然而，谷歌和其他大型科技公司现在可以可靠地部署拥有数十亿用户的产品。在许多情况下，这些产品具有明显的净收益，但当我们以如此规模运营时，即使是可用性、可访问性、公平性或滥用可能性方面的微小差异也会被放大，通常会对已经被边缘化的群体造成损害.软件渗透到社会和文化的方方面面；因此，明智的做法是在做出产品和技术决策时了解我们所带来的好处和坏处。我们将在第 4 章对此进行更多讨论。

除了上述成本（或我们对它们的估计）之外，还有偏差：现状偏差、损失厌恶等。 当我们评估成本时，我们需要牢记前面列出的所有成本：一个组织的健康状况不仅在于银行里是否有钱，还在于其成员是否感到受到重视和富有成效。 在软件工程等极具创意和利润丰厚的领域，财务成本通常不是限制因素——人员成本通常是。 保持工程师的快乐、专注和参与所带来的效率收益很容易支配其他因素，因为专注度和生产力变化很大，很容易想象 10% 到 20% 的差异。

### 示例：标记

在许多组织中，白板标记被视为贵重物品。它们受到严格控制并且总是供不应求。任何给定白板上的一半标记总是干燥且无法使用。您参加因缺少工作标记而中断的会议的频率如何？您有多少次因为标记用尽而使您的思路脱轨？多久所有的标记都丢失了，大概是因为其他团队的标记用完了，不得不和你的一起潜逃？所有这些都是为了一种成本不到一美元的产品。&#x20;

谷歌倾向于在大多数工作区域打开装满办公用品的壁橱，包括白板标记。只需稍等片刻，即可轻松抓取多种颜色的标记。沿着这条线的某个地方，我们做了一个明确的权衡：优化无障碍头脑风暴比防止有人带着一堆标记徘徊更重要。&#x20;

我们的目标是对我们所做的一切所涉及的成本/收益权衡，从办公用品和员工津贴到开发人员的日常经验，到如何在全球范围内提供和运行- 规模服务。我们常说，“谷歌是一种数据驱动的文化。”事实上，这是一种简化：即使没有数据，也可能有证据、先例和论据。做出好的工程决策就是权衡所有可用的输入，并就权衡做出明智的决定。有时，这些决定是基于直觉或公认的最佳实践，但前提是我们已经用尽了试图衡量或估计真正潜在成本的方法。&#x20;

最后，工程组的决策应该归结为很少的事情：&#x20;

* 我们这样做是因为我们必须（法律要求、客户要求）。
* 我们这样做是因为根据当前证据，这是我们当时可以看到的最佳选择（由某些适当的决策者确定）。&#x20;

决策不应该是“我们这样做是因为我是这么说的。”

### 决策的输入

当我们在权衡数据时，我们发现了两种常见的场景：

所有涉及的数量都是可测量的或至少可以估计的。这通常意味着我们正在评估 CPU 和网络之间的权衡，或者美元和 RAM 之间的权衡，或者考虑是否要花费两周的工程师时间来节省我们数据中心的 N 个 CPU。&#x20;

有些量是微妙的，或者我们不知道如何测量它们。有时这表现为“我们不知道这需要多少工程师时间。”有时甚至更模糊：您如何衡量设计不佳的 API 的工程成本？还是产品选择的社会影响？&#x20;

没有理由在第一种类型的决定上有缺陷。任何软件工程组织都可以而且应该跟踪计算资源的当前成本、工程工时和您经常接触的其他数量。即使您不想向您的组织公布确切的美元金额，您仍然可以生成一个转换表：这么多 CPU 的成本与这么多 RAM 或这么多网络带宽的成本相同。&#x20;

有了商定的转换表，每个工程师都可以进行自己的分析。 “如果我花两周时间将这个链表更改为更高性能的结构，我将多使用 5 GB 的生产 RAM，但会节省 2000 个 CPU。我应该做吗？”这个问题不仅取决于 RAM 和 CPU 的相对成本，还取决于人员成本（为软件工程师提供两周的支持）和机会成本（该工程师在两周内还能生产什么？）。 对于第二种类型的决定，没有简单的答案。我们依靠经验、领导力和先例来协商这些问题。我们正在投资于研究以帮助我们量化难以量化的事物（参见第 7 章）。然而，我们所拥有的最好的广泛建议是要意识到并非所有事情都是可衡量或可预测的，并尝试以相同的优先级和更加谨慎的态度对待此类决定。它们通常同样重要，但更难管理。

### 示例：分布式构建

考虑您的构建。 根据完全不科学的 Twitter 民意调查，大约 60% 到 70% 的开发人员在本地构建，即使是今天大型、复杂的构建。 正如本“编译”漫画所示，这直接导致了非玩笑——您的组织在等待构建过程中浪费了多少生产时间？ 将其与为小团体运行类似 distcc 的成本进行比较。 或者，为一大群人经营一个小型建造农场需要多少钱？ 这些成本需要多少周/月才能实现净赢？

早在 2000 年代中期，Google 完全依赖于本地构建系统：您检查代码并在本地编译。 在某些情况下，我们拥有大量本地机器（您可以在桌面上构建地图！），但是随着代码库的增长，编译时间变得越来越长。 不出所料，由于时间损失，以及更大、更强大的本地机器的资源成本增加等，我们招致了不断增加的人员成本开销。 这些资源成本特别麻烦：我们当然希望人们拥有尽可能快的构建，但大多数时候，高性能桌面开发机器会闲置。 感觉这不是投资这些资源的正确方式。

最终，谷歌开发了自己的分布式构建系统。 这个系统的开发当然是有成本的：工程师需要花费时间来开发，改变每个人的习惯和工作流程以及学习新系统需要更多的工程师时间，当然也需要额外的计算资源。 但总体上的节省显然是值得的：构建速度更快，工程师时间得到补偿，硬件投资可以专注于托管共享基础设施（实际上，是我们生产团队的一个子集）而不是更强大的台式机。 第 18 章详细介绍了我们的分布式构建方法和相关权衡。

因此，我们构建了一个新系统，将其部署到生产环境中，并加快了每个人的构建速度。这就是故事的美好结局吗？不完全是：提供分布式构建系统极大地提高了工程师的生产力，但随着时间的推移，分布式构建本身变得臃肿。在之前的案例中，个别工程师受到的限制（因为他们有既得利益，尽可能快地保持本地构建）在分布式构建系统中不受限制。构建图中臃肿或不必要的依赖关系变得太普遍了。当每个人都直接感受到非最佳构建的痛苦并被激励保持警惕时，激励措施会更好地协调一致。通过在并行分布式构建中消除这些激励并隐藏臃肿的依赖关系，我们创造了一种情况，在这种情况下，消耗可能会猖獗，并且几乎没有人受到激励去关注构建膨胀。这让人想起杰文斯悖论：资源的消耗可能会随着资源使用效率的提高而增加。&#x20;

总的来说，与添加分布式构建系统相关的节省成本远远超过与其构建和维护相关的负成本。但是，正如我们看到的消费增加，我们并没有预见到所有这些成本。一路领先，我们发现自己处于一种情况，需要重新定义系统的目标和约束以及我们的使用，确定最佳实践（小依赖，依赖的机器管理），并为工具和维护提供资金新的生态系统。即使是“我们将花费 \$$$ 用于计算资源以收回工程师时间”这种形式的相对简单的权衡，也会产生无法预见的下游影响。

### 示例：在时间和规模之间做出决定

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Much of the time, our major themes of time and scale overlap and work in conjunction. A policy like the Beyoncé Rule scales well and helps us maintain things over time. A change to an OS interface might require many small refactorings to adapt to,but most of those changes will scale well because they are of a similar form: the OS change doesn’t manifest differently for every caller and every project.
</div></details>

很多时候，我们的时间和规模的主要主题重叠并协同工作。像碧昂斯规则这样的政策可以很好地扩展并帮助我们随着时间的推移维护事物。对操作系统接口的更改可能需要进行许多小的重构才能适应，但大多数更改都可以很好地扩展，因为它们具有相似的形式：操作系统更改不会对每个调用者和每个项目都有不同的表现。&#x20;

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Occasionally time and scale come into conflict, and nowhere so clearly as in the basic question: should we add a dependency or fork/reimplement it to better suit our local needs?
</div></details>

有时时间和规模会发生冲突，没有比基本问题更清楚的地方了：我们应该添加依赖项还是分叉/重新实现它以更好地满足我们自身的需求？&#x20;

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
This question can arise at many levels of the software stack because it is regularly the case that a bespoke solution customized for your narrow problem space may outperform the general utility solution that needs to handle all possibilities. By forking or reimplementing utility code and customizing it for your narrow domain, you can add new features with greater ease, or optimize with greater certainty, regardless of whether we are talking about a microservice, an in-memory cache, a compression routine, or anything else in our software ecosystem. Perhaps more important, the control you gain from such a fork isolates you from changes in your underlying dependencies: those changes aren’t dictated by another team or third-party provider.You are in control of how and when to react to the passage of time and necessity to change.
</div></details>

这个问题可能出现在软件堆栈的许多级别，因为通常情况下，为小的空间问题定制的解决方案可能优于需要处理所有可能性的通用解决方案。通过分叉或重新实现实用程序代码并针对小的领域进行自定义，可以更轻松地添加新功能或更好地优化，无论我们是在谈论微服务、内存缓存、压缩程序还是我们软件生态系统中的其他任何东西。也许更重要的是，您从这种分叉中获得的控制权将您与底层依赖项的更改隔离开来：这些更改不是由另一个团队或第三方提供商决定的。您可以控制如何以及何时对时间的流逝和改变的必要性做出反应。&#x20;

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
On the other hand, if every developer forks everything used in their software project instead of reusing what exists, scalability suffers alongside sustainability. Reacting to a security issue in an underlying library is no longer a matter of updating a single dependency and its users: it is now a matter of identifying every vulnerable fork of that dependency and the users of those forks.
</div></details>

另一方面，如果每个开发人员都分叉项目中使用的所有内容而不是重用现有的内容，那么可扩展性就会受到可持续性的影响。对底层库中的安全问题做出反应不再是更新单个依赖项及其用户的问题：现在是识别该依赖项的每个易受攻击的分支以及这些分支用户的问题。&#x20;

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
As with most software engineering decisions, there isn’t a one-size-fits-all answer to this situation. If your project life span is short, forks are less risky. If the fork in question is provably limited in scope, that helps, as well—avoid forks for interfaces that
could operate across time or project-time boundaries (data structures, serialization formats, networking protocols). Consistency has great value, but generality comes with its own costs, and you can often win by doing your own thing—if you do it carefully.
</div></details>

与大多数软件工程决策一样，对于这种情况没有一刀切的答案。如果您的项目生命周期较短，那么分叉的风险较小。如果问题分支可以证明范围有限，那么这也有助于避免分叉可以跨时间或项目时间边界（数据结构、序列化格式、网络协议）操作的接口。一致性具有很大的价值，但通用性也有其自身的成本，如果您谨慎行事，您通常可以通过做自己的事情来获胜。

### 重新审视决策，犯错

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
One of the unsung benefits of committing to a data-driven culture is the combined ability and necessity of admitting to mistakes. A decision will be made at some point,based on the available data—hopefully based on good data and only a few assumptions, but implicitly based on currently available data. As new data comes in, contexts change, or assumptions are dispelled, it might become clear that a decision was in
error or that it made sense at the time but no longer does. This is particularly critical for a long-lived organization: time doesn’t only trigger changes in technical dependencies and software systems, but in data used to drive decisions.
</div></details>

致力于数据驱动文化的一项隐藏好处是承认错误的综合能力和必要性。将在某个时候根据可用数据做出决定——希望基于良好的数据和一些假设，但隐含地基于当前可用的数据。随着新数据的出现、上下文的变化或假设的消除，可能会清楚地表明某个决策是错误的，或者它在当时有意义但不再有意义。这对于一个长期存在的组织来说尤其重要：时间不仅会引发技术依赖和软件系统的变化，还会引发用于驱动决策的数据的变化。&#x20;

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
We believe strongly in data informing decisions, but we recognize that the data will change over time, and new data may present itself. This means, inherently, that decisions will need to be revisited from time to time over the life span of the system in question. For long-lived projects, it’s often critical to have the ability to change directions after an initial decision is made. And, importantly, it means that the deciders
need to have the right to admit mistakes. Contrary to some people’s instincts, leaders who admit mistakes are more respected, not less.
</div></details>

我们坚信数据为决策提供依据，但我们认识到数据会随着时间的推移而发生变化，并且可能会出现新的数据。这本质上意味着，需要在相关系统的整个生命周期内不时重新审视决策。对于长期项目，在做出初步决定后能够改变方向通常是至关重要的。而且，重要的是，这意味着决策者需要有权承认错误。与某些人的直觉相反，承认错误的领导者更受尊重，而不是更少。&#x20;

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Be evidence driven, but also realize that things that can’t be measured may still have value. If you’re a leader, that’s what you’ve been asked to do: exercise judgement,assert that things are important. We’ll speak more on leadership in Chapters 5 and 6.
</div></details>

以证据为导向，但也要意识到无法衡量的事物可能仍然有价值。如果您是领导者，这就是您被要求做的事情：运用判断力，断言事情很重要。我们将在第 <mark style="color:red">5</mark> 章和第 <mark style="color:red;">6</mark> 章中更多地讨论领导力。
