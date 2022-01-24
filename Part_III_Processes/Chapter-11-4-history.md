## Google的测试历史 History of Testing at Google

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Now that we’ve discussed how Google approaches testing, it might be enlightening to learn how we got here. As mentioned previously, Google’s engineers didn’t always embrace the value of automated testing. In fact, until 2005, testing was closer to a curiosity than a disciplined practice. Most of the testing was done manually, if it was done at all. However, from 2005 to 2006, a testing revolution occurred and changed the way we approach software engineering. Its effects continue to reverberate within the company to this day.
</div></details>
既然我们已经讨论了Google如何处理测试，那么了解一下我们是如何做到这一点的可能会很有启发。如前所述，Google的工程师并不总是认同自动化测试的价值。事实上，在2005年之前，测试更接近于一种好奇心，而不是一种训练有素的实践。大多数测试都是手工完成的，如果有的话。然而，从2005年到2006年，测试革命发生了，并改变了我们处理软件工程的方式。时至今日，它的影响仍在公司内部回荡。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The experience of the GWS project, which we discussed at the opening of this chapter, acted as a catalyst. It made it clear how powerful automated testing could be. Following the improvements to GWS in 2005, the practices began spreading across the entire company. The tooling was primitive. However, the volunteers, who came to be known as the Testing Grouplet, didn’t let that slow them down.
</div></details>
我们在本章开头讨论的GWS项目的经验起到了催化剂的作用。它清楚地表明了自动化测试是多么强大。在2005年对GWS的改进之后，这种做法开始在整个公司蔓延。工具是原始的。然而，这些后来被称为“测试小组”的志愿者们并没有让这种情况阻碍他们。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Three key initiatives helped usher automated testing into the company’s consciousness: Orientation Classes, the Test Certified program, and Testing on the Toilet. Each one had influence in a completely different way, and together they reshaped Google’s engineering culture.
</div></details>
有三个关键举措帮助公司意识到自动化测试:培训课程、测试认证项目和厕所测试。每一个都以完全不同的方式产生影响，它们共同重塑了Google的工程文化。

### 入职培训方面 Orientation Classes

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Even though much of the early engineering staff at Google eschewed testing, the pioneers of automated testing at Google knew that at the rate the company was growing, new engineers would quickly outnumber existing team members. If they could reach all the new hires in the company, it could be an extremely effective avenue for introducing cultural change. Fortunately, there was, and still is, a single choke point that all new engineering hires pass through: orientation.
</div></details>
尽管Google的早期工程人员大多回避测试，但Google的自动化测试先驱们知道，以公司的增长速度，新工程师的数量很快就会超过现有的团队成员。如果他们能接触到公司所有的新员工，这可能是引入文化变革的一个非常有效的途径。幸运的是，所有新招的工程师都要经历一个瓶颈:入职培训。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Most of Google’s early orientation program concerned things like medical benefits and how Google Search worked, but starting in 2005 it also began including an hourlong discussion of the value of automated testing.The class covered the various benefits of testing, such as increased productivity, better documentation, and support for refactoring. It also covered how to write a good test. For many Nooglers (new Googlers) at the time, such a class was their first exposure to this material. Most important, all of these ideas were presented as though they were standard practice at the company. The new hires had no idea that they were being used as trojan horses to sneak this idea into their unsuspecting teams.
</div></details>

Google早期的培训项目大多关注医疗福利和Google搜索的工作方式，但从2005年开始，它也开始包括一个小时的讨论自动化测试的价值[^9]。这个类涵盖了测试的各种好处，例如提高生产力、更好的文档和对重构的支持。它还介绍了如何编写一个好的测试。对于当时的许多Google人(新员工)来说，这样的课程是他们第一次接触到这些材料。最重要的是，所有这些想法都被认为是公司的标准实践。这些新员工并不知道他们被当成了特洛伊木马，把这个想法偷偷带进了毫无戒心的团队。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
As Nooglers joined their teams following orientation, they began writing tests and questioning those on the team who didn’t. Within only a year or two, the population of engineers who had been taught testing outnumbered the pretesting culture engineers. As a result, many new projects started off on the right foot.
</div></details>
随着新人在接受培训后加入团队，他们开始编写测试，并质疑团队中没有这样做的人。仅在一两年内，接受过测试教育的工程师数量就超过了接受测试前的文化工程师。因此，许多新项目一开始就很顺利。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Testing has now become more widely practiced in the industry, so most new hires arrive with the expectations of automated testing firmly in place. Nonetheless, orientation classes continue to set expectations about testing and connect what Nooglers know about testing outside of Google to the challenges of doing so in our very large and very complex codebase.
</div></details>

测试现在已经在行业中得到了更广泛的实践，所以大多数新员工都带着自动化测试的期望来到这里。尽管如此，面向类仍然对测试抱有期望，并将Nooglers对Google之外的测试的了解与在我们非常庞大和非常复杂的代码库中进行测试的挑战联系起来。

