## 设计测试套件 Designing a Test Suite
<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Today, Google operates at a massive scale, but we haven’t always been so large, and the foundations of our approach were laid long ago. Over the years, as our codebase has grown, we have learned a lot about how to approach the design and execution of a test suite, often by making mistakes and cleaning up afterward.
</div></details>

今天，Google的运营规模很大，但我们并不总是那么大，我们的方法的基础很久以前就奠定了。多年来，随着我们代码库的增长，我们学到了很多关于如何设计和执行测试套件的知识，通常是通过犯错和事后清理。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
One of the lessons we learned fairly early on is that engineers favored writing larger, system-scale tests, but that these tests were slower, less reliable, and more difficult to debug than smaller tests. Engineers, fed up with debugging the system-scale tests,asked themselves, “Why can’t we just test one server at a time?” or, “Why do we need to test a whole server at once? We could test smaller modules individually.” Eventually, the desire to reduce pain led teams to develop smaller and smaller tests, which turned out to be faster, more stable, and generally less painful.
</div></details>

我们很早就学到的教训之一是工程师喜欢编写更大的系统规模测试，但这些测试比小型测试更慢、更不可靠且更难调试。工程师们厌倦了调试系统规模的测试，他们问自己：“为什么我们不能一次只测试一台服务器？”或者，“为什么我们需要一次测试整个服务器？我们可以单独测试较小的模块。”最终，减轻疼痛的愿望促使团队开发越来越小的测试，结果证明这些测试更快、更稳定，而且通常疼痛更小。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
This led to a lot of discussion around the company about the exact meaning of “small.” Does small mean unit test? What about integration tests, what size are those? We have come to the conclusion that there are two distinct dimensions for every test case: size and scope. Size refers to the resources that are required to run a test case: things like memory, processes, and time. Scope refers to the specific code paths we are verifying. Note that executing a line of code is different from verifying that it worked as expected. Size and scope are interrelated but distinct concepts.
</div></details>

这在公司内部引发了很多关于“小”的确切含义的讨论。小意味着单元测试吗？那么集成测试呢，它们的大小是多少？我们得出的结论是，每个测试用例都有两个不同的维度：大小和范围。大小是指运行测试用例所需的资源：例如内存、进程和时间。范围是指我们正在验证的特定代码路径。请注意，执行一行代码不同于验证它是否按预期工作。规模和范围是相互关联但又截然不同的概念。

### Test Size

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
At Google, we classify every one of our tests into a size and encourage engineers to always write the smallest possible test for a given piece of functionality. A test’s size is determined not by its number of lines of code, but by how it runs, what it is allowed to do, and how many resources it consumes. In fact, in some cases, our definitions of small, medium, and large are actually encoded as constraints the testing infrastructure can enforce on a test. We go into the details in a moment, but in brief, small tests run in a single process, medium tests run on a single machine, and large tests run wherever they want, as demonstrated in Figure 11-2

</div></details>

在Google，我们对每个测试进行分类，并鼓励工程师总是为给定的功能块编写尽可能小的测试。一个测试的大小不是由它的代码行数决定的，而是由它如何运行、允许它做什么以及它消耗了多少资源决定的。事实上，在某些情况下，我们对小型、中型和大型的定义实际上被编码为测试基础结构可以在测试上强制执行的约束条件。我们稍后会详细介绍，简单地说，小型测试在单个流程中运行，中型测试在单个机器上运行，大型测试在任意位置运行，如图11-2所示。[^4]

<img src='./pic/fingure-11.2.png' width='100%'>

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
We make this distinction, as opposed to the more traditional “unit” or “integration,” because the most important qualities we want from our test suite are speed and determinism, regardless of the scope of the test. Small tests, regardless of the scope, are almost always faster and more deterministic than tests that involve more infrastructure or consume more resources. Placing restrictions on small tests makes speed and determinism much easier to achieve. As test sizes grow, many of the restrictions are relaxed. Medium tests have more flexibility but also more risk of nondeterminism. Larger tests are saved for only the most complex and difficult testing scenarios. Let’s take a closer look at the exact constraints imposed on each type of test.
</div></details>

