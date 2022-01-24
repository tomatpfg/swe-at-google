**<p align="right">第11章</p>**  
-- -  
**<p align="right">测试概述（Testing overview）</p>**  

*<p align="right">作者： Adam Bender</p>* 
*<p align="right">编辑： Tom Manshreck</p>*

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Testing has always been a part of programming. In fact, the first time you wrote a computer program you almost certainly threw some sample data at it to see whether it performed as you expected. For a long time, the state of the art in software testing resembled a very similar process, largely manual and error prone. However, since the early 2000s, the software industry’s approach to testing has evolved dramatically to cope with the size and complexity of modern software systems. Central to that evolution has been the practice of developer-driven, automated testing.
</div></details>

测试一直是编程的一部分。事实上，当您第一次编写计算机程序时，您几乎肯定会向它抛出一些样本数据，看看它是否按照您的预期运行。在很长一段时间里，软件测试的技术状态类似于一个非常相似的过程，大部分是手工的，并且很容易出错。然而，自21世纪初以来，软件行业的测试方法已经发生了巨大的变化，以应对现代软件系统的规模和复杂性。这种发展的核心是开发人员驱动的自动化测试的实践。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Automated testing can prevent bugs from escaping into the wild and affecting your users. The later in the development cycle a bug is caught, the more expensive it is; exponentially so in many cases.However, “catching bugs” is only part of the motivation. An equally important reason why you want to test your software is to support the ability to change. Whether you’re adding new features, doing a refactoring focused on code health, or undertaking a larger redesign, automated testing can quickly catch mistakes, and this makes it possible to change software with confidence.
</div></details>

自动化测试可以防止错误泛滥并影响您的用户。在开发周期中，越晚发现漏洞，代价就越高;在许多情况下，指数上是如此然而，“捕捉bug”只是动机的一部分。测试软件的一个同样重要的原因是为了支持更改的能力。[^1]无论您是在添加新特性、进行代码健康度重构，还是进行更大规模的重新设计，自动化测试都可以快速地捕获错误，这使得您可以满怀信心地更改软件。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Companies that can iterate faster can adapt more rapidly to changing technologies, market conditions, and customer tastes. If you have a robust testing practice, you needn’t fear change—you can embrace it as an essential quality of developing software. The more and faster you want to change your systems, the more you need a fast way to test them.
</div></details>
能够更快地进行迭代的公司能够更快地适应不断变化的技术、市场条件和客户口味。如果您有一个健壮的测试实践，您就不必害怕变更——您可以将其作为开发软件的基本质量来接受。您想要更改系统的次数越多、速度越快，就越需要一种快速的方法来测试它们。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The act of writing tests also improves the design of your systems. As the first clients of your code, a test can tell you much about your design choices. Is your system too tightly coupled to a database? Does the API support the required use cases? Does your system handle all of the edge cases? Writing automated tests forces you to confront these issues early on in the development cycle. Doing so generally leads to more modular software that enables greater flexibility later on.
</div></details>
编写测试的行为还可以改进系统的设计。作为代码的第一个客户端，测试可以告诉您很多关于设计选择的信息。您的系统是否与数据库耦合得太紧?API是否支持所需的用例?你的系统能处理所有的边缘情况吗?编写自动化测试迫使您在开发周期的早期就面对这些问题。这样做通常会导致更模块化的软件，从而在以后实现更大的灵活性。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Much ink has been spilled about the subject of testing software, and for good reason: for such an important practice, doing it well still seems to be a mysterious craft to many. At Google, while we have come a long way, we still face difficult problems getting our processes to scale reliably across the company. In this chapter, we’ll share what we have learned to help further the conversation.
</div></details>
关于软件测试这个话题已经有了很多的讨论，而且有充分的理由:对于这样一个重要的实践，做好它对许多人来说仍然是一件神秘的事情。在Google，虽然我们已经取得了很大的进展，但我们仍然面临着困难的问题，即如何让我们的流程在整个公司范围内可靠地扩展。在本章中，我们将分享我们所学到的知识，以帮助进一步对话。


[^1]:See “[Defect Prevention: Reducing Costs and Enhancing Quality.](https://oreil.ly/27R87)”