### 测试认证 Test Certified

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Initially, the larger and more complex parts of our codebase appeared resistant to good testing practices. Some projects had such poor code quality that they were almost impossible to test. To give projects a clear path forward, the Testing Grouplet devised a certification program that they called Test Certified. Test Certified aimed to give teams a way to understand the maturity of their testing processes and, more critically, cookbook instructions on how to improve it.
</div></details>

最初，我们的代码库中更大、更复杂的部分似乎对良好的测试实践有所抵触。有些项目的代码质量很差，几乎不可能进行测试。为了给项目一个清晰的前进道路，测试小组设计了一个他们称之为测试认证的认证程序。Test Certified旨在为团队提供一种方法来理解他们的测试过程的成熟度，更重要的是，提供关于如何改进测试过程的烹饪指南。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The program was organized into five levels, and each level required some concrete actions to improve the test hygiene on the team. The levels were designed in such a way that each step up could be accomplished within a quarter, which made it a convenient fit for Google’s internal planning cadence.
</div></details>

该计划被组织成五个级别，每个级别需要一些具体的行动来改善团队中的测试卫生。这些关卡的设计是这样的，每个步骤都可以在一个季度内完成，这使得它非常适合Google的内部规划节奏。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Test Certified Level 1 covered the basics: set up a continuous build; start tracking code coverage; classify all your tests as small, medium, or large; identify (but don’t necessarily fix) flaky tests; and create a set of fast (not necessarily comprehensive) tests that can be run quickly. Each subsequent level added more challenges like “no releases with broken tests” or “remove all nondeterministic tests.” By Level 5, all tests were automated, fast tests were running before every commit, all nondeterminism had been removed, and every behavior was covered. An internal dashboard applied social pressure by showing the level of every team. It wasn’t long before teams were competing with one another to climb the ladder.
</div></details>

Test Certified Level 1涵盖了基本内容:建立一个持续构建;开始跟踪代码覆盖率;将所有测试分类为小、中、大;确定(但不一定修复)不稳定的测试;并创建一组可以快速运行的快速(不一定是全面的)测试。每一个随后的关卡都增加了更多的挑战，比如“没有带有中断测试的发布”或“移除所有不确定性测试”。到第5级，所有的测试都自动化了，在每次提交之前都运行快速测试，所有的不确定性都被消除了，并且涵盖了所有的行为。内部仪表盘通过显示每个团队的级别来施加社会压力。没过多久，团队就开始互相竞争爬上梯子。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
By the time the Test Certified program was replaced by an automated approach in 2015 (more on pH later), it had helped more than 1,500 projects improve their testing culture.
</div></details>

到2015年，测试认证程序被自动化方法取代(稍后会详细介绍pH值)，它已经帮助超过1500个项目改善了他们的测试文化。

### 马桶测试 Testing on the Toilet

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Of all the methods the Testing Grouplet used to try to improve testing at Google, perhaps none was more off-beat than Testing on the Toilet (TotT). The goal of TotT was fairly simple: actively raise awareness about testing across the entire company. The question is, what’s the best way to do that in a company with employees scattered around the world?
</div></details>
在测试组用来尝试改进 Google 测试的所有方法中，也许没有比上厕所测试 (TotT) 更另类的方法了。 TotT 的目标相当简单：积极提高整个公司对测试的认识。问题是，在一家员工分散在世界各地的公司中，最好的方法是什么？

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The Testing Grouplet considered the idea of a regular email newsletter, but given the heavy volume of email everyone deals with at Google, it was likely to become lost in the noise. After a little bit of brainstorming, someone proposed the idea of posting flyers in the restroom stalls as a joke. We quickly recognized the genius in it: the bathroom is one place that everyone must visit at least once each day, no matter what. Joke or not, the idea was cheap enough to implement that it had to be tried.
</div></details>
Testing Grouplet 考虑过定期发送电子邮件通讯的想法，但鉴于 Google 的每个人都处理大量电子邮件，它很可能会迷失在噪音中。经过一番头脑风暴后，有人提出在厕所隔间张贴传单的想法是一个笑话。我们很快就发现了其中的天才：浴室是每个人每天都必须至少去一次的地方，无论如何。不管是不是开玩笑，这个想法很便宜，可以实施，必须尝试一下。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In April 2006, a short writeup covering how to improve testing in Python appeared in restroom stalls across Google. This first episode was posted by a small band of volunteers. To say the reaction was polarized is an understatement; some saw it as an invasion of personal space, and they objected strongly. Mailing lists lit up with complaints, but the TotT creators were content: the people complaining were still talking about testing.
</div></details>
2006 年 4 月，一篇关于如何改进 Python 测试的简短文章出现在 Google 的洗手间摊位上。第一集是由一小群志愿者发布的。说反应是两极分化是轻描淡写的。有些人认为这是对个人空间的侵犯，他们强烈反对。邮件列表充满了抱怨，但 TotT 的创建者很满意：抱怨的人仍在谈论测试。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Ultimately, the uproar subsided and TotT quickly became a staple of Google culture. To date, engineers from across the company have produced several hundred episodes, covering almost every aspect of testing imaginable (in addition to a variety of other technical topics). New episodes are eagerly anticipated and some engineers even volunteer to post the episodes around their own buildings. We intentionally limit each episode to exactly one page, challenging authors to focus on the most important and actionable advice. A good episode contains something an engineer can take back to the desk immediately and try.
</div></details>
最终，骚动平息了，TotT 迅速成为Google文化的主要内容。迄今为止，整个公司的工程师已经制作了数百集，几乎涵盖了可以想象的测试的各个方面（除了各种其他技术主题）。新剧集备受期待，一些工程师甚至自愿在他们自己的建筑物周围发布剧集。我们有意将每一集限制在一页，挑战作者专注于最重要和可操作的建议。一个好的情节包含工程师可以立即带回办公桌并尝试的东西。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Ironically for a publication that appears in one of the more private locations, TotT has had an outsized public impact. Most external visitors see an episode at some point in their visit, and such encounters often lead to funny conversations about how Googlers always seem to be thinking about code. Additionally, TotT episodes make great blog posts, something the original TotT authors recognized early on. They began publishing lightly edited versions publicly, helping to share our experience with the industry at large.
</div></details>
具有讽刺意味的是，对于出现在一个更私密的地方的出版物来说，TotT 产生了巨大的公众影响。大多数外部访问者在他们访问的某个时间点都会看到一个插曲，而这种相遇通常会引发关于 Google 员工似乎总是在思考代码的有趣对话。此外，TotT 剧集制作了很棒的博客文章，这是最初的 TotT 作者很早就认识到的。他们开始公开发布经过轻微编辑的版本，帮助与整个行业分享我们的经验。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Despite starting as a joke, TotT has had the longest run and the most profound impact of any of the testing initiatives started by the Testing Grouplet.
</div></details>
尽管一开始是个玩笑，但在 Testing Grouplet 发起的所有测试计划中，TotT 的运行时间最长且影响最深远。