我们做出这种区分，而不是更传统的“单元”或“集成”，因为我们最希望从我们的测试套件中获得的品质是效率和确定性，而不管测试的范围是什么。无论范围如何，小型测试几乎总是比涉及更多基础设施或消耗更多资源的测试更快、更确定。对小测试设置限制会使速度和确定性更容易实现。随着测试规模的增长，许多限制都放宽了。中等程度的测试更具灵活性，但也有更大的不确定性风险。较大的测试只用于最复杂和最困难的测试场景。让我们仔细看看强加在每种测试类型上的确切约束。

#### Small tests

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Small tests are the most constrained of the three test sizes. The primary constraint is that small tests must run in a single process. In many languages, we restrict this even further to say that they must run on a single thread. This means that the code performing the test must run in the same process as the code being tested. You can’t run a server and have a separate test process connect to it. It also means that you can’t run a third-party program such as a database as part of your test.

The other important constraints on small tests are that they aren’t allowed to sleep, perform I/O operations,or make any other blocking calls. This means that small tests aren’t allowed to access the network or disk. Testing code that relies on these sorts of operations requires the use of test doubles (see Chapter 13) to replace the heavyweight dependency with a lightweight, in-process dependency.

The purpose of these restrictions is to ensure that small tests don’t have access to the main sources of test slowness or nondeterminism. A test that runs on a single process and never makes blocking calls can effectively run as fast as the CPU can handle. It’s difficult (but certainly not impossible) to accidentally make such a test slow or nondeterministic. The constraints on small tests provide a sandbox that prevents engineers from shooting themselves in the foot.

These restrictions might seem excessive at first, but consider a modest suite of a couple hundred small test cases running throughout the day. If even a few of them fail nondeterministically (often called flaky tests), tracking down the cause becomes a serious drain on productivity. At Google’s scale, such a problem could grind our testing infrastructure to a halt.

At Google, we encourage engineers to try to write small tests whenever possible, regardless of the scope of the test, because it keeps the entire test suite running fast and reliably. For more discussion on small versus unit tests, see Chapter 12.
</div></details>


小测试是三个测试尺寸中最受限制的。主要的约束是小型测试必须在单个流程中运行。在许多语言中，我们甚至进一步限制它，说它们必须在单个线程上运行。这意味着执行测试的代码必须与被测试的代码运行在相同的进程中。您不能在运行服务器的同时让一个单独的测试进程连接到它。这也意味着您不能在测试中运行第三方程序，比如数据库。

对小型测试的另一个重要限制是不允许它们休眠[^5]、执行I/O操作或进行任何其他阻塞调用。这意味着不允许小型测试访问网络或磁盘。依赖于这类操作的测试代码需要使用测试双精度(参见第13章)来用轻量级的进程内依赖替换重量级依赖。

这些限制的目的是确保小型测试不能访问测试缓慢或不确定性的主要来源。如果测试运行在单个进程上，并且从不进行阻塞调用，那么它的运行速度可以达到CPU的处理速度。偶然地使这样的测试变慢或不确定是很困难的(但当然不是不可能)。对小型测试的限制提供了一个沙箱，防止工程师搬起石头砸自己的脚。

这些限制一开始可能看起来有些过分，但是考虑一下一个由几百个小测试用例组成的适度的套件。如果其中的一些测试不确定地失败了(通常称为不稳定的测试)，那么跟踪原因就会严重影响生产效率。在Google的规模下，这样的问题可能会使我们的测试基础设施陷入停顿。

在Google，我们鼓励工程师尽可能编写小的测试，不管测试的范围是什么，因为这样可以保持整个测试套件快速可靠地运行。关于小测试和单元测试的更多讨论，请参见第12章。

#### Medium tests
<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The constraints placed on small tests can be too restrictive for many interesting kinds of tests. The next rung up the ladder of test sizes is the medium test. Medium tests can span multiple processes, use threads, and can make blocking calls, including network calls, to localhost. The only remaining restriction is that medium tests aren’t allowed to make network calls to any system other than localhost. In other words, the test must be contained within a single machine.

The ability to run multiple processes opens up a lot of possibilities. For example, you could run a database instance to validate that the code you’re testing integrates correctly in a more realistic setting. Or you could test a combination of web UI and server code. Tests of web applications often involve tools like WebDriver that start a real browser and control it remotely via the test process.

