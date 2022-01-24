
## 为什么我们需要编写测试 Why Do We Write Tests?

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

To better understand how to get the most out of testing, let’s start from the beginning. When we talk about automated testing, what are we really talking about? The simplest test is defined by: 
- A single behavior you are testing, usually a method or API that you are calling 
- A specific input, some value that you pass to the API 
- An observable output or behavior
- A controlled environment such as a single isolated process
</div></details>

为了更好地了解如何充分利用测试，让我们从头开始。当我们谈论自动化测试时，我们真正在谈论什么？最简单的测试定义为：
- 您正在测试的单个行为，通常是您正在调用的方法或 API
- 特定输入，您传递给 API 的一些值
- 可观察到的输出或行为
- 受控环境，例如单个隔离过程

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
When you execute a test like this, passing the input to the system and verifying the output, you will learn whether the system behaves as you expect. Taken in aggregate, hundreds or thousands of simple tests (usually called a test suite) can tell you how well your entire product conforms to its intended design and, more important, when it doesn’t.
</div></details>
当您执行这样的测试，将输入传递给系统并验证输出时，您将了解系统是否按预期运行。总的来说，数百或数千个简单测试（通常称为测试套件）可以告诉您整个产品与其预期设计的符合程度，更重要的是，何时不符合。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Creating and maintaining a healthy test suite takes real effort. As a codebase grows, so too will the test suite. It will begin to face challenges like instability and slowness. A failure to address these problems will cripple a test suite. Keep in mind that tests derive their value from the trust engineers place in them. If testing becomes a productivity sink, constantly inducing toil and uncertainty, engineers will lose trust and begin to find workarounds. A bad test suite can be worse than no test suite at all.
</div></details>
创建和维护一个健康的测试套件需要付出真正的努力。随着代码库的增长，测试套件也会增长。它将开始面临不稳定和缓慢等挑战。未能解决这些问题将削弱测试套件。请记住，测试的价值来自工程师对它们的信任。如果测试成为生产力的下沉，不断地导致劳动和不确定性，工程师将失去信任并开始寻找解决方法。一个糟糕的测试套件可能比没有测试套件更糟糕。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In addition to empowering companies to build great products quickly, testing is becoming critical to ensuring the safety of important products and services in our lives. Software is more involved in our lives than ever before, and defects can cause more than a little annoyance: they can cost massive amounts of money, loss of property, or, worst of all, loss of life.
</div></details>

除了使公司能够快速构建出色的产品外，测试对于确保我们生活中重要产品和服务的安全性也变得至关重要。软件比以往任何时候都更多地参与到我们的生活中，而缺陷可能会带来更多的烦恼：它们可能会造成大量金钱损失、财产损失，或者最糟糕的是，造成生命损失。[^2]

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
At Google, we have determined that testing cannot be an afterthought. Focusing on quality and testing is part of how we do our jobs. We have learned, sometimes painfully, that failing to build quality into our products and services inevitably leads to bad outcomes. As a result, we have built testing into the heart of our engineering culture.
</div></details>
在 Google，我们已经确定测试不能是事后的想法。专注于质量和测试是我们工作的一部分。我们有时痛苦地了解到，未能在我们的产品和服务中建立质量不可避免地会导致糟糕的结果。因此，我们已将测试纳入我们工程文化的核心。

### The Story of Google Web Server

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In Google’s early days, engineer-driven testing was often assumed to be of little importance. Teams regularly relied on smart people to get the software right. A few systems ran large integration tests, but mostly it was the Wild West. One product in particular seemed to suffer the worst: it was called the Google Web Server, also known as GWS.
</div></details>

在Google的早期，工程师驱动的测试通常被认为并不重要。团队经常依靠聪明的人来获得正确的软件。一些系统运行了大型集成测试，但主要是狂野西部。尤其是一种产品似乎遭受了最严重的损失：它被称为 Google Web Server，也称为 GWS。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
GWS is the web server responsible for serving Google Search queries and is as important to Google Search as air traffic control is to an airport. Back in 2005, as the project swelled in size and complexity, productivity had slowed dramatically. Releases were becoming buggier, and it was taking longer and longer to push them out. Team members had little confidence when making changes to the service, and often found out something was wrong only when features stopped working in production. (At one point, more than 80% of production pushes contained user-affecting bugs that had to be rolled back.)

