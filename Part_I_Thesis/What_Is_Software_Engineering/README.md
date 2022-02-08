# 1. 什么是软件工程？

> 没有什么是建在石头上的；因为一切都是建立在沙土之上，但是，我们必须视沙土为石头
>
> —— 豪尔赫·路易斯·博尔赫斯

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Nothing is built on stone; all is built on sand, but we must build as if the sand were stone.
—Jorge Luis Borges
</div></details>

我们看到编程和软件工程之间的三个关键区别：时间、规模和权衡。在软件工程项目中，工程师需要更加关注时间的流逝和最终的需求变更。在软件工程组织中，我们需要更加关注规模和效率，无论是我们生产的软件还是生产它的组织。最后，作为软件工程师，我们常常被要求基于对时间和增长的不精确估计来做出具有更高风险结果的复杂决策。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
We see three critical differences between programming and software engineering: time, scale, and the trade-offs at play. On a software engineering project, engineers need to be more concerned with the passage of time and the eventual need for change. In a software engineering organization, we need to be more concerned about scale and efficiency, both for the software we produce as well as for the organization that is producing it. Finally, as software engineers, we are asked to make more complex decisions with higher-stakes outcomes, often based on imprecise estimates of time and growth.
</div></details>

在谷歌内部，我们有时会说，“软件工程是编程加时间。” 编程当然是软件工程的重要组成部分：毕竟，编程才是生成新软件的首要方式。如果你意识到了这种区别，那么很明显我们可能需要把编程任务（开发）和软件工程任务（开发、修改、维护）划分开。时间的增加为编程增加了一个新的重要的维度。立方体不是正方形，距离不是速度。软件工程不是编程。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Within Google, we sometimes say, “Software engineering is programming integrated over time.” Programming is certainly a significant part of software engineering: after all, programming is how you generate new software in the first place. If you accept this distinction, it also becomes clear that we might need to delineate between programming tasks (development) and software engineering tasks (development, modification, maintenance). The addition of time adds an important new dimension to programming. Cubes aren’t squares, distance isn’t velocity. Software engineering isn’t programming.
</div></details>

探索时间对程序的影响，一种方法是考虑如下问题：“代码的预期生命周期[^1](<mark style="color:orange;">我们的意思不是“执行生命周期”，而是“维护生命周期”——代码将继续构建、执行和维护多长时间？ 该软件将提供多长时间的价值？</mark>)是多少？”这个问题的合理答案相差大约 100,000 倍。仅需要持续几分钟的代码与想象可以存活几十年的代码一样合理。<mark style="color:red;">通常，短生命周期的代码不受时间的影响</mark>。对于一个使用仅一个小时的程序，你不太可能去适应新版本的底层库、操作系统 (OS)、硬件或语言版本。这些短暂的系统实际上“只是”一个编程问题，就像立方体压缩到一个面上就是正方形一样。随着我们不断延长生命周期，变更变得更加重要。在十年或更长的时间里，程序大多数的依赖，无论是隐式的还是显式的，都可能发生变化。这种认识是我们区分软件工程和编程的根源。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
One way to see the impact of time on a program is to think about the question, “What is the expected life span1 of your code?” Reasonable answers to this question vary by roughly a factor of 100,000. It is just as reasonable to think of code that needs to last for a few minutes as it is to imagine code that will live for decades. Generally, code on the short end of that spectrum is unaffected by time. It is unlikely that you need to adapt to a new version of your underlying libraries, operating system (OS), hardware, or language version for a program whose utility spans only an hour. These short-lived systems are effectively “just” a programming problem, in the same way that a cube compressed far enough in one dimension is a square. As we expand that time to allow for longer life spans, change becomes more important. Over a span of a decade or more, most program dependencies, whether implicit or explicit, will likely change. This recognition is at the root of our distinction between software engineer‐ ing and programming.
</div></details>

这种区别就是我们所说的软件<i>可持续性<i>的核心。如果在软件的预期生命周期内，无论是出于技术原因还是商业原因，您都能够对出现的任何有价值的变化做出反应，那么您的项目就是可持续的。重要的是，我们只是在寻找能力——您可能会选择不执行既定的升级，无论是因为缺乏价值还是其他优先事项[^2]（<mark style="color:orange;">这可能是对技术债务的一个合理的手动定义：“应该”完成但尚未完成的事情——我们的代码与我们希望的事情之间的差异。</mark>）。当您无法对基础技术或产品方向的变化做出应对时，您就会铤而走险寄希望于这种变化永远不会变得至关重要。对于短期项目而言，这可能是一个安全的赌注。超过几十年的话，它可能就不是了[^3]（<mark style="color:orange;">还要考虑一个问题，我们是否能提前知道项目将是长期存在的。</mark>）。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
This distinction is at the core of what we call sustainability for software. Your project is sustainable if, for the expected life span of your software, you are capable of react‐ ing to whatever valuable change comes along, for either technical or business reasons. Importantly, we are looking only for capability—you might choose not to perform a given upgrade, either for lack of value or other priorities. When you are fundamentally incapable of reacting to a change in underlying technology or product direction, you’re placing a high-risk bet on the hope that such a change never becomes critical. For short-term projects, that might be a safe bet. Over multiple decades, it probably isn’t.
</div></details>