Unfortunately, with increased flexibility comes increased potential for tests to become slow and nondeterministic. Tests that span processes or are allowed to make blocking calls are dependent on the operating system and third-party processes to be fast and deterministic, which isn’t something we can guarantee in general. Medium tests still provide a bit of protection by preventing access to remote machines via the network, which is far and away the biggest source of slowness and nondeterminism in most systems. Still, when writing medium tests, the “safety” is off, and engineers need to be much more careful.
</div></details>

对于许多有趣的测试来说，放置在小型测试上的约束可能过于严格。测试尺寸的下一个阶梯是中等测试。中等测试可以跨越多个进程，使用线程，并可以对本地主机进行阻塞调用(包括网络调用)。剩下的唯一限制是，不允许中测试对本地主机以外的任何系统进行网络调用。换句话说，测试必须包含在单个机器中。

运行多个流程的能力带来了很多可能性。例如，您可以运行一个数据库实例来验证您正在测试的代码是否在一个更现实的设置中正确集成。或者你可以测试web UI和服务器代码的组合。web应用程序的测试通常涉及WebDriver这样的工具，它们可以启动一个真正的浏览器，并通过测试过程远程控制它。

不幸的是，随着灵活性的增加，测试变得缓慢和不确定性的可能性也增加了。跨越进程或允许进行阻塞调用的测试依赖于操作系统和第三方进程的速度和确定性，这不是我们通常能保证的。通过阻止通过网络访问远程机器，中端测试仍然提供了一些保护，这无疑是大多数系统中缓慢和不确定性的最大根源。然而，当编写中等测试时，“安全”是关闭的，工程师需要更加小心。

#### Large tests

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Finally, we have large tests. Large tests remove the localhost restriction imposed on medium tests, allowing the test and the system being tested to span across multiple machines. For example, the test might run against a system in a remote cluster
</div></details>

最后，我们有大型测试。大型测试消除了对中型测试施加的本地主机限制，允许测试和被测试的系统跨越多台机器。例如，测试可能在远程集群中的系统上运行

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
As before, increased flexibility comes with increased risk. Having to deal with a system that spans multiple machines and the network connecting them increases the chance of slowness and nondeterminism significantly compared to running on a single machine. We mostly reserve large tests for full-system end-to-end tests that are more about validating configuration than pieces of code, and for tests of legacy components for which it is impossible to use test doubles. We’ll talk more about use cases for large tests in Chapter 14. Teams at Google will frequently isolate their large tests from their small or medium tests, running them only during the build and release process so as not to impact developer workflow.
</div></details>

与前面一样，灵活性的提高伴随着风险的增加。与在一台机器上运行相比，必须处理一个跨越多台机器和连接它们的网络的系统大大增加了运行速度慢和不确定性的机会。我们通常将大型测试留给完整系统的端到端测试，这些测试更多的是验证配置，而不是代码片段，以及不可能使用测试双副本的遗留组件的测试。我们将在第14章中更多地讨论大型测试的用例。Google的团队经常将大型测试与小型或中型测试隔离开来，只在构建和发布过程中运行它们，以避免影响开发人员的工作流程。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
A better way to approach the quality of your test suite is to think about the behaviors that are tested. Do you have confidence that everything your customers expect to work will work? Do you feel confident you can catch breaking changes in your dependencies? Are your tests stable and reliable? Questions like these are a more holistic way to think about a test suite. Every product and team is going to be different; some will have difficult-to-test interactions with hardware, some involve massive datasets. Trying to answer the question “do we have enough tests?” with a single number ignores a lot of context and is unlikely to be useful. Code coverage can provide some insight into untested code, but it is not a substitute for thinking critically about how well your system is tested.
</div></details>
了解测试套件质量的更好方法是考虑测试的行为。你有信心客户期望的一切都能成功吗?您有信心捕捉到依赖性中的破坏性更改吗?您的测试是否稳定可靠?像这样的问题是考虑测试套件的更全面的方式。每个产品和团队都是不同的;有些将与硬件进行难以测试的交互，有些涉及大量的数据集。试图回答“我们有足够的测试吗?”，而一个单独的数字会忽略很多上下文，不太可能有用。代码覆盖可以为未测试的代码提供一些见解，但它不能替代批判性地考虑系统的测试情况。