</div></details>

GWS 是负责为 Google 搜索查询提供服务的网络服务器，它对 Google 搜索的重要性不亚于空中交通管制对机场的重要性。早在 2005 年，随着项目规模和复杂性的扩大，生产力急剧下降。发布变得越来越错误，将它们推出所需的时间越来越长。团队成员在对服务进行更改时缺乏信心，并且经常只有在功能停止在生产中工作时才发现有问题。 （在某一时刻，超过 80% 的生产推送包含必须回滚的影响用户的错误。）

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
To address these problems, the tech lead (TL) of GWS decided to institute a policy of engineer-driven, automated testing. As part of this policy, all new code changes were required to include tests, and those tests would be run continuously. Within a year of instituting this policy, the number of emergency pushes dropped by half. This drop occurred despite the fact that the project was seeing a record number of new changes every quarter. Even in the face of unprecedented growth and change, testing brought renewed productivity and confidence to one of the most critical projects at Google. Today, GWS has tens of thousands of tests, and releases almost every day with relatively few customer-visible failures.
</div></details>
为了解决这些问题，GWS 的技术主管 (TL) 决定制定工程师驱动的自动化测试政策。作为该政策的一部分，所有新的代码更改都必须包含测试，并且这些测试将持续运行。该政策实施一年内，紧急推送次数减少了一半。尽管该项目每个季度都有创纪录的新变化，但还是出现了这种下降。即使面对前所未有的增长和变化，测试也为 Google 最关键的项目之一带来了新的生产力和信心。如今，GWS 已经进行了数以万计的测试，并且几乎每天都会发布，客户可见的故障相对较少。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The changes in GWS marked a watershed for testing culture at Google as teams in other parts of the company saw the benefits of testing and moved to adopt similar tactics.
</div></details>

GWS 的变化标志着Google测试文化的一个分水岭，因为公司其他部门的团队看到了测试的好处，并采取了类似的策略。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
One of the key insights the GWS experience taught us was that you can’t rely on programmer ability alone to avoid product defects. Even if each engineer writes only the occasional bug, after you have enough people working on the same project, you will be swamped by the ever-growing list of defects. Imagine a hypothetical 100-person team whose engineers are so good that they each write only a single bug a month. Collectively, this group of amazing engineers still produces five new bugs every workday. Worse yet, in a complex system, fixing one bug can often cause another, as engineers adapt to known bugs and code around them.
</div></details>

GWS 经验告诉我们的关键见解之一是，您不能仅依靠程序员的能力来避免产品缺陷。即使每个工程师只写偶尔的错误，当你有足够多的人在同一个项目上工作时，你也会被不断增长的缺陷列表所淹没。想象一个假设的 100 人团队，其工程师非常优秀，以至于每个人每个月只写一个 bug。总的来说，这群了不起的工程师每个工作日仍然会产生五个新的错误。更糟糕的是，在一个复杂的系统中，修复一个错误通常会导致另一个错误，因为工程师会适应已知的错误和围绕它们的代码。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The best teams find ways to turn the collective wisdom of its members into a benefit for the entire team. That is exactly what automated testing does. After an engineer on the team writes a test, it is added to the pool of common resources available to others. Everyone else on the team can now run the test and will benefit when it detects an issue. Contrast this with an approach based on debugging, wherein each time a bug occurs, an engineer must pay the cost of digging into it with a debugger. The cost in engineering resources is night and day and was the fundamental reason GWS was able to turn its fortunes around.
</div></details>

最好的团队想方设法将其成员的集体智慧转化为整个团队的利益。这正是自动化测试所做的。团队中的工程师编写测试后，会将其添加到其他人可用的公共资源池中。团队中的其他人现在都可以运行测试，并在检测到问题时受益。将此与基于调试的方法进行对比，其中每次发生错误时，工程师都必须支付使用调试器进行挖掘的成本。工程资源的成本是日以继夜的，这是 GWS 能够扭转命运的根本原因。