### 现在的测试文化 Testing Culture Today

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Testing culture at Google today has come a long way from 2005. Nooglers still attend orientation classes on testing, and TotT continues to be distributed almost weekly. However, the expectations of testing have more deeply embedded themselves in the daily developer workflow.
</div></details>
与 2005 年相比，今天 Google 的测试文化已经走过了漫长的道路。Nooglers 仍然参加有关测试的定向课程，并且几乎每周都会分发 TotT。然而，测试的期望已经更深入地嵌入到日常开发人员工作流程中。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Every code change at Google is required to go through code review. And every change is expected to include both the feature code and tests. Reviewers are expected to review the quality and correctness of both. In fact, it is perfectly reasonable to block a change if it is missing tests.
</div></details>
Google 的每一次代码更改都需要经过代码审查。并且每次更改都应包括功能代码和测试。审稿人应审查两者的质量和正确性。事实上，如果更改缺少测试，则阻止更改是完全合理的。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
As a replacement for Test Certified, one of our engineering productivity teams recently launched a tool called Project Health (pH). The pH tool continuously gathers dozens of metrics on the health of a project, including test coverage and test latency, and makes them available internally. pH is measured on a scale of one (worst) to five (best). A pH-1 project is seen as a problem for the team to address. Almost every team that runs a continuous build automatically gets a pH score.
</div></details>
作为 Test Certified 的替代品，我们的一个工程生产力团队最近推出了一款名为 Project Health (pH) 的工具。 pH 工具不断收集有关项目健康状况的数十个指标，包括测试覆盖率和测试延迟，并使其在内部可用。 pH值的测量范围为一（最差）到五（最好）。 pH-1 项目被视为团队需要解决的问题。几乎每个运行连续构建的团队都会自动获得 pH 分数。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Over time, testing has become an integral part of Google’s engineering culture. We have myriad ways to reinforce its value to engineers across the company. Through a combination of training, gentle nudges, mentorship, and, yes, even a little friendly competition, we have created the clear expectation that testing is everyone’s job.
</div></details>
随着时间的推移，测试已成为 Google 工程文化不可或缺的一部分。我们有无数种方法可以增强其对整个公司工程师的价值。通过培训、温和的推动、指导，甚至是一点点友好的竞争，我们已经明确期望测试是每个人的工作。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Why didn’t we start by mandating the writing of tests?
</div></details>
为什么我们不从强制编写测试开始呢？

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The Testing Grouplet had considered asking for a testing mandate from senior leadership but quickly decided against it. Any mandate on how to develop code would be seriously counter to Google culture and likely slow the progress, independent of the idea being mandated. The belief was that successful ideas would spread, so the focus became demonstrating success.
</div></details>
测试小组曾考虑向高层领导提出测试任务，但很快就决定不这样做。任何关于如何开发代码的授权都将严重违背Google文化，并且可能会减缓进度，与授权的想法无关。人们相信成功的想法会传播开来，因此重点变成了展示成功。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
If engineers were deciding to write tests on their own, it meant that they had fully accepted the idea and were likely to keep doing the right thing—even if no one was compelling them to.
</div></details>
如果工程师决定自己编写测试，这意味着他们已经完全接受了这个想法，并且可能会继续做正确的事情——即使没有人强迫他们这样做。





[^9]:This class was so successful that an updated version is still taught today. In fact, it is one of the longestrunning orientation classes in the company’s history.