<div style="border:1px #EEE solid;padding:10px;">

<p align="center">案例分析:不可靠的测试是昂贵的 </p>

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
If you have a few thousand tests, each with a very tiny bit of nondeterminism, running all day, occasionally one will probably fail (flake). As the number of tests grows, statistically so will the number of flakes. If each test has even a 0.1% of failing when it should not, and you run 10,000 tests per day, you will be investigating 10 flakes per day. Each investigation takes time away from something more productive that your team could be doing.
</div></details>

如果您有数千个测试，每个测试都有非常小的不确定性，并且运行一整天，偶尔可能会有一个失败(flake)。随着测试数量的增加，薄片的数量在统计上也会增加。如果每个测试在不应该失败的情况下有0.1%的失败，并且您每天运行10,000个测试，那么您将每天调查10片。每一项调查都从你的团队本可以做的更有成效的事情上浪费时间。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In some cases, you can limit the impact of flaky tests by automatically rerunning them when they fail. This is effectively trading CPU cycles for engineering time. At low levels of flakiness, this trade-off makes sense. Just keep in mind that rerunning a test is only delaying the need to address the root cause of flakiness.
</div></details>
在某些情况下，您可以通过在不可靠的测试失败时自动重新运行它们来限制它们的影响。这实际上是用CPU周期来交换工程时间。在低水平的不稳定状态下，这种取舍是有意义的。请记住，重新运行测试只是推迟了解决不稳定根源的需要。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
If test flakiness continues to grow, you will experience something much worse than lost productivity: a loss of confidence in the tests. It doesn’t take needing to investigate many flakes before a team loses trust in the test suite. After that happens, engineers will stop reacting to test failures, eliminating any value the test suite provided. Our experience suggests that as you approach 1% flakiness, the tests begin to lose value. At Google, our flaky rate hovers around 0.15%, which implies thousands of flakes every day. We fight hard to keep flakes in check, including actively investing engineering hours to fix them.
</div></details>
如果测试不稳定持续增长，您将经历比丧失生产力更糟糕的事情:对测试失去信心。在团队失去对测试套件的信任之前，不需要研究很多薄片。在此之后，工程师将停止对测试失败作出反应，从而消除测试套件所提供的任何价值。我们的经验表明，当您接近1%的不稳定时，测试开始失去价值。在Google时，我们的薄片率在0.15%左右，这意味着每天会有数千片薄片。我们努力控制雪花，包括积极投入工程时间来修复它们。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In most cases, flakes appear because of nondeterministic behavior in the tests themselves. Software provides many sources of nondeterminism: clock time, thread scheduling, network latency, and more. Learning how to isolate and stabilize the effects of randomness is not easy. Sometimes, effects are tied to low-level concerns like hardware interrupts or browser rendering engines. A good automated test infrastructure should help engineers identify and mitigate any nondeterministic behavior.
</div></details>
在大多数情况下，薄片的出现是因为测试本身的不确定性行为。软件提供了许多不确定性的来源:时钟时间、线程调度、网络延迟等等。学习如何隔离和稳定随机性的影响并不容易。有时，效果与硬件中断或浏览器呈现引擎等底层问题有关。一个好的自动化测试基础设施应该帮助工程师识别和减少任何不确定性的行为。

</div>

#### Properties common to all test sizes


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
All tests should strive to be hermetic: a test should contain all of the information necessary to set up, execute, and tear down its environment. Tests should assume as little as possible about the outside environment, such as the order in which the tests are run. For example, they should not rely on a shared database. This constraint becomes more challenging with larger tests, but effort should still be made to ensure isolation.

A test should contain only the information required to exercise the behavior in question. Keeping tests clear and simple aids reviewers in verifying that the code does what it says it does. Clear code also aids in diagnosing failure when they fail. We like to say that “a test should be obvious upon inspection.” Because there are no tests for the tests themselves, they require manual review as an important check on correctness. As a corollary to this, we also strongly discourage the use of control flow statements like conditionals and loops in a test. More complex test flows risk containing bugs themselves and make it more difficult to determine the cause of a test failure.