### Testing at the Speed of Modern Development

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Software systems are growing larger and ever more complex. A typical application or service at Google is made up of thousands or millions of lines of code. It uses hundreds of libraries or frameworks and must be delivered via unreliable networks to an increasing number of platforms running with an uncountable number of configurations. To make matters worse, new versions are pushed to users frequently, sometimes multiple times each day. This is a far cry from the world of shrink-wrapped software that saw updates only once or twice a year.
</div></details>
软件系统变得越来越大，越来越复杂。 Google 的典型应用程序或服务由数千或数百万行代码组成。它使用数百个库或框架，并且必须通过不可靠的网络交付给越来越多的运行着无数配置的平台。更糟糕的是，新版本被频繁地推送给用户，有时每天多次推送。这与每年只更新一两次的收缩包装软件的世界相去甚远。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The ability for humans to manually validate every behavior in a system has been unable to keep pace with the explosion of features and platforms in most software. Imagine what it would take to manually test all of the functionality of Google Search, like finding flights, movie times, relevant images, and of course web search results (see Figure 11-1). Even if you can determine how to solve that problem, you then need to multiply that workload by every language, country, and device Google Search must support, and don’t forget to check for things like accessibility and security. Attempting to assess product quality by asking humans to manually interact with every feature just doesn’t scale. When it comes to testing, there is one clear answer: automation
</div></details>

人类手动验证系统中每个行为的能力已经无法跟上大多数软件中功能和平台的爆炸式增长。想象一下手动测试 Google 搜索的所有功能需要什么，比如查找航班、电影时间、相关图像，当然还有网络搜索结果（见图 11-1）。即使您可以确定如何解决该问题，您也需要将工作量乘以 Google 搜索必须支持的每种语言、国家和设备，并且不要忘记检查可访问性和安全性等内容。试图通过要求人类手动与每个功能交互来评估产品质量是无法扩展的。谈到测试，有一个明确的答案：自动化

<img src='./pic/fingure-11.1.png' style='width:100%'>

### Write, Run, React

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In its purest form, automating testing consists of three activities: writing tests, running tests, and reacting to test failures. An automated test is a small bit of code, usually a single function or method, that calls into an isolated part of a larger system that you want to test. The test code sets up an expected environment, calls into the system, usually with a known input, and verifies the result. Some of the tests are very small, exercising a single code path; others are much larger and can involve entire systems, like a mobile operating system or web browser.
</div></details>

就其最纯粹的形式而言，自动化测试包括三个活动：编写测试、运行测试和对测试失败做出反应。 自动化测试是一小段代码，通常是单个函数或方法，它调用您想要测试的更大系统的一个隔离部分。 测试代码设置一个预期的环境，调用系统，通常使用已知的输入，并验证结果。 一些测试非常小，执行单个代码路径； 其他的则更大，可能涉及整个系统，例如移动操作系统或 Web 浏览器。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Example 11-1 presents a deliberately simple test in Java using no frameworks or testing libraries. This is not how you would write an entire test suite, but at its core every automated test looks similar to this very simple example.

Example 11-1. An example test
</div></details>
示例 11-1 展示了一个特意使用 Java 进行的简单测试，不使用任何框架或测试库。 这不是您编写整个测试套件的方式，但在其核心，每个自动化测试看起来都类似于这个非常简单的示例。

例 11-1。 示例测试

```c++
// Verifies a Calculator class can handle negative results.
public void main(String[] args) {
 Calculator calculator = new Calculator();
 int expectedResult = -3;
 int actualResult = calculator.subtract(2, 5); // Given 2, Subtracts 5.
 assert(expectedResult == actualResult);
}
```

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Unlike the QA processes of yore, in which rooms of dedicated software testers pored over new versions of a system, exercising every possible behavior, the engineers who build systems today play an active and integral role in writing and running automated tests for their own code. Even in companies where QA is a prominent organization, developer-written tests are commonplace. At the speed and scale that today’s systems are being developed, the only way to keep up is by sharing the development of tests around the entire engineering staff.
</div></details>

