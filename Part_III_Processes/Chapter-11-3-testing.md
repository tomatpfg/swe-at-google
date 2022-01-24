## Google规模级别的测试 Testing at Google Scale
<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Much of the guidance to this point can be applied to codebases of almost any size. However, we should spend some time on what we have learned testing at our very large scale. To understand how testing works at Google, you need an understanding of our development environment, the most important fact about which is that most of Google’s code is kept in a single, monolithic repository (monorepo). Almost every line of code for every product and service we operate is all stored in one place. We have more than two billion lines of code in the repository today.
</div></details>

关于这一点的大部分指导都可以应用于几乎任何大小的代码库。然而，我们应该花一些时间在我们所学到的大规模测试上。要理解在Google中测试是如何工作的，你需要了解我们的开发环境，最重要的事实是，Google的大部分代码都保存在一个单一的、整体的存储库(monorepo)中。我们运营的每一种产品和服务的几乎每一行代码都存储在一个地方。今天，我们的存储库中有超过20亿行代码。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Google’s codebase experiences close to 25 million lines of change every week. Roughly half of them are made by the tens of thousands of engineers working in our monorepo, and the other half by our automated systems, in the form of configuration updates or large-scale changes (Chapter 22). Many of those changes are initiated from outside the immediate project. We don’t place many limitations on the ability of engineers to reuse code.
</div></details>
Google的代码库每周经历近2500万行更改。其中大约一半是由在我们的monorepo工作的成千上万的工程师完成的，另一半是由我们的自动化系统完成的，以配置更新或大规模更改的形式(第22章)。这些更改中的许多都是从当前项目的外部开始的。我们没有对工程师重用代码的能力设置很多限制。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The openness of our codebase encourages a level of co-ownership that lets everyone take responsibility for the codebase. One benefit of such openness is the ability to directly fix bugs in a product or service you use (subject to approval, of course) instead of complaining about it. This also implies that many people will make changes in a part of the codebase owned by someone else.
</div></details>
我们代码库的开放鼓励了某种程度的共同所有权，让每个人都对代码库负责。这种开放性的一个好处是能够直接修复您使用的产品或服务中的bug(当然，需要经过批准)，而不是抱怨它。这也意味着许多人将对其他人拥有的代码库的一部分进行更改。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Another thing that makes Google a little different is that almost no teams use repository branching. All changes are committed to the repository head and are immediately visible for everyone to see. Furthermore, all software builds are performed using the last committed change that our testing infrastructure has validated. When a product or service is built, almost every dependency required to run it is also built from source, also from the head of the repository. Google manages testing at this scale by use of a CI system. One of the key components of our CI system is our Test Automated Platform (TAP).
</div></details>
让Google有点不同的另一件事是，几乎没有团队使用存储库分支。所有的更改都被提交到存储库头部，并且每个人都可以立即看到。此外，所有的软件构建都是使用我们的测试基础设施已经验证过的最后提交的变更来执行的。当构建产品或服务时，运行它所需的几乎所有依赖项都是从源代码构建的，也就是从存储库的头部构建的。Google通过使用CI系统来管理这种规模的测试。我们的CI系统的一个关键组成部分是我们的测试自动化平台(TAP)。

<div> <image src='./pic/bird.png' width=60 style="float:left"/>
For more information on TAP and our CI philosophy, see <a href='./Chapter-22-0.md'>Chapter 23.</a>
</div>
<br/>
<br/>

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Whether you are considering our size, our monorepo, or the number of products we offer, Google’s engineering environment is complex. Every week it experiences millions of changing lines, billions of test cases being run, tens of thousands of binaries being built, and hundreds of products being updated—talk about complicated!
</div></details>

无论您是在考虑我们的规模、单品还是我们提供的产品数量，Google的工程环境都是复杂的。它每周都要经历数百万行变更，数十亿个测试用例正在运行，数万个二进制文件正在构建，数百个产品正在更新——这真是太复杂了!

### 大型测试的缺陷 The Pitfalls of a Large Test Suite


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
As a codebase grows, you will inevitably need to make changes to existing code. When poorly written, automated tests can make it more difficult to make those changes. Brittle tests—those that over-specify expected outcomes or rely on extensive and complicated boilerplate—can actually resist change. These poorly written tests can fail even when unrelated changes are made.
</div></details>

随着代码库的增长，您将不可避免地需要对现有代码进行更改。如果写得不好，自动化测试会使更改变得更加困难。脆弱的测试——那些过分指定预期结果或依赖广泛而复杂的样板的测试——实际上会抵制改变。即使进行了不相关的更改，这些编写得很差的测试也可能失败。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
If you have ever made a five-line change to a feature only to find dozens of unrelated, broken tests, you have felt the friction of brittle tests. Over time, this friction can make a team reticent to perform necessary refactoring to keep a codebase healthy. The subsequent chapters will cover strategies that you can use to improve the robustness and quality of your tests.
</div></details>
如果您曾经对一个特性进行了五行更改，结果却发现了许多不相关的、破碎的测试，那么您就会感受到脆性测试的摩擦。随着时间的推移，这种摩擦会使团队不愿执行必要的重构以保持代码库的健康。后面的章节将涵盖您可以用来提高测试的健壮性和质量的策略。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Some of the worst offenders of brittle tests come from the misuse of mock objects. Google’s codebase has suffered so badly from an abuse of mocking frameworks that it has led some engineers to declare “no more mocks!” Although that is a strong statement, understanding the limitations of mock objects can help you avoid misusing them
</div></details>
一些最容易破坏测试的因素来自对模拟对象的误用。Google的代码库受到了模仿框架滥用的严重影响，以至于一些工程师宣称“不要再模仿了!”尽管这是一个强有力的声明，但理解模拟对象的限制可以帮助您避免滥用它们