Remember that tests are often revisited only when something breaks. When you are called to fix a broken test that you have never seen before, you will be thankful someone took the time to make it easy to understand. Code is read far more than it is written, so make sure you write the test you’d like to read!

Test sizes in practice. Having precise definitions of test sizes has allowed us to create tools to enforce them. Enforcement enables us to scale our test suites and still make certain guarantees about speed, resource utilization, and stability. The extent to which these definitions are enforced at Google varies by language. For example, we run all Java tests using a custom security manager that will cause all tests tagged as small to fail if they attempt to do something prohibited, such as establish a network connection.
</div></details>
所有的测试都应该是密封的:测试应该包含设置、执行和拆除环境所需的所有信息。测试应该尽可能少地假设外部环境，比如运行测试的顺序。例如，它们不应该依赖于共享数据库。在大型测试中，这个约束变得更具挑战性，但仍然应该努力确保隔离。

测试应该只包含执行相关行为所需的信息。保持测试的清晰和简单有助于检查人员验证代码是否按照它所说的那样运行。清晰的代码还有助于在故障发生时诊断故障。我们喜欢说“一个测试在检查时应该是显而易见的。”因为没有针对测试本身的测试，所以它们需要手工检查作为对正确性的重要检查。因此，我们也强烈反对在测试中使用像条件和循环这样的控制流语句。更复杂的测试流本身就有包含bug的风险，并且使得确定测试失败的原因变得更加困难。

请记住，只有当某些东西发生故障时，测试才会被重新访问。当你被要求修复一个你从未见过的坏测试时，你会感谢有人花时间让它变得容易理解。阅读代码的次数远远多于编写代码的次数，所以请确保编写的是您想要阅读的测试!

**实际测试尺寸**。有了测试大小的精确定义，我们就可以创建工具来执行它们。强制使我们能够扩展我们的测试套件，并且仍然保证了速度、资源利用和稳定性。在Google中强制执行这些定义的程度因语言而异。例如，我们使用自定义安全管理器运行所有Java测试，如果标记为小的测试试图做一些被禁止的事情(比如建立网络连接)，就会导致所有测试失败。

### 测试范围 The Scope

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Though we at Google put a lot of emphasis on test size, another important property to consider is test scope. Test scope refers to how much code is being validated by a given test. Narrow-scoped tests (commonly called “unit tests”) are designed to validate the logic in a small, focused part of the codebase, like an individual class or method. Medium-scoped tests (commonly called integration tests) are designed to verify interactions between a small number of components; for example, between a server and its database. Large-scoped tests (commonly referred to by names like functional tests, end-to-end tests, or system tests) are designed to validate the interaction of several distinct parts of the system, or emergent behaviors that aren’t expressed in a single class or method.
</div></details>