与过去的 QA 流程不同，在过去的 QA 流程中，专门的软件测试人员在房间里仔细研究系统的新版本，练习每一种可能的行为，而如今构建系统的工程师在为自己的代码编写和运行自动化测试方面发挥着积极而不可或缺的作用。即使在 QA 是一个重要组织的公司中，开发人员编写的测试也是司空见惯的。以当今系统开发的速度和规模，跟上步伐的唯一方法是在整个工程人员中共享测试的开发。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Of course, writing tests is different from writing good tests. It can be quite difficult to train tens of thousands of engineers to write good tests. We will discuss what we have learned about writing good tests in the chapters that follow.
</div></details>
当然，编写测试不同于编写好的测试。培训成千上万的工程师编写好的测试是相当困难的。我们将在接下来的章节中讨论我们学到的关于编写好的测试的知识。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Writing tests is only the first step in the process of automated testing. After you have written tests, you need to run them. Frequently. At its core, automated testing consists of repeating the same action over and over, only requiring human attention when something breaks. We will discuss this Continuous Integration (CI) and testing in Chapter 23. By expressing tests as code instead of a manual series of steps, we can run them every time the code changes—easily thousands of times per day. Unlike human testers, machines never grow tired or bored.
</div></details>
编写测试只是自动化测试过程中的第一步。编写完测试后，您需要运行它们。频繁地。自动化测试的核心是一遍又一遍地重复相同的操作，只有在出现问题时才需要人工注意。我们将在第 23 章讨论这种持续集成（CI）和测试。通过将测试表达为代码而不是手动的一系列步骤，我们可以在每次代码更改时运行它们——每天轻松数千次。与人类测试人员不同，机器永远不会感到疲倦或无聊。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Another benefit of having tests expressed as code is that it is easy to modularize them for execution in various environments. Testing the behavior of Gmail in Firefox requires no more effort than doing so in Chrome, provided you have configurations for both of these systems.Running tests for a user interface (UI) in Japanese or German can be done using the same test code as for English.
</div></details>

将测试表示为代码的另一个好处是可以很容易地将它们模块化以在各种环境中执行。在 Firefox 中测试 Gmail 的行为与在 Chrome 中测试一样，只要您对这两个系统都有配置。[^3]运行日语或德语用户界面 (UI) 的测试可以使用相同的测试代码来完成英语。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Products and services under active development will inevitably experience test failures. What really makes a testing process effective is how it addresses test failures. Allowing failing tests to pile up quickly defeats any value they were providing, so it is imperative not to let that happen. Teams that prioritize fixing a broken test within minutes of a failure are able to keep confidence high and failure isolation fast, and therefore derive more value out of their tests.
</div></details>
积极开发中的产品和服务不可避免地会出现测试失败。真正使测试过程有效的是它如何解决测试失败。让失败的测试迅速堆积起来会破坏他们提供的任何价值，因此必须不要让这种情况发生。优先在失败后几分钟内修复损坏的测试的团队能够保持高信心和快速隔离故障，因此从他们的测试中获得更多价值。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In summary, a healthy automated testing culture encourages everyone to share the work of writing tests. Such a culture also ensures that tests are run regularly. Last, and perhaps most important, it places an emphasis on fixing broken tests quickly so as to maintain high confidence in the process.
</div></details>
总之，健康的自动化测试文化鼓励每个人分享编写测试的工作。这种文化还确保定期运行测试。最后，也许也是最重要的一点，它强调快速修复损坏的测试，以保持对该过程的高度信心。

### 测试代码的好处Benefits of Testing Code
<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
To developers coming from organizations that don’t have a strong testing culture, the idea of writing tests as a means of improving productivity and velocity might seem antithetical. After all, the act of writing tests can take just as long (if not longer!) than implementing a feature would take in the first place. On the contrary, at Google, we’ve found that investing in software tests provides several key benefits to developer productivity:
</div></details>

对于来自没有强大测试文化的组织的开发人员来说，编写测试作为一种提高生产力和速度的方法的想法似乎是对立的。毕竟，编写测试所花费的时间可能和实现特性所花费的时间一样长(如果不是更长的话!)。相反，在Google，我们发现在软件测试上的投资为开发人员的生产力提供了几个关键的好处:

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

*Less debugging*

As you would expect, tested code has fewer defects when it is submitted. Critically, it also has fewer defects throughout its existence; most of them will be caught before the code is submitted. A piece of code at Google is expected to be modified dozens of times in its lifetime. It will be changed by other teams and even automated code maintenance systems. A test written once continues to pay dividends and prevent costly defects and annoying debugging sessions through the lifetime of the project. Changes to a project, or the dependencies of a project, that break a test can be quickly detected by test infrastructure and rolled back before the problem is ever released to production.

</div></details>

*更少的调试*

