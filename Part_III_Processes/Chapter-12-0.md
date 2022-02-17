
**<p align="right">第12章</p>**
-- -
**<p align="right">单元测试 Unit Testing</p>**

*<p align="right">作者：  Erik Kueer</p>*
*<p align="right">编辑： Tom Manshreck</p>*


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

The previous chapter introduced two of the main axes along which Google classifies tests: size and scope. To recap, size refers to the resources consumed by a test and what it is allowed to do, and scope refers to how much code a test is intended to validate. Though Google has clear definitions for test size, scope tends to be a little fuzzier. We use the term unit test to refer to tests of relatively narrow scope, such as of a single class or method. Unit tests are usually small in size, but this isn’t always the case.

After preventing bugs, the most important purpose of a test is to improve engineers’ productivity. Compared to broader-scoped tests, unit tests have many properties that make them an excellent way to optimize productivity:
</div></details>

前一章介绍了谷歌对测试进行分类的两个主要轴:大小和范围。概括一下，大小指的是测试所消耗的资源以及它所允许做的事情，而范围指的是测试要验证的代码数量。虽然谷歌对测试大小有明确的定义，但范围往往有点模糊。我们使用术语单元测试来指代相对狭窄范围的测试，比如单个类或方法的测试。单元测试通常规模较小，但情况并非总是如此。

在防止bug之后，测试最重要的目的是提高工程师的生产力。与范围更广的测试相比，单元测试有许多特性，这些特性使其成为优化生产力的绝佳方式:


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

- They tend to be small according to Google’s definitions of test size. Small tests are fast and deterministic, allowing developers to run them frequently as part of their workflow and get immediate feedback.
 - They tend to be easy to write at the same time as the code they’re testing, allow‐ ing engineers to focus their tests on the code they’re working on without having to set up and understand a larger system. 
- They promote high levels of test coverage because they are quick and easy to write. High test coverage allows engineers to make changes with confidence that they aren’t breaking anything. 
- They tend to make it easy to understand what’s wrong when they fail because each test is conceptually simple and focused on a particular part of the system. 
- They can serve as documentation and examples, showing engineers how to use the part of the system being tested and how that system is intended to work.
</div></details>

- 根据谷歌对测试大小的定义，它们倾向于较小。小测试是快速和确定的，允许开发人员经常运行它们作为工作流的一部分，并获得即时反馈。
- 它们易于在测试代码的同时编写，允许工程师将测试的重点放在他们正在工作的代码上，而不必建立和理解一个更大的系统。
- 它们促进了高水平的测试覆盖，因为它们快速且容易编写。高测试覆盖率使工程师能够自信地做出更改，确信他们没有破坏任何东西。
- 他们倾向于在失败时让玩家更容易理解哪里出了问题，因为每个测试概念上都很简单，并且只关注系统的特定部分。
- 它们可以作为文档和示例，向工程师展示如何使用被测试系统的部分，以及该系统如何工作。



<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Due to their many advantages, most tests written at Google are unit tests, and as a rule of thumb, we encourage engineers to aim for a mix of about 80% unit tests and 20% broader-scoped tests. This advice, coupled with the ease of writing unit tests and the speed with which they run, means that engineers run a lot of unit tests—it’s not at all unusual for an engineer to execute thousands of unit tests (directly or indirectly) during the average workday.
</div></details>

由于谷歌的许多优势，大多数在谷歌上编写的测试都是单元测试，根据经验，我们鼓励工程师将目标设定为80%的单元测试和20%的宽范围测试的组合。这个建议，加上编写单元测试的便便性和运行的速度，意味着工程师要运行大量的单元测试——对于一个工程师来说，在平均工作日中执行数千个单元测试(直接或间接)是很常见的。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Because they make up such a big part of engineers’ lives, Google puts a lot of focus on test maintainability. Maintainable tests are ones that “just work”: after writing them, engineers don’t need to think about them again until they fail, and those failures indicate real bugs with clear causes. The bulk of this chapter focuses on exploring the idea of maintainability and techniques for achieving it.
</div></details>
由于它们在工程师的生活中占据了如此大的一部分，谷歌将大量精力放在测试的可维护性上。可维护的测试是那些“只是工作”的测试:在编写它们之后，工程师不需要再考虑它们，直到它们失败，而这些失败表明了真正的bug，并且有明确的原因。本章的主要内容是探讨可维护性的概念和实现可维护性的技术。