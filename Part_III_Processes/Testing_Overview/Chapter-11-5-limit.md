## 自动化测试的局限性 The Limits of Automated Testing

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Automated testing is not suitable for all testing tasks. For example, testing the quality of search results often involves human judgment. We conduct targeted, internal studies using Search Quality Raters who execute real queries and record their impressions. Similarly, it is difficult to capture the nuances of audio and video quality in an automated test, so we often use human judgment to evaluate the performance of telephony or video-calling systems.
</div></details>

自动化测试并不适合所有的测试任务。例如，测试搜索结果的质量通常涉及到人类的判断。我们使用搜索质量评分者进行有针对性的内部研究，他们执行真实的查询并记录他们的印象。类似地，在自动化测试中很难捕捉到音频和视频质量的细微差别，因此我们经常使用人类的判断来评估电话或视频呼叫系统的性能。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In addition to qualitative judgements, there are certain creative assessments at which humans excel. For example, searching for complex security vulnerabilities is something that humans do better than automated systems. After a human has discovered and understood a flaw, it can be added to an automated security testing system like Google’s Cloud Security Scanner where it can be run continuously and at scale.
</div></details>
除了定性判断之外，人类还擅长某些创造性的评估。例如，搜索复杂的安全漏洞是人类比自动化系统做得更好的事情。当人们发现并理解了一个缺陷后，就可以将其添加到自动安全测试系统中，比如Google的云安全扫描仪(Cloud security Scanner)，该系统可以连续且大规模地运行。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
A more generalized term for this technique is Exploratory Testing. Exploratory Testing is a fundamentally creative endeavor in which someone treats the application under test as a puzzle to be broken, maybe by executing an unexpected set of steps or by inserting unexpected data. When conducting an exploratory test, the specific problems to be found are unknown at the start. They are gradually uncovered by probing commonly overlooked code paths or unusual responses from the application. As with the detection of security vulnerabilities, as soon as an exploratory test discovers an issue, an automated test should be added to prevent future regressions.
</div></details>
这种技术的一个更一般化的术语是探索性测试。探索性测试是一种根本性的创造性尝试，在这种尝试中，有人将被测试的应用程序视为一个有待破解的谜题，可能是执行一组意想不到的步骤，或者插入意想不到的数据。在进行探索性测试时，要发现的特定问题在一开始是未知的。通过探测应用程序中经常被忽略的代码路径或异常响应，它们会逐渐被发现。与安全漏洞的检测一样，只要探索性测试发现问题，就应该添加自动化测试，以防止将来出现倒退。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Using automated testing to cover well-understood behaviors enables the expensive and qualitative efforts of human testers to focus on the parts of your products for which they can provide the most value—and avoid boring them to tears in the process.
</div></details>

使用自动化测试来覆盖易于理解的行为，可以使人类测试人员花费昂贵和定性的努力，集中在产品中他们可以提供最大价值的部分，并避免在过程中让他们感到无聊。


## 结论 Conclusion

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The adoption of developer-driven automated testing has been one of the most transformational software engineering practices at Google. It has enabled us to build larger systems with larger teams, faster than we ever thought possible. It has helped us keep up with the increasing pace of technological change. Over the past 15 years, we have successfully transformed our engineering culture to elevate testing into a cultural norm. Despite the company growing by a factor of almost 100 times since the journey began, our commitment to quality and testing is stronger today than it has ever been.
</div></details>

采用开发人员驱动的自动化测试已经成为Google最具变革性的软件工程实践之一。它使我们能够用更大的团队构建更大的系统，速度比我们想象的要快。它帮助我们跟上技术变革的步伐。在过去的15年里，我们已经成功地将我们的工程文化转变为一种文化规范来提升测试。尽管公司自成立以来增长了近100倍，但我们对质量和测试的承诺比以往任何时候都更加坚定。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
This chapter has been written to help orient you to how Google thinks about testing. In the next few chapters, we are going to dive even deeper into some key topics that have helped shape our understanding of what it means to write good, stable, and reliable tests. We will discuss the what, why, and how of unit tests, the most common kind of test at Google. We will wade into the debate on how to effectively use test doubles in tests through techniques such as faking, stubbing, and interaction testing. Finally, we will discuss the challenges with testing larger and more complex systems, like many of those we have at Google.

</div></details>

本章的目的是帮助您了解Google如何看待测试。在接下来的几章中，我们将更深入地探讨一些关键的主题，这些主题帮助我们理解编写好的、稳定的和可靠的测试意味着什么。我们将讨论单元测试的内容、原因和方法，单元测试是Google中最常见的测试类型。我们将深入讨论如何通过诸如伪造、存根和交互测试等技术有效地在测试中使用测试替身。最后，我们将讨论测试更大、更复杂的系统所面临的挑战，就像我们在Google所遇到的许多系统一样。

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
At the conclusion of these three chapters, you should have a much deeper and clearer picture of the testing strategies we use and, more important, why we use them.
</div></details>

在这三章的结尾，您应该对我们使用的测试策略有一个更深入和更清晰的了解，更重要的是，我们为什么要使用它们。


## 其他 TL;DRs

<details> <summary>英文原文</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

- Automated testing is foundational to enabling software to change.
- For tests to scale, they must be automated.
- A balanced test suite is necessary for maintaining healthy test coverage.
- “If you liked it, you should have put a test on it.”
- Changing the testing culture in organizations takes time.
</div></details>

- 自动化测试是使软件能够改变的基础。
- 对于可扩展的测试，它们必须是自动化的。
- 一个平衡的测试套件对于保持健康的测试覆盖是必要的。
- “如果你喜欢它，你应该给它做个测试。”
- 改变组织中的测试文化需要时间。