虽然我们在Google中非常强调测试大小，但另一个需要考虑的重要属性是测试范围。测试范围指的是给定的测试要验证多少代码。窄范围测试(通常称为“单元测试”)的设计目的是验证代码库中一小部分集中的逻辑，比如单个类或方法。中等范围的测试(通常称为集成测试)旨在验证少量组件之间的交互;例如，在服务器与其数据库之间。大范围测试(通常称为功能测试、端到端测试或系统测试)的设计是为了验证系统中几个不同部分的交互，或者不是在单个类或方法中表示的紧急行为。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
It’s important to note that when we talk about unit tests as being narrowly scoped, we’re referring to the code that is being validated, not the code that is being executed. It’s quite common for a class to have many dependencies or other classes it refers to, and these dependencies will naturally be invoked while testing the target class. Though some other testing strategies make heavy use of test doubles (fakes or mocks) to avoid executing code outside of the system under test, at Google, we prefer to keep the real dependencies in place when it is feasible to do so. Chapter 13 discusses this issue in more detail.
</div></details>
值得注意的是，当我们谈到单元测试的范围很窄时，我们指的是正在验证的代码，而不是正在执行的代码。类有许多依赖项或它引用的其他类是很常见的，这些依赖项在测试目标类时自然会被调用。尽管其他一些测试策略大量使用测试双倍(假冒或模拟)来避免在测试系统之外执行代码，但在Google中，我们倾向于在可行的情况下保留真正的依赖项。第13章更详细地讨论了这个问题。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Narrow-scoped tests tend to be small, and broad-scoped tests tend to be medium or large, but this isn’t always the case. For example, it’s possible to write a broad-scoped test of a server endpoint that covers all of its normal parsing, request validation, and business logic, which is nevertheless small because it uses doubles to stand in for all out-of-process dependencies like a database or filesystem. Similarly, it’s possible to write a narrow-scoped test of a single method that must be medium sized. For example, modern web frameworks often bundle HTML and JavaScript together, and testing a UI component like a date picker often requires running an entire browser, even to validate a single code path.
</div></details>
窄范围的测试往往是小型的，而宽范围的测试往往是中型或大型的，但情况并非总是如此。例如，可以编写一个服务器端点的范围广泛的测试，涵盖它的所有常规解析、请求验证和业务逻辑，但是这个测试很小，因为它使用double来表示所有进程外的依赖关系，比如数据库或文件系统。类似地，可以编写一个必须是中等大小的单一方法的窄范围测试。例如，现代的web框架经常将HTML和JavaScript捆绑在一起，测试像日期选择器这样的UI组件通常需要运行整个浏览器，甚至验证单个代码路径。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Just as we encourage tests of smaller size, at Google, we also encourage engineers to write tests of narrower scope. As a very rough guideline, we tend to aim to have a mix of around 80% of our tests being narrow-scoped unit tests that validate the majority of our business logic; 15% medium-scoped integration tests that validate the interactions between two or more components; and 5% end-to-end tests that validate the entire system. Figure 11-3 depicts how we can visualize this as a pyramid.
</div></details>
正如我们鼓励更小规模的测试一样，在Google，我们也鼓励工程师编写更小范围的测试。作为一个非常粗略的指导方针，我们倾向于将80%的测试混合为窄范围的单元测试，以验证我们的大部分业务逻辑;15%中等范围的集成测试，验证两个或多个组件之间的交互;5%的端到端测试验证整个系统。图11-3描述了我们如何将其可视化为一个金字塔。

<img src='./pic/fingure-11.3.png' width='100%'>[^6]


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Unit tests form an excellent base because they are fast, stable, and dramatically narrow the scope and reduce the cognitive load required to identify all the possible behaviors a class or function has. Additionally, they make failure diagnosis quick and painless. Two antipatterns to be aware of are the “ice cream cone” and the “hourglass,” as illustrated in Figure 11-4.
</div></details>
单元测试形成了一个优秀的基础，因为它们快速、稳定，并且极大地缩小了范围，减少了识别类或函数的所有可能行为所需的认知负荷。此外，它们使故障诊断快速和无痛。需要注意的两个反模式是“冰淇淋蛋卷”和“沙漏”，如图11-4所示。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
With the ice cream cone, engineers write many end-to-end tests but few integration or unit tests. Such suites tend to be slow, unreliable, and difficult to work with. This pattern often appears in projects that start as prototypes and are quickly rushed to production, never stopping to address testing debt.
</div></details>
使用冰淇淋甜筒，工程师编写了许多端到端测试，但很少编写集成或单元测试。这样的套件往往很慢、不可靠，而且难以使用。这种模式经常出现在一些项目中，这些项目开始时是原型，然后迅速投入生产，从不停下来处理测试债务的问题。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The hourglass involves many end-to-end tests and many unit tests but few integration tests. It isn’t quite as bad as the ice cream cone, but it still results in many end-to-end test failures that could have been caught quicker and more easily with a suite of medium-scope tests. The hourglass pattern occurs when tight coupling makes it difficult to instantiate individual dependencies in isolation.
</div></details>
沙漏包括许多端到端测试和许多单元测试，但集成测试很少。这并不像冰淇淋甜筒那样糟糕，但它仍然会导致许多端到端测试失败，这些失败本可以通过一套中等范围的测试更快更容易地发现。当紧密耦合使得很难单独实例化单个依赖时，就会出现沙漏模式。