<div> <image src='./pic/bird.png' width=60 style="float:left"/>
For more information on working effectively with mock objects, see <a href='./Chapter-13-0.md'>Chapter 13.</a>
</div>
<br/>
<br/>

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In addition to the friction caused by brittle tests, a larger suite of tests will be slower to run. The slower a test suite, the less frequently it will be run, and the less benefit it provides. We use a number of techniques to speed up our test suite, including parallelizing execution and using faster hardware. However, these kinds of tricks are eventually swamped by a large number of individually slow test cases.
</div></details>
除了脆性测试造成的摩擦外，更大规模的测试将会运行得更慢。测试套件运行得越慢，运行的频率就越低，所带来的好处也就越少。我们使用了许多技术来加速我们的测试套件，包括并行执行和使用更快的硬件。然而，这些技巧最终会被大量单独缓慢的测试用例所淹没。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Tests can become slow for many reasons, like booting significant portions of a system, firing up an emulator before execution, processing large datasets, or waiting for disparate systems to synchronize. Tests often start fast enough but slow down as the system grows. For example, maybe you have an integration test exercising a single dependency that takes five seconds to respond, but over the years you grow to depend on a dozen services, and now the same tests take five minutes.
</div></details>
测试速度变慢的原因有很多，比如启动系统的重要部分、在执行前启动模拟器、处理大型数据集或等待不同的系统同步。测试通常启动得足够快，但会随着系统的增长而变慢。例如，可能您有一个集成测试执行一个依赖项，它需要5秒钟来响应，但是随着时间的推移，您逐渐依赖于12个服务，现在相同的测试只需要5分钟。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Tests can also become slow due to unnecessary speed limits introduced by functions like sleep() and setTimeout(). Calls to these functions are often used as naive heuristics before checking the result of nondeterministic behavior. Sleeping for half a second here or there doesn’t seem too dangerous at first; however, if a “wait-and-check” is embedded in a widely used utility, pretty soon you have added minutes of idle time to every run of your test suite. A better solution is to actively poll for a state transition with a frequency closer to microseconds. You can combine this with a timeout value in case a test fails to reach a stable state.
</div></details>
由于sleep()和setTimeout()等函数引入了不必要的速度限制，测试也可能变慢。在检查不确定性行为的结果之前，对这些函数的调用通常被用作朴素试探法。一开始，在这里或那里睡上半秒钟似乎并不太危险;但是，如果“等待和检查”嵌入到一个广泛使用的实用程序中，很快您就会在每次运行测试套件时增加几分钟的空闲时间。更好的解决方案是主动轮询频率接近微秒的状态转换。如果测试未能达到稳定状态，您可以将其与超时值结合使用。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Failing to keep a test suite deterministic and fast ensures it will become roadblock to productivity. At Google, engineers who encounter these tests have found ways to work around slowdowns, with some going as far as to skip the tests entirely when submitting changes. Obviously, this is a risky practice and should be discouraged, but if a test suite is causing more harm than good, eventually engineers will find a way to get their job done, tests or no tests.
</div></details>

如果不能保证测试套件的确定性和快速性，那么它将成为生产力的障碍。在Google，遇到这些测试的工程师已经找到了解决慢速问题的方法，有些人甚至在提交更改时完全跳过测试。显然，这是一个有风险的实践，应该被劝阻，但是如果测试套件造成的危害大于好处，工程师最终会找到一种方法来完成他们的工作，测试或者不测试。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The secret to living with a large test suite is to treat it with respect. Incentivize engineers to care about their tests; reward them as much for having rock-solid tests as you would for having a great feature launch. Set appropriate performance goals and refactor slow or marginal tests. Basically, treat your tests like production code. When simple changes begin taking nontrivial time, spend effort making your tests less brittle.
</div></details>
使用大型测试套件的秘密是要尊重它。鼓励工程师关心他们的测试;奖励他们进行了可靠的测试，就像奖励他们发布了出色的功能一样。设置适当的性能目标，重构缓慢或边缘测试。基本上，将测试视为生产代码。当简单的更改开始花费大量时间时，请努力使您的测试不那么脆弱。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In addition to developing the proper culture, invest in your testing infrastructure by developing linters, documentation, or other assistance that makes it more difficult to write bad tests. Reduce the number of frameworks and tools you need to support to increase the efficiency of the time you invest to improve things.If you don’t invest in making it easy to manage your tests, eventually engineers will decide it isn’t worth having them at all.
</div></details>

除了开发适当的文化外，还应该通过开发测试器、文档或其他帮助来投资您的测试基础设施，这使得编写糟糕的测试变得更加困难。减少您需要支持的框架和工具的数量，以提高您用于改进的时间的效率[^8]。如果您不投资于简化测试管理，最终工程师会认为根本不值得拥有它们。




[^8]:Each supported language at Google has one standard test framework and one standard mocking/stubbing library. One set of infrastructure runs most tests in all languages across the entire codebase.