另一种看待软件工程的方式是规模。有多少人参与？随着时间的推移，他们在开发和维护中将扮演什么样的角色？编程任务通常是个人的创造行为，但软件工程任务则是团队合作的结果。早期定义软件工程的尝试给出了这个观点一个很好的解释：“多版本程序的多人开发。”[^4]（<mark style="color:orange;">关于这句话的原出处有一些疑问；共识似乎是它最初是由布赖恩兰德尔或玛格丽特汉密尔顿提出的，但很可能由戴夫帕纳斯所创立。对它的常见引用是“软件工程技术：北约科学委员会主办的会议报告”，意大利罗马，1969 年 10 月 27-31 日，布鲁塞尔，北约科学事务部。</mark>） 这表明软件工程和编程之间的区别是时间和人的区别。团队协作带来了新问题，但相比单个程序员它提供了更多的潜力来生产有价值的系统。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Another way to look at software engineering is to consider scale. How many people are involved? What part do they play in the development and maintenance over time? A programming task is often an act of individual creation, but a software engineering task is a team effort. An early attempt to define software engineering produced a good definition for this viewpoint: “The multiperson development of multiversion programs.”4 This suggests the difference between software engineering and program‐ ming is one of both time and people. Team collaboration presents new problems, but also provides more potential to produce valuable systems than any single programmer could.
</div></details>

团队组织、项目组成以及软件项目的策略和实践都决定了软件工程的复杂性。这些问题是规模所固有的：随着组织的发展和项目的扩张，生产软件是否变得更有效率？随着我们的成长，我们的开发工作流程是否变得更有效率，或者我们的版本控制策略和测试策略是否会同比例增加成本？从软件工程的早期开始，就一直在讨论围绕通信和人类扩张的规模问题，一直追溯到《人月神话》[^5](<mark style="color:orange;">Frederick P. Brooks Jr. The Mythical Man-Month: Essays on Software Engineering (Boston: Addison-Wesley, 1995).</mark>)。此类规模问题通常是政策问题，并且是软件可持续性问题的基础：做我们需要重复做的事情要花多少钱？

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Team organization, project composition, and the policies and practices of a software project all dominate this aspect of software engineering complexity. These problems are inherent to scale: as the organization grows and its projects expand, does it become more efficient at producing software? Does our development workflow become more efficient as we grow, or do our version control policies and testing strategies cost us proportionally more? Scale issues around communication and human scaling have been discussed since the early days of software engineering, going all the way back to the Mythical Man Month. Such scale issues are often matters of policy and are fundamental to the question of software sustainability: how much will it cost to do the things that we need to do repeatedly?
</div></details>

我们也可以说，软件工程在需要做出决策的复杂性及其风险方面不同于编程。在软件工程中，我们经常被迫评估多条前进路径之间的权衡，有时风险很高，而且价值指标往往不完善。软件工程师或软件工程负责人的工作目标是实现组织、产品和开发工作流程的扩张成本的可持续性和管理。考虑到这些，然后权衡并做出合理决策。我们有时可能会推迟维护变更，甚至采用不能很好扩展的策略，因为我们知道我们需要重新审视这些决策。这些选择应该明确并清楚地说明延期成本。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
We can also say that software engineering is different from programming in terms of the complexity of decisions that need to be made and their stakes. In software engineering, we are regularly forced to evaluate the trade-offs between several paths for‐ ward, sometimes with high stakes and often with imperfect value metrics. The job of a software engineer, or a software engineering leader, is to aim for sustainability and management of the scaling costs for the organization, the product, and the development workflow. With those inputs in mind, evaluate your trade-offs and make rational decisions. We might sometimes defer maintenance changes, or even embrace policies that don’t scale well, with the knowledge that we’ll need to revisit those decisions. Those choices should be explicit and clear about the deferred costs.
</div></details>

在软件工程中很少有一刀切的解决方案，这同样适用于本书。假设“这个软件能存活多久”的合理答案的系数为 100,000，“您的组织中有多少工程师”的范围可能为 10,000，而“有多少计算资源可用于您的项目，”Google 的经验可能与您不符。在本书中，我们的目标是展示我们在构建和维护持续长达数十年、拥有数万名工程师和全球计算资源的软件方面的发现。我们认为在这种规模下大多数必要的实践同样也适用于较小的努力：将其视为一份关于我们认为随着规模扩大可能会很好的工程生态系统的报告。在一些地方，超大规模有其自身的成本，我们更乐意不用付出额外的开销。我们称这些为警告。如果您的组织发展到足以担心这些成本，希望您可以找到更好的答案。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Rarely is there a one-size-fits-all solution in software engineering, and the same applies to this book. Given a factor of 100,000 for reasonable answers on “How long will this software live,” a range of perhaps a factor of 10,000 for “How many engineers are in your organization,” and who-knows-how-much for “How many compute resources are available for your project,” Google’s experience will probably not match yours. In this book, we aim to present what we’ve found that works for us in the construction and maintenance of software that we expect to last for decades, with tens of thousands of engineers, and world-spanning compute resources. Most of the practices that we find are necessary at that scale will also work well for smaller endeavors: consider this a report on one engineering ecosystem that we think could be good as you scale up. In a few places, super-large scale comes with its own costs, and we’d be happier to not be paying extra overhead. We call those out as a warning. Hopefully if your organization grows large enough to be worried about those costs, you can find a better answer.
</div></details>

在我们详细了解团队合作、文化、政策和工具的细节之前，让我们首先详细说明时间、规模和权衡等主要主题。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Before we get to specifics about teamwork, culture, policies, and tools, let’s first elaborate on these primary themes of time, scale, and trade-offs.
</div></details>