<img src='./pic/fingure-11.4.png' width='100%'>

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Our recommended mix of tests is determined by our two primary goals: engineering productivity and product confidence. Favoring unit tests gives us high confidence quickly, and early in the development process. Larger tests act as sanity checks as the product develops; they should not be viewed as a primary method for catching bugs.
</div></details>

我们推荐的测试组合由我们的两个主要目标决定:工程生产率和产品信心。支持单元测试可以在开发过程的早期快速地给予我们高度的信心。较大的测试在产品开发时充当完整性检查;它们不应该被视为捕获bug的主要方法。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
When considering your own mix, you might want a different balance. If you emphasize integration testing, you might discover that your test suites take longer to run but catch more issues between components. When you emphasize unit tests, your test suites can complete very quickly, and you will catch many common logic bugs. But, unit tests cannot verify the interactions between components, like a contract between two systems developed by different teams. A good test suite contains a blend of different test sizes and scopes that are appropriate to the local architectural and organizational realities.
</div></details>
当考虑你自己的组合时，你可能想要一个不同的平衡。如果您强调集成测试，您可能会发现您的测试套件运行时间更长，但在组件之间捕获更多的问题。当您强调单元测试时，您的测试套件可以很快完成，并且您将捕获许多常见的逻辑错误。但是，单元测试不能验证组件之间的交互，就像不同团队开发的两个系统之间的契约一样。一个好的测试套件包含不同的测试规模和范围的混合，它们适合于当地的架构和组织的实际情况。

### Beyoncé规则 The Beyoncé Rule

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
We are often asked, when coaching new hires, which behaviors or properties actually need to be tested? The straightforward answer is: test everything that you don’t want to break. In other words, if you want to be confident that a system exhibits a particular behavior, the only way to be sure it will is to write an automated test for it. This includes all of the usual suspects like testing performance, behavioral correctness, accessibility, and security. It also includes less obvious properties like testing how a system handles failure.
</div></details>
我们经常被问到，在指导新员工时，哪些行为或特性需要测试?简单的答案是:测试您不想破坏的所有内容。换句话说，如果您想确信一个系统展示了一个特定的行为，唯一的方法就是为它编写一个自动化的测试。这包括所有常见的问题，如测试性能、行为正确性、可访问性和安全性。它还包括一些不太明显的属性，比如测试系统如何处理故障。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
We have a name for this general philosophy: we call it the Beyoncé Rule. Succinctly, it can be stated as follows: “If you liked it, then you shoulda put a test on it.” The Beyoncé Rule is often invoked by infrastructure teams that are responsible for making changes across the entire codebase. If unrelated infrastructure changes pass all of your tests but still break your team’s product, you are on the hook for fixing it and adding the additional tests.
</div></details>
我们为这一普遍哲学取了个名字:我们称之为Beyoncé规则。简单地说，可以这样说:“如果你喜欢它，那么你应该对它进行测试。”Beyoncé规则通常由负责跨整个代码库进行更改的基础架构团队调用。如果不相关的基础架构更改通过了所有的测试，但仍然破坏了团队的产品，则需要修复它并添加额外的测试。


<div style="border:1px #EEE solid;padding:10px;">

**<p align="center">测试失败 Testing for Failure</p>**

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
One of the most important situations a system must account for is failure. Failure is inevitable, but waiting for an actual catastrophe to find out how well a system responds to a catastrophe is a recipe for pain. Instead of waiting for a failure, write automated tests that simulate common kinds of failures. This includes simulating exceptions or errors in unit tests and injecting Remote Procedure Call (RPC) errors or latency in integration and end-to-end tests. It can also include much larger disruptions that affect the real production network using techniques like Chaos Engineering. A predictable and controlled response to adverse conditions is a hallmark of a reliable system.
</div></details>

系统必须考虑的最重要的情况之一是故障。失败是不可避免的，但等待真正的灾难来发现系统对灾难的反应有多好，这是痛苦的处方。与其等待失败，不如编写模拟常见失败类型的自动化测试。这包括在单元测试中模拟异常或错误，以及在集成和端到端测试中注入远程过程调用(RPC)错误或延迟。它还可以包括使用混沌工程等技术影响实际生产网络的更大的中断。对不利条件的可预测和可控制的反应是可靠系统的标志。
</div>