正如您所期望的那样，经过测试的代码在提交时缺陷更少。关键的是，它在整个存在过程中也有较少的缺陷;它们中的大多数将在代码提交之前被捕获。Google的一段代码在其生命周期中预计会被修改数十次。它将由其他团队甚至自动化代码维护系统进行更改。在项目的整个生命周期中，编写一次的测试将继续带来好处，并防止代价高昂的缺陷和恼人的调试会话。对项目或项目依赖项的更改会破坏测试，测试基础设施可以快速检测到这些更改，并在问题发布到生产环境之前进行回滚。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

*Increased confidence in changes*

All software changes. Teams with good tests can review and accept changes to their project with confidence because all important behaviors of their project are continuously verified. Such projects encourage refactoring. Changes that refactor code while preserving existing behavior should (ideally) require no changes to existing tests.
</div></details>

*增加了对变更的信心*

所有的软件更改。拥有良好测试的团队可以满怀信心地审查和接受对项目的更改，因为项目的所有重要行为都得到了持续的验证。这样的项目鼓励重构。在保留现有行为的同时重构代码的更改(理想情况下)应该不需要更改现有的测试。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

*Improved documentation*

Software documentation is notoriously unreliable. From outdated requirements to missing edge cases, it is common for documentation to have a tenuous relationship to the code. Clear, focused tests that exercise one behavior at a time function as executable documentation. If you want to know what the code does in a particular case, look at the test for that case. Even better, when requirements change and new code breaks an existing test, we get a clear signal that the “documentation” is now out of date. Note that tests work best as documentation only if care is taken to keep them clear and concise.
</div></details>

*改进的文档*

众所周知，软件文档是不可靠的。从过时的需求到缺失的边缘用例，文档与代码之间的关系通常很脆弱。一次执行一个行为的清晰、集中的测试可以作为可执行文档。如果您想知道代码在特定情况下做什么，请查看该情况的测试。更棒的是，当需求变更和新代码破坏了现有的测试时，我们会得到一个明确的信号，即“文档”现在已经过时了。请注意，只有注意保持测试的清晰和简洁，测试才能作为文档工作得最好。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

*Simpler reviews*

All code at Google is reviewed by at least one other engineer before it can be submitted (see Chapter 9 for more details). A code reviewer spends less effort verifying code works as expected if the code review includes thorough tests that demonstrate code correctness, edge cases, and error conditions. Instead of the tedious effort needed to mentally walk each case through the code, the reviewer can verify that each case has a passing test. 
</div></details>

*简单的评论*

所有在Google的代码在提交之前，至少要经过另一名工程师的审核(详情请参阅第9章)。如果代码评审包括演示代码正确性、边缘情况和错误条件的完整测试，那么代码评审人员在验证代码是否按预期工作时所花的精力就会更少。审查员可以验证每个用例都有一个通过的测试，而不是需要在心里对每个用例进行冗长乏味的工作。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

*Thoughtful design*

Writing tests for new code is a practical means of exercising the API design of the code itself. If new code is difficult to test, it is often because the code being tested has too many responsibilities or difficult-to-manage dependencies. Welldesigned code should be modular, avoiding tight coupling and focusing on specific responsibilities. Fixing design issues early often means less rework later. 
</div></details>

*深思熟虑的设计*

为新代码编写测试是实践代码本身API设计的一种实际方法。如果新代码难以测试，通常是因为被测试的代码有太多的职责或难以管理的依赖项。设计良好的代码应该是模块化的，避免紧密耦合，专注于特定的职责。尽早解决设计问题通常意味着更少的返工。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

*Fast, high-quality releases*

With a healthy automated test suite, teams can release new versions of their application with confidence. Many projects at Google release a new version to production every day—even large projects with hundreds of engineers and thousands of code changes submitted every day. This would not be possible without automated testing.

</div></details>

*快速、高质量的发布*

有了一个健康的自动化测试套件，团队可以满怀信心地发布他们应用程序的新版本。Google的许多项目每天都会发布一个新版本，即使是大型项目，每天都有数百名工程师和数千个代码更改提交。如果没有自动化测试，这是不可能的。





[^2]:See “[Failure at Dhahran](https://oreil.ly/lhO7Z).”
[^3]:Getting the behavior right across different browsers and languages is a different story! But, ideally, the enduser experience should be the same for everyone.
