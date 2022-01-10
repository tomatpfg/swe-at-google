## 设计测试套件 Designing a Test Suite
<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Today, Google operates at a massive scale, but we haven’t always been so large, and the foundations of our approach were laid long ago. Over the years, as our codebase has grown, we have learned a lot about how to approach the design and execution of a test suite, often by making mistakes and cleaning up afterward.
</div></details>

今天，谷歌的运营规模很大，但我们并不总是那么大，我们的方法的基础很久以前就奠定了。多年来，随着我们代码库的增长，我们学到了很多关于如何设计和执行测试套件的知识，通常是通过犯错和事后清理。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
One of the lessons we learned fairly early on is that engineers favored writing larger, system-scale tests, but that these tests were slower, less reliable, and more difficult to debug than smaller tests. Engineers, fed up with debugging the system-scale tests,asked themselves, “Why can’t we just test one server at a time?” or, “Why do we need to test a whole server at once? We could test smaller modules individually.” Eventually, the desire to reduce pain led teams to develop smaller and smaller tests, which turned out to be faster, more stable, and generally less painful.
</div></details>

我们很早就学到的教训之一是工程师喜欢编写更大的系统规模测试，但这些测试比小型测试更慢、更不可靠且更难调试。工程师们厌倦了调试系统规模的测试，他们问自己：“为什么我们不能一次只测试一台服务器？”或者，“为什么我们需要一次测试整个服务器？我们可以单独测试较小的模块。”最终，减轻疼痛的愿望促使团队开发越来越小的测试，结果证明这些测试更快、更稳定，而且通常疼痛更小。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
This led to a lot of discussion around the company about the exact meaning of “small.” Does small mean unit test? What about integration tests, what size are those? We have come to the conclusion that there are two distinct dimensions for every test case: size and scope. Size refers to the resources that are required to run a test case: things like memory, processes, and time. Scope refers to the specific code paths we are verifying. Note that executing a line of code is different from verifying that it worked as expected. Size and scope are interrelated but distinct concepts.
</div></details>

这在公司内部引发了很多关于“小”的确切含义的讨论。小意味着单元测试吗？那么集成测试呢，它们的大小是多少？我们得出的结论是，每个测试用例都有两个不同的维度：大小和范围。大小是指运行测试用例所需的资源：例如内存、进程和时间。范围是指我们正在验证的特定代码路径。请注意，执行一行代码不同于验证它是否按预期工作。规模和范围是相互关联但又截然不同的概念。