### 关于代码覆盖的说明 A Note on Code Coverage


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Code coverage is a measure of which lines of feature code are exercised by which tests. If you have 100 lines of code and your tests execute 90 of them, you have 90% code coverage.Code coverage is often held up as the gold standard metric for understanding test quality, and that is somewhat unfortunate. It is possible to exercise a lot of lines of code with a few tests, never checking that each line is doing anything useful. That’s because code coverage only measures that a line was invoked, not what happened as a result. (We recommend only measuring coverage from small tests to avoid coverage inflation that occurs when executing larger tests.)

An even more insidious problem with code coverage is that, like other metrics, it quickly becomes a goal unto itself. It is common for teams to establish a bar for expected code coverage—for instance, 80%. At first, that sounds eminently reasonable; surely you want to have at least that much coverage. In practice, what happens is that instead of treating 80% like a floor, engineers treat it like a ceiling. Soon, changes begin landing with no more than 80% coverage. After all, why do more work than the metric requires?
</div></details>

代码覆盖率是对哪些特性代码行由哪些测试执行的度量[^7]。如果您有100行代码，并且您的测试执行了其中的90行，那么您就有90%的代码覆盖率。代码覆盖率经常被认为是理解测试质量的黄金标准，这有点不幸。用几个测试来测试大量的代码行是有可能的，而从来不检查每一行是否做了什么有用的事情。这是因为代码覆盖率只度量调用的行，而不是结果。(我们建议只度量小测试的覆盖率，以避免执行大测试时出现的覆盖率膨胀。)

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
An even more insidious problem with code coverage is that, like other metrics, it quickly becomes a goal unto itself. It is common for teams to establish a bar for expected code coverage—for instance, 80%. At first, that sounds eminently reasonable; surely you want to have at least that much coverage. In practice, what happens is that instead of treating 80% like a floor, engineers treat it like a ceiling. Soon, changes begin landing with no more than 80% coverage. After all, why do more work than the metric requires?
</div></details>

代码覆盖的一个更隐蔽的问题是，像其他度量一样，它很快就成为了自己的目标。对于团队来说，为预期的代码覆盖率建立一个标准是很常见的——例如，80%。起初，这听起来非常合理;你肯定希望至少有那么多的报道。实际上，工程师们把80%当成了天花板，而不是地板。不久，改变开始着陆不超过80%的覆盖。毕竟，为什么要做比标准要求更多的工作呢?

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
A better way to approach the quality of your test suite is to think about the behaviors that are tested. Do you have confidence that everything your customers expect to work will work? Do you feel confident you can catch breaking changes in your dependencies? Are your tests stable and reliable? Questions like these are a more holistic way to think about a test suite. Every product and team is going to be different; some will have difficult-to-test interactions with hardware, some involve massive datasets. Trying to answer the question “do we have enough tests?” with a single number ignores a lot of context and is unlikely to be useful. Code coverage can provide some insight into untested code, but it is not a substitute for thinking critically about how well your system is tested.
</div></details>
了解测试套件质量的更好方法是考虑测试的行为。你有信心客户期望的一切都能成功吗?您有信心捕捉到依赖性中的破坏性更改吗?您的测试是否稳定可靠?像这样的问题是考虑测试套件的更全面的方式。每个产品和团队都是不同的;有些将与硬件进行难以测试的交互，有些涉及大量的数据集。试图回答“我们有足够的测试吗?”，而一个单独的数字会忽略很多上下文，不太可能有用。代码覆盖可以为未测试的代码提供一些见解，但它不能替代批判性地考虑系统的测试情况。




[^4]:Technically, we have four sizes of test at Google: small, medium, large, and enormous. The internal difference between large and enormous is actually subtle and historical; so, in this book, most descriptions of large actually apply to our notion of enormous.
[^5]:There is a little wiggle room in this policy. Tests are allowed to access a filesystem if they use a hermetic, inmemory implementation.
[^6]:Mike Cohn, Succeeding with Agile: Sofeware Development Using Scrum (New York:Addison-Wesley Professional, 2009).
[^7]:Keep in mind that there are different kinds of coverage (line, path, branch, etc.), and each says something different about which code has been tested. In this simple example, line coverage is being used.


