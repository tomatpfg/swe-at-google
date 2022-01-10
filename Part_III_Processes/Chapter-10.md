**<p align="right">第10章</p>**  
-- -  
**<p align="right">文档（Documentation）</p>**  

*<p align="right">作者： Tom Manshreck</p>* 
*<p align="right">编辑： Riona MacNamara</p>*


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Of the complaints most engineers have about writing, using, and maintaining code, a singular common frustration is the lack of quality documentation. “What are the side effects of this method?” “I got an error after step 3.” “What does this acronym mean?” “Is this document up to date?” Every software engineer has voiced complaints about the quality, quantity, or sheer lack of documentation throughout their career, and the software engineers at Google are no different.
</div></details>

在大多数工程师对编写、使用和维护代码的抱怨中，最常见的是缺乏高质量的文档。“这种方法有什么副作用?”“我在步骤3后得到一个错误。”“这个缩略词是什么意思?”“这份文件是最新的吗?”每个软件工程师在他们的职业生涯中都曾抱怨过文档的质量、数量或纯粹的缺乏，谷歌的软件工程师也不例外。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Technical writers and project managers may help, but software engineers will always need to write most documentation themselves. Engineers, therefore, need the proper tools and incentives to do so effectively. The key to making it easier for them to write quality documentation is to introduce processes and tools that scale with the organization and that tie into their existing workflow.
</div></details>

技术作家和项目经理可能会有所帮助，但软件工程师总是需要自己编写大部分文档。因此，工程师需要适当的工具和激励来有效地做到这一点。使他们更容易编写高质量文档的关键是引入与组织相适应的过程和工具，并将其与他们现有的工作流程相结合。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Overall, the state of engineering documentation in the late 2010s is similar to the state of software testing in the late 1980s. Everyone recognizes that more effort needs to be made to improve it, but there is not yet organizational recognition of its critical benefits. That is changing, if slowly. At Google, our most successful efforts have been when documentation is treated like code and incorporated into the traditional engineering workflow, making it easier for engineers to write and maintain simple documents.
</div></details>

总的来说，2010年代后期的工程文档状态类似于1980年代后期的软件测试状态。每个人都认识到需要做出更多的努力来改进它，但是组织还没有认识到它的关键好处。这种情况正在改变，尽管速度缓慢。在谷歌，我们最成功的努力是将文档视为代码，并将其整合到传统的工程工作流程中，使工程师更容易编写和维护简单的文档。

## 什么是文档？（What Qualifies as Documentation?）

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
When we refer to “documentation,” we’re talking about every supplemental text that an engineer needs to write to do their job: not only standalone documents, but code comments as well. (In fact, most of the documentation an engineer at Google writes comes in the form of code comments.) We’ll discuss the various types of engineering documents further in this chapter.
</div></details>

当我们提到“文档”时，我们指的是工程师为完成工作而需要编写的所有补充文本：不仅是独立文档，还有代码注释。 （实际上，Google 工程师编写的大部分文档都是以代码注释的形式出现的。）我们将在本章中进一步讨论各种类型的工程文档。

## 为什么需要文档？（Why Is Documentation Needed?）


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Quality documentation has tremendous benefits for an engineering organization. Code and APIs become more comprehensible, reducing mistakes. Project teams are more focused when their design goals and team objectives are clearly stated. Manual processes are easier to follow when the steps are clearly outlined. Onboarding new members to a team or code base takes much less effort if the process is clearly documented.
</div></details>

质量文档对工程组织有巨大的好处。代码和api变得更易于理解，从而减少了错误。当项目团队的设计目标和团队目标被明确表述时，他们会更加专注。当步骤被清楚地列出时，手动流程更容易遵循。如果过程被清楚地记录下来，那么让新成员加入团队或代码库将会花费更少的精力。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
But because documentation’s benefits are all necessarily downstream, they generally don’t reap immediate benefits to the author. Unlike testing, which (as we’ll see) quickly provides benefits to a programmer, documentation generally requires more effort up front and doesn’t provide clear benefits to an author until later. But, like investments in testing, the investment made in documentation will pay for itself over time. After all, you might write a document only once, but it will be read hundreds, perhaps thousands of times afterward; its initial cost is amortized across all the future readers. Not only does documentation scale over time, but it is critical for the rest of the organization to scale as well. It helps answer questions like these:
- Why were these design decisions made? 
- Why did we implement this code in this manner? 
- Why did I implement this code in this manner, if you’re looking at your own code two years later?

</div></details>

但是，由于文档的好处都必然是下游的，它们通常不会给作者带来直接的好处。与测试不同(正如我们将看到的那样)，测试可以快速地为程序员提供好处，而文档通常需要提前付出更多的努力，直到后来才能为作者提供明显的好处。但是，就像在测试上的投资一样，在文档上的投资会随着时间的推移而得到回报。毕竟，你可能只写了一次文档[^1]，但是之后它会被读几百次，甚至上千次;它的初始成本分摊给所有未来的读取器。文档不仅可以随着时间的推移而扩展，而且对于组织的其他部分来说，扩展也是至关重要的。它有助于回答以下问题:
- 为什么要做出这些设计决策?
- 为什么我们以这种方式实现这段代码?
- 为什么我要用这种方式实现这段代码，如果你在两年后看到你自己的代码?


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
If documentation conveys all these benefits, why is it generally considered “poor” by engineers? One reason, as we’ve mentioned, is that the benefits aren’t immediate, especially to the writer. But there are several other reasons:

- Engineers often view writing as a separate skill than that of programming. (We’ll try to illustrate that this isn’t quite the case, and even where it is, it isn’t necessarily a separate skill from that of software engineering.) 
- Some engineers don’t feel like they are capable writers. But you don’t need a robust command of English to produce workable documentation. You just need to step outside yourself a bit and see things from the audience’s perspective.
- Writing documentation is often more difficult because of limited tools support or integration into the developer workflow. 
- Documentation is viewed as an extra burden—something else to maintain— rather than something that will make maintenance of their existing code easier.
</div></details>

如果文档传达了所有这些好处，为什么工程师通常认为它很“差”?其中一个原因，正如我们提到的，是好处不是立竿见影的，尤其是对作者来说。但还有其他几个原因:
- 工程师经常将写作视为一项独立的技能，而不是编程。(我们将试图说明事实并非如此，即使是在这种情况下，它也不一定是一种与软件工程相分离的技能。)
- 有些工程师不觉得自己是有能力的作家。但是，您不需要精通英语[^2]来生成可操作的文档。你只需要稍微跳出自己的圈子，站在观众的角度看问题。
- 编写文档通常更困难，因为有限的工具支持或集成到开发人员的工作流程中。
- 文档被视为一种额外的负担——需要维护的东西——而不是使现有代码的维护更容易的东西。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Not every engineering team needs a technical writer (and even if that were the case, there aren’t enough of them). This means that engineers will, by and large, write most of the documentation themselves. So, instead of forcing engineers to become technical writers, we should instead think about how to make writing documentation easier for engineers. Deciding how much effort to devote to documentation is a decision your organization will need to make at some point.
</div></details>

并不是每个工程团队都需要一个技术写手(即使是这样，也没有足够的技术写手)。这意味着工程师将自己编写大部分文档。因此，与其强迫工程师成为技术作家，不如考虑如何让工程师更容易地编写文档。在某些时候，您的组织需要决定为文档投入多少精力。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Documentation benefits several different groups. Even to the writer, documentation provides the following benefits:

- It helps formulate an API. Writing documentation is one of the surest ways to figure out if your API makes sense. Often, the writing of the documentation itself leads engineers to reevaluate design decisions that otherwise wouldn’t be questioned. If you can’t explain it and can’t define it, you probably haven’t designed it well enough.
- It provides a road map for maintenance and a historical record. Tricks in code should be avoided, in any case, but good comments help out a great deal when you’re staring at code you wrote two years ago, trying to figure out what’s wrong.
- It makes your code look more professional and drive traffic. Developers will naturally assume that a well-documented API is a better-designed API. That’s not always the case, but they are often highly correlated. Although this benefit sounds cosmetic, it’s not quite so: whether a product has good documentation is usually a pretty good indicator of how well a product will be maintained.
- It will prompt fewer questions from other users. This is probably the biggest benefit over time to someone writing the documentation. If you have to explain something to someone more than once, it usually makes sense to document that process.
</div></details>

文档对几个不同的群体都有好处。即使对作者来说，文档也提供了以下好处:
- 它有助于形成API。编写文档是确定API是否有意义的最可靠的方法之一。通常，编写文档本身会导致工程师重新评估原本不会受到质疑的设计决策。如果你不能解释它，也不能定义它，那么你可能设计得不够好。
- 提供维修路线图和历史记录。在任何情况下都应该避免代码中的技巧，但是当您盯着两年前编写的代码，试图找出错误的地方时，好的注释会有很大的帮助。
- 它让你的代码看起来更专业，并促进流量。开发人员自然会认为文档良好的API是设计得更好的API。情况并非总是如此，但它们通常高度相关。虽然这一好处听起来只是表面文章，但事实并非如此:产品是否有良好的文档通常是一个很好的指标，表明产品将如何维护。
- 这会减少其他用户提出的问题。随着时间的推移，这可能是编写文档的人最大的好处。如果你不得不多次向别人解释某件事，记录下这个过程通常是有意义的。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
As great as these benefits are to the writer of documentation, the lion’s share of documentation’s benefits will naturally accrue to the reader. Google’s C++ Style Guide notes the maxim “optimize for the reader.” This maxim applies not just to code, but to the comments around code, or the documentation set attached to an API. Much like testing, the effort you put into writing good documents will reap benefits many times over its lifetime. Documentation is critical over time, and reaps tremendous benefits for especially critical code as an organization scales.
</div></details>

尽管这些好处对文档的作者来说是巨大的，但文档的大部分好处自然也会为读者带来。谷歌的c++风格指南中提到了“为读者优化”的格言。这条准则不仅适用于代码，也适用于代码周围的注释，或者API附带的文档集。与测试非常相似，您在编写优秀文档方面所付出的努力将在其生命周期中获得多次收益。随着时间的推移，文档是非常重要的，并且随着组织规模的扩大，文档为特别重要的代码带来了巨大的好处。

## 文档和代码差不多（Documentation Is Like Code）

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Software engineers who write in a single, primary programming language still often reach for different languages to solve specific problems. An engineer might write shell scripts or Python to run command-line tasks, or they might write most of their backend code in C++ but write some middleware code in Java, and so on. Each language is a tool in the toolbox.
</div></details>

用单一的、主要的编程语言编写程序的软件工程师仍然经常使用不同的语言来解决特定的问题。工程师可能会编写shell脚本或Python来运行命令行任务，或者他们可能会用c++编写大部分后端代码，但用Java编写一些中间件代码，等等。每种语言都是工具箱中的一种工具。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Documentation should be no different: it’s a tool, written in a different language (usually English) to accomplish a particular task. Writing documentation is not much different than writing code. Like a programming language, it has rules, a particular syntax, and style decisions, often to accomplish a similar purpose as that within code: enforce consistency, improve clarity, and avoid (comprehension) errors. Within technical documentation, grammar is important not because one needs rules, but to standardize the voice and avoid confusing or distracting the reader. Google requires a certain comment style for many of its languages for this reason.
</div></details>
文档也一样:它是一种工具，用不同的语言(通常是英语)编写来完成特定的任务。编写文档与编写代码没有太大的不同。像编程语言一样，它也有规则、特定的语法和风格决策，通常是为了实现与代码中类似的目的:加强一致性、提高清晰度和避免(理解)错误。在技术文档中，语法很重要，不是因为需要规则，而是为了标准化发音，避免混淆或分散读者的注意力。因此，谷歌需要为其许多语言使用特定的注释样式。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Like code, documents should also have owners. Documents without owners become stale and difficult to maintain. Clear ownership also makes it easier to handle documentation through existing developer workflows: bug tracking systems, code review tooling, and so forth. Of course, documents with different owners can still conflict with one another. In those cases, it is important to designate canonical documentation: determine the primary source and consolidate other associated documents into that primary source (or deprecate the duplicates).
</div></details>

与代码一样，文档也应该有所有者。没有所有者的文档会变得陈旧和难以维护。清晰的所有权还使得通过现有的开发人员工作流更容易处理文档:bug跟踪系统、代码审查工具，等等。当然，拥有不同所有者的文档仍然可能相互冲突。在这些情况下，重要的是指定规范文档:确定主源并将其他相关文档合并到该主源中(或弃用副本)。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The prevalent usage of “go/ links” at Google (see Chapter 3) makes this process easier. Documents with straightforward go/ links often become the canonical source of truth. One other way to promote canonical documents is to associate them directly with the code they document by placing them directly under source control and alongside the source code itself.
</div></details>

“go/ links”在谷歌的普遍使用(见第3章)使这个过程更容易。带有直接链接的文档通常会成为事实的权威来源。另一种促进规范文档的方法是，通过将规范文档直接置于源代码控制之下，并与源代码本身放在一起，将规范文档与它们所记录的代码直接关联起来。  

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Documentation is often so tightly coupled to code that it should, as much as possible, be treated as code. That is, your documentation should:
- Have internal policies or rules to be followed 
- Be placed under source control 
- Have clear ownership responsible for maintaining the docs 
- Undergo reviews for changes (and change with the code it documents)
- Have issues tracked, as bugs are tracked in code • Be periodically evaluated (tested, in some respect) 
- If possible, be measured for aspects such as accuracy, freshness, etc. (tools have still not caught up here)
</div></details>

文档通常与代码紧密耦合，因此应该尽可能地将其视为代码。也就是说，你的文档应该:
- 有需要遵守的内部政策或规则
- 处于源代码控制之下
- 有明确的所有者负责维护文件
- 对变更进行评审(和变更的代码文档)
- 跟踪问题，就像在代码中跟踪bug一样•定期评估(在某些方面测试)
- 如果可能的话，在准确性、新鲜度等方面进行测量(工具在这方面还没有赶上)

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The more engineers treat documentation as “one of ” the necessary tasks of software development, the less they will resent the upfront costs of writing, and the more they will reap the long-term benefits. In addition, making the task of documentation easier reduces those upfront costs. 
</div></details>

工程师越是把文档作为软件开发的“必要任务之一”，他们就越不会怨恨前期的编写成本，他们就会收获更多的长期利益。此外，简化文档工作可以减少前期成本。

<div style="border:1px #EEE solid;padding:10px;">

<p align="center">案例分析: The Google Wiki</p>

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
When Google was much smaller and leaner, it had few technical writers. The easiest way to share information was through our own internal wiki (GooWiki). At first, this seemed like a reasonable approach; all engineers shared a single documentation set and could update it as needed.
</div></details>

当谷歌更小更精简的时候，它只有很少的技术编写人员。分享信息最简单的方式就是通过我们自己的内部维基(GooWiki)。一开始，这似乎是一个合理的方法;所有工程师共享一个文档集，并可以根据需要进行更新。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
When Google was much smaller and leaner, it had few technical writers. The easiest way to share information was through our own internal wiki (GooWiki). At first, this seemed like a reasonable approach; all engineers shared a single documentation set and could update it as needed.

But as Google scaled, problems with a wiki-style approach became apparent. Because there were no true owners for documents, many became obsolete.Because no process was put in place for adding new documents, duplicate documents and document sets began appearing. GooWiki had a flat namespace, and people were not good at applying any hierarchy to the documentation sets. At one point, there were 7 to 10 documents (depending on how you counted them) on setting up Borg, our production compute environment, only a few of which seemed to be maintained, and most were specific to certain teams with certain permissions and assumptions.

Another problem with GooWiki became apparent over time: the people who could fix the documents were not the people who used them. New users discovering bad documents either couldn’t confirm that the documents were wrong or didn’t have an easy way to report errors. They knew something was wrong (because the document didn’t work), but they couldn’t “fix” it. Conversely, the people best able to fix the documents often didn’t need to consult them after they were written. The documentation became so poor as Google grew that the quality of documentation became Google’s number one developer complaint on our annual developer surveys.

The way to improve the situation was to move important documentation under the same sort of source control that was being used to track code changes. Documents began to have their own owners, canonical locations within the source tree, and processes for identifying bugs and fixing them; the documentation began to dramatically improve. Additionally, the way documentation was written and maintained began to look the same as how code was written and maintained. Errors in the documents could be reported within our bug tracking software. Changes to the documents could be handled using the existing code review process. Eventually, engineers began to fix the documents themselves or send changes to technical writers (who were often the owners).

Moving documentation to source control was initially met with a lot of controversy. Many engineers were convinced that doing away with the GooWiki, that bastion of freedom of information, would lead to poor quality because the bar for documentation (requiring a review, requiring owners for documents, etc.) would be higher. But that wasn’t the case. The documents became better.

The introduction of Markdown as a common documentation formatting language also helped because it made it easier for engineers to understand how to edit documents without needing specialized expertise in HTML or CSS. Google eventually introduced its own framework for embedding documentation within code: g3doc. With that framework, documentation improved further, as documents existed side by side with the source code within the engineer’s development environment. Now, engineers could update the code and its associated documentation in the same change (a practice for which we’re still trying to improve adoption).

The key difference was that maintaining documentation became a similar experience to maintaining code: engineers filed bugs, made changes to documents in changelists, sent changes to reviews by experts, and so on. Leveraging of existing developer workflows, rather than creating new ones, was a key benefit.
</div></details>

但随着谷歌规模的扩大，wiki风格方法的问题变得明显起来。由于没有文档的真正所有者，很多文档都过时了。由于没有设置添加新文档的过程，重复的文档和文档集开始出现。GooWiki有一个扁平的名称空间，人们不擅长将任何层次结构应用到文档集。在设置我们的生产计算环境Borg时，有7到10个文档(取决于你如何计算)，但似乎只有少数文件需要维护，大多数都是针对具有特定权限和假设的特定团队。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
But as Google scaled, problems with a wiki-style approach became apparent. Because there were no true owners for documents, many became obsolete.Because no process was put in place for adding new documents, duplicate documents and document sets began appearing. GooWiki had a flat namespace, and people were not good at applying any hierarchy to the documentation sets. At one point, there were 7 to 10 documents (depending on how you counted them) on setting up Borg, our production compute environment, only a few of which seemed to be maintained, and most were specific to certain teams with certain permissions and assumptions.
</div></details>

随着时间的推移，GooWiki的另一个问题变得越来越明显:能够修复文档的人不是使用它们的人。新用户发现错误的文档时，要么无法确认文档是错误的，要么没有简单的方法来报告错误。他们知道有些地方出了问题(因为文档不能工作)，但是他们不能“修复”它。相反，最能修复文档的人在文档编写后通常不需要咨询文档。随着谷歌的发展，文档变得非常糟糕，以至于在我们的年度开发者调查中，文档质量成为谷歌开发者抱怨最多的问题。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Another problem with GooWiki became apparent over time: the people who could fix the documents were not the people who used them. New users discovering bad documents either couldn’t confirm that the documents were wrong or didn’t have an easy way to report errors. They knew something was wrong (because the document didn’t work), but they couldn’t “fix” it. Conversely, the people best able to fix the documents often didn’t need to consult them after they were written. The documentation became so poor as Google grew that the quality of documentation became Google’s number one developer complaint on our annual developer surveys.
</div></details>

改善这种情况的方法是将重要的文档移到与跟踪代码更改相同的源代码控制下。文档开始有自己的所有者，在源代码树中的规范位置，以及识别和修复bug的过程;文档开始显著改善。此外，编写和维护文档的方式开始与编写和维护代码的方式相同。文档中的错误可以在我们的bug跟踪软件中报告。可以使用现有的代码审查过程来处理对文档的更改。最终，工程师们开始自己修复文档，或者将更改发送给技术作者(他们通常是文档的所有者)。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The way to improve the situation was to move important documentation under the same sort of source control that was being used to track code changes. Documents began to have their own owners, canonical locations within the source tree, and processes for identifying bugs and fixing them; the documentation began to dramatically improve. Additionally, the way documentation was written and maintained began to look the same as how code was written and maintained. Errors in the documents could be reported within our bug tracking software. Changes to the documents could be handled using the existing code review process. Eventually, engineers began to fix the documents themselves or send changes to technical writers (who were often the owners).
</div></details>

将文档转移到源代码控制最初遇到了很多争议。许多工程师确信，废除GooWiki这个信息自由的堡垒，会导致质量低下，因为文档的门槛(需要审查，需要文档所有者等)会更高。但事实并非如此。文件变得更好了。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The introduction of Markdown as a common documentation formatting language also helped because it made it easier for engineers to understand how to edit documents without needing specialized expertise in HTML or CSS. Google eventually introduced its own framework for embedding documentation within code: g3doc. With that framework, documentation improved further, as documents existed side by side with the source code within the engineer’s development environment. Now, engineers could update the code and its associated documentation in the same change (a practice for which we’re still trying to improve adoption).
</div></details>

Markdown作为一种通用文档格式语言的引入也有帮助，因为它使工程师更容易理解如何编辑文档，而不需要HTML或CSS方面的专业知识。谷歌最终引入了自己的框架，用于在代码中嵌入文档:g3doc。有了这个框架，文档得到了进一步的改进，因为在工程师的开发环境中，文档与源代码并列存在。现在，工程师可以在相同的变更中更新代码及其相关的文档(我们仍在努力改进这一实践)。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The key difference was that maintaining documentation became a similar experience to maintaining code: engineers filed bugs, made changes to documents in changelists, sent changes to reviews by experts, and so on. Leveraging of existing developer workflows, rather than creating new ones, was a key benefit.
</div></details>

关键的区别在于，维护文档与维护代码的体验是相似的:工程师归档错误，在变更列表中对文档进行更改，将更改发送给专家评审，等等。利用现有的开发人员工作流，而不是创建新的工作流，是一个关键的好处。
</div>

## Know Your Audience

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
One of the most important mistakes that engineers make when writing documentation is to write only for themselves. It’s natural to do so, and writing for yourself is not without value: after all, you might need to look at this code in a few years and try to figure out what you once meant. You also might be of approximately the same skill set as someone reading your document. But if you write only for yourself, you are going to make certain assumptions, and given that your document might be read by a very wide audience (all of engineering, external developers), even a few lost readers is a large cost. As an organization grows, mistakes in documentation become more prominent, and your assumptions often do not apply.
</div></details>

工程师在编写文档时所犯的最重要的错误之一就是只为自己编写文档。这样做是很自然的，为自己编写代码也不是没有价值的:毕竟，您可能需要在几年后查看这些代码，并试图弄清楚您曾经的意思。您也可能与阅读您的文档的人拥有大致相同的技能。但是，如果您只是为自己编写文档，那么您将做出一定的假设，并且考虑到您的文档可能会被非常广泛的读者(所有的工程人员、外部开发人员)阅读，即使是一些失去的读者也是巨大的成本。随着组织的发展，文档中的错误变得更加突出，而您的假设通常并不适用。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Instead, before you begin writing, you should (formally or informally) identify the audience(s) your documents need to satisfy. A design document might need to persuade decision makers. A tutorial might need to provide very explicit instructions to someone utterly unfamiliar with your codebase. An API might need to provide complete and accurate reference information for any users of that API, be they experts or novices. Always try to identify a primary audience and write to that audience.
</div></details>

相反，在开始写作之前，您应该(正式或非正式地)确定您的文档需要满足哪些读者。设计文件可能需要说服决策者。教程可能需要向完全不熟悉您的代码库的人提供非常明确的说明。API可能需要为该API的任何用户(无论是专家还是新手)提供完整和准确的参考信息。总是试着确定一个主要的受众，并给这个受众写信。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Good documentation need not be polished or “perfect.” One mistake engineers make when writing documentation is assuming they need to be much better writers. By that measure, few software engineers would write. Think about writing like you do about testing or any other process you need to do as an engineer. Write to your audience, in the voice and style that they expect. If you can read, you can write. Remember that your audience is standing where you once stood, but without your new domain knowledge. So you don’t need to be a great writer; you just need to get someone like you as familiar with the domain as you now are. (And as long as you get a stake in the ground, you can improve this document over time.)
</div></details>

好的文档不需要润色或“完美”。工程师在编写文档时犯的一个错误是，他们认为自己需要成为更好的写手。按照这个标准，很少有软件工程师会写作。把写作想象成测试或作为工程师需要做的任何其他过程。给你的听众写信，用他们期望的声音和风格。如果你能读，你就能写。记住，你的听众和你站在同样的位置，但没有你的新领域知识。所以你不需要成为一个伟大的作家;你只需要找一个像你一样熟悉这个领域的人。(而且，只要你已经做好了准备，你就可以随着时间的推移改进这份文件。)

### Types of Audiences


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
We’ve pointed out that you should write at the skill level and domain knowledge appropriate for your audience. But who precisely is your audience? Chances are, you have multiple audiences based on one or more of the following criteria:
 - Experience level (expert programmers, or junior engineers who might not even be familiar—gulp!—with the language). 
- Domain knowledge (team members, or other engineers in your organization who are familiar only with API endpoints).
 - Purpose (end users who might need your API to do a specific task and need to find that information quickly, or software gurus who are responsible for the guts of a particularly hairy implementation that you hope no one else needs to maintain).
</div></details>

我们已经指出，你应该以适合你的读者的技能水平和领域知识来写作。但到底谁是你的受众呢?可能的情况是，基于以下一个或多个标准，你有多个受众:
- 经验水平(专业程序员或初级工程师，甚至可能不熟悉!)——语言)。
- 领域知识(团队成员，或组织中仅熟悉API端点的其他工程师)。
- 目的(可能需要你的API执行特定任务并需要快速找到该信息的终端用户，或者负责一个特别麻烦的实现的核心的软件大师，你希望其他人不需要维护这个实现)。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In some cases, different audiences require different writing styles, but in most cases, the trick is to write in a way that applies as broadly to your different audience groups as possible. Often, you will need to explain a complex topic to both an expert and a novice. Writing for the expert with domain knowledge may allow you to cut corners, but you’ll confuse the novice; conversely, explaining everything in detail to the novice will doubtless annoy the expert. 
</div></details>

在某些情况下，不同的读者需要不同的写作风格，但在大多数情况下，诀窍在于尽可能广泛地适用于不同的读者群体。通常，您需要向专家和新手解释一个复杂的主题。为拥有领域知识的专家写作可能会让你走捷径，但你会让新手感到困惑;相反地，向新手详细解释一切无疑会惹恼专家



<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Obviously, writing such documents is a balancing act and there’s no silver bullet, but one thing we’ve found is that it helps to keep your documents short. Write descriptively enough to explain complex topics to people unfamiliar with the topic, but don’t lose or annoy experts. Writing a short document often requires you to write a longer one (getting all the information down) and then doing an edit pass, removing duplicate information where you can. This might sound tedious, but keep in mind that this expense is spread across all the readers of the documentation. As Blaise Pascal once said, “If I had more time, I would have written you a shorter letter.” By keeping a document short and clear, you will ensure that it will satisfy both an expert and a novice.
</div></details>

显然，编写这样的文档是一种平衡的行为，没有什么灵丹妙药，但我们发现，它有助于保持文档简短。以描述性的方式向不熟悉该主题的人解释复杂的主题，但不要失去或惹恼专家。写一篇简短的文档通常需要你写一篇较长的文档(记下所有的信息)，然后进行编辑，尽可能地删除重复的信息。这可能听起来很乏味，但请记住，这种费用是在文档的所有读者中分摊的。正如布莱斯•帕斯卡曾经说过:“如果我有更多的时间，我会给你写一封更短的信。”通过保持一份简短而清晰的文档，你将确保它能够同时满足专家和新手的需求。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Another important audience distinction is based on how a user encounters a document: 
- Seekers are engineers who know what they want and want to know if what they are looking at fits the bill. A key pedagogical device for this audience is consistency. If you are writing reference documentation for this group—within a code file, for example—you will want to have your comments follow a similar format so that readers can quickly scan a reference and see whether they find what they are looking for.
 - Stumblers might not know exactly what they want. They might have only a vague idea of how to implement what they are working with. The key for this audience is clarity. Provide overviews or introductions (at the top of a file, for example) that explain the purpose of the code they are looking at. It’s also useful to identify when a doc is not appropriate for an audience. A lot of documents at Google begin with a “TL;DR statement” such as “TL;DR: if you are not interested in C++ compilers at Google, you can stop reading now.”
</div></details>

另一个重要的受众区别是基于用户遇到文档的方式:
- 搜索者是工程师，他们知道自己想要什么，并想知道他们正在寻找的东西是否符合要求。对这个听众来说，一个关键的教学方法就是保持一致性。如果您正在为该组编写参考文档，例如在代码文件中，您将希望您的注释遵循类似的格式，以便读者能够快速浏览参考文档，并查看他们是否找到了他们想要的内容。
- “绊倒者”可能不知道自己到底想要什么。他们可能对如何实现他们正在使用的东西只有一个模糊的概念。对这些观众来说，关键在于清晰。提供概述或介绍(例如，在文件的顶部)来解释他们正在查看的代码的目的。识别文档何时不适合受众也是很有用的。很多谷歌的文档都以“TL;DR语句”开头，比如“TL;DR:如果你对谷歌的c++编译器不感兴趣，现在就可以停止阅读了。”

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Finally, one important audience distinction is between that of a customer (e.g., a user of an API) and that of a provider (e.g., a member of the project team). As much as possible, documents intended for one should be kept apart from documents intended for the other. Implementation details are important to a team member for maintenance purposes; end users should not need to read such information. Often, engineers denote design decisions within the reference API of a library they publish. Such reasonings belong more appropriately in specific documents (design documents) or, at best, within the implementation details of code hidden behind an interface.
</div></details>
最后，一个重要的受众区别是客户(例如API的用户)和提供者(例如项目团队的成员)之间的区别。为其中一方准备的文件应尽可能与为另一方准备的文件分开保存。实现细节对团队成员的维护非常重要;最终用户不需要阅读这些信息。通常，工程师在他们发布的库的参考API中表示设计决策。这种推理更适合于特定的文档(设计文档)，或者，最好是隐藏在接口后面的代码实现细节中。


## Documentation Types

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Engineers write various different types of documentation as part of their work: design documents, code comments, how-to documents, project pages, and more. These all count as “documentation.” But it is important to know the different types, and to not mix types. A document should have, in general, a singular purpose, and stick to it. Just as an API should do one thing and do it well, avoid trying to do several things within one document. Instead, break out those pieces more logically. 
</div></details>

工程师编写各种不同类型的文档作为他们工作的一部分:设计文档、代码注释、操作文档、项目页面等等。这些都可以算作“文档”。但重要的是要了解不同的类型，不要混合类型。一般来说，一份文件应该有一个单一的目的，并坚持下去。正如API应该做一件事并做好它一样，避免试图在一个文档中做几件事。相反，要更有逻辑地分解这些部分。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

There are several main types of documents that software engineers often need to write: 
- Reference documentation, including code comments 
- Design documents
- Tutorials
- Conceptual documentation 
- Landing pages
</div></details>

软件工程师通常需要编写几种主要类型的文档:
- 参考文档，包括代码注释
- 设计文档
- 教程
- 概念性文档
- 登陆页面


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
It was common in the early days of Google for teams to have monolithic wiki pages with bunches of links (many broken or obsolete), some conceptual information about how the system worked, an API reference, and so on, all sprinkled together. Such documents fail because they don’t serve a single purpose (and they also get so long that no one will read them; some notorious wiki pages scrolled through several dozens of screens). Instead, make sure your document has a singular purpose, and if adding something to that page doesn’t make sense, you probably want to find, or even create, another document for that purpose.
</div></details>
在早期的谷歌开发团队中，通常会有一个单一的wiki页面，其中包含大量链接(很多已经失效或过时)、一些关于系统如何工作的概念性信息、一个API引用等等。这类文件之所以失败，是因为它们没有一个单一的目的(而且它们也太长了，没有人会读它们;一些臭名昭著的维基页面在几十个屏幕上滚动)。相反，要确保您的文档有一个单一的目的，如果向该页面添加某些内容没有意义，您可能希望找到或甚至创建另一个文档用于此目的。


### Reference Documentation

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Reference documentation is the most common type that engineers need to write; indeed, they often need to write some form of reference documents every day. By reference documentation, we mean anything that documents the usage of code within the codebase. Code comments are the most common form of reference documentation that an engineer must maintain. Such comments can be divided into two basic camps: API comments versus implementation comments. Remember the audience differences between these two: API comments don’t need to discuss implementation details or design decisions and can’t assume a user is as versed in the API as the author. Implementation comments, on the other hand, can assume a lot more domain knowledge of the reader, though be careful in assuming too much: people leave projects, and sometimes it’s safer to be methodical about exactly why you wrote this code the way you did.
</div></details>
参考文档是工程师需要编写的最常见的文档类型;事实上，他们经常需要每天编写某种形式的参考文档。通过引用文档，我们指的是任何记录代码库中代码使用情况的文档。代码注释是工程师必须维护的最常见的参考文档形式。这些注释可以分为两个基本阵营:API注释和实现注释。记住这两者之间的受众差异:API注释不需要讨论实现细节或设计决策，也不能假设用户和作者一样精通API。另一方面，实现注释可以假设读者有更多的领域知识，但要小心，不要假设太多:人们会离开项目，有时候，有系统地了解为什么要这样写代码会更安全。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Most reference documentation, even when provided as separate documentation from the code, is generated from comments within the codebase itself. (As it should; reference documentation should be single-sourced as much as possible.) Some languages such as Java or Python have specific commenting frameworks (Javadoc, PyDoc, GoDoc) meant to make generation of this reference documentation easier. Other languages, such as C++, have no standard “reference documentation” implementation, but because C++ separates out its API surface (in header or .h files) from the implementation (.cc files), header files are often a natural place to document a C++ API.
</div></details>

大多数参考文档，即使作为独立于代码的文档提供，也是由代码库本身的注释生成的。(就像它应该;参考文档应该尽可能是单一来源的。)有些语言，如Java或Python，有特定的注释框架(Javadoc、PyDoc、GoDoc)，这意味着更容易生成参考文档。其他语言，如c++，没有标准的“参考文档”实现，但因为c++将其API表面(在头文件或.h文件中)与实现(。，头文件通常是记录c++ API的一个自然位置。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Google takes this approach: a C++ API deserves to have its reference documentation live within the header file. Other reference documentation is embedded directly in the Java, Python, and Go source code as well. Because Google’s Code Search browser (see Chapter 17) is so robust, we’ve found little benefit to providing separate generated reference documentation. Users in Code Search not only search code easily, they can usually find the original definition of that code as the top result. Having the documentation alongside the code’s definitions also makes the documentation easier to discover and maintain.
</div></details>
谷歌采用了这种方法:c++ API应该在头文件中包含参考文档。其他参考文档也直接嵌入到Java、Python和Go源代码中。因为谷歌的代码搜索浏览器(见第17章)非常健壮，我们发现提供单独生成的参考文档并没有什么好处。代码搜索的用户不仅可以轻松地搜索代码，他们通常可以在最上面的结果中找到代码的原始定义。将文档与代码定义放在一起，还可以使文档更容易发现和维护。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
We all know that code comments are essential to a well-documented API. But what precisely is a “good” comment? Earlier in this chapter, we identified two major audiences for reference documentation: seekers and stumblers. Seekers know what they want; stumblers don’t. The key win for seekers is a consistently commented codebase so that they can quickly scan an API and find what they are looking for. The key win for stumblers is clearly identifying the purpose of an API, often at the top of a file header. We’ll walk through some code comments in the subsections that follow. The code commenting guidelines that follow apply to C++, but similar rules are in place at Google for other languages.
</div></details>
我们都知道代码注释对于文档良好的API来说是必不可少的。但什么才是“好”评论呢?在本章的前面，我们确定了参考文档的两个主要受众:搜索者和绊倒者。探索者知道他们想要什么;有过失者不要。对于搜索者来说，关键的胜利是一个一致的注释代码库，这样他们就可以快速扫描一个API并找到他们想要的东西。对于绊脚者来说，关键在于清楚地识别API的用途，通常是在文件头的顶部。我们将在后面的小节中详细介绍一些代码注释。下面的代码注释准则适用于c++，但类似的规则在谷歌中也适用于其他语言。

#### 文件注释 File comments

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Almost all code files at Google must contain a file comment. (Some header files that contain only one utility function, etc., might deviate from this standard.) File comments should begin with a header of the following form:
	
</div></details>

几乎谷歌中的所有代码文件都必须包含文件注释。(有些头文件只包含一个实用函数，等等，可能会偏离这个标准。)文件注释应该以以下表单的头文件开头:

```c++
// -----------------------------------------------------------------------------
// str_cat.h
// -----------------------------------------------------------------------------
//
// This header file contains functions for efficiently concatenating and appending
// strings: StrCat() and StrAppend(). Most of the work within these routines is
// actually handled through use of a special AlphaNum type, which was designed
// to be used as a parameter type that efficiently manages conversion to
// strings and avoids copies in the above operations.
…
```


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Generally, a file comment should begin with an outline of what’s contained in the code you are reading. It should identify the code’s main use cases and intended audience (in the preceding case, developers who want to concatenate strings). Any API that cannot be succinctly described in the first paragraph or two is usually the sign of an API that is not well thought out. Consider breaking the API into separate components in those cases.
</div></details>

通常，文件注释应该以您正在阅读的代码中包含的内容的大纲开头。它应该标识代码的主要用例和目标受众(在前面的例子中，是希望连接字符串的开发人员)。任何不能在第一或第二段中简单描述的API通常都是一个没有考虑周全的API的标志。在这些情况下，考虑将API分解为独立的组件。

#### 类注释 Class comments


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Most modern programming languages are object oriented. Class comments are therefore important for defining the API “objects” in use in a codebase. All public classes (and structs) at Google must contain a class comment describing the class/struct, important methods of that class, and the purpose of the class. Generally, class comments should be “nouned” with documentation emphasizing their object aspect. That is, say, “The Foo class contains x, y, z, allows you to do Bar, and has the following Baz aspects,” and so on. Class comments should generally begin with a comment of the following form:
</div></details>

大多数现代编程语言都是面向对象的。因此，类注释对于定义代码库中使用的API“对象”非常重要。谷歌的所有公共类(和结构)必须包含一个类注释，描述类/结构、该类的重要方法和该类的目的。通常，类注释应该用强调其对象方面的文档来“命名”。也就是说，“Foo类包含x, y, z，允许您执行Bar，并具有以下Baz方面，”等等。类注释通常应该以以下形式的注释开始:
```c++
// -----------------------------------------------------------------------------
// AlphaNum
// -----------------------------------------------------------------------------
//
// The AlphaNum class acts as the main parameter type for StrCat() and
// StrAppend(), providing efficient conversion of numeric, boolean, and
// hexadecimal values (through the Hex type) into strings.
```

#### 函数注释 Function comments

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
All free functions, or public methods of a class, at Google must also contain a function comment describing what the function does. Function comments should stress the active nature of their use, beginning with an indicative verb describing what the function does and what is returned. Function comments should generally begin with a comment of the following form:
ith a comment of the following form:
</div></details>
所有自由函数，或类的公共方法，在谷歌也必须包含函数注释，描述函数的功能。函数注释应该强调它们使用的主动性质，以指示性动词开头，描述函数的功能和返回的内容。函数注释通常应该以以下形式的注释开始:

```c++
// StrCat()
//
// Merges the given strings or numbers, using no delimiter(s),
// returning the merged result as a string.
```


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Note that starting a function comment with a declarative verb introduces consistency across a header file. A seeker can quickly scan an API and read just the verb to get an idea of whether the function is appropriate: “Merges, Deletes, Creates,” and so on.

</div></details>

请注意，以声明性动词开始函数注释会在头文件中引入一致性。搜索者可以快速地扫描一个API，只读一个动词，就能知道这个函数是否合适:“合并、删除、创建”，等等。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Some documentation styles (and some documentation generators) require various forms of boilerplate on function comments, like “Returns:”, “Throws:”, and so forth, but at Google we haven’t found them to be necessary. It is often clearer to present such information in a single prose comment that’s not broken up into artificial section boundaries:
</div></details>

一些文档样式(和一些文档生成器)需要函数注释的各种形式的样板，如“Returns:”、“Throws:”等等，但在谷歌中，我们没有发现它们是必要的。通常，在一个没有被分割成人工分段边界的单一散文评论中呈现这些信息会更清晰:
```c++
// Creates a new record for a customer with the given name and address,
// and returns the record ID, or throws `DuplicateEntryError` if a
// record with that name already exists.
int AddCustomer(string name, string address);
```


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Notice how the postcondition, parameters, return value, and exceptional cases are naturally documented together (in this case, in a single sentence), because they are not independent of one another. Adding explicit boilerplate sections would make the comment more verbose and repetitive, but no clearer (and arguably less clear). 
</div></details>
请注意后置条件、参数、返回值和异常情况是如何被自然地记录在一起的(在本例中，是在一个句子中)，因为它们不是彼此独立的。添加显式的样板部分会使注释更加冗长和重复，但不会更清晰(也可能不那么清晰)。


### 设计文档 Design Docs

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Most teams at Google require an approved design document before starting work on any major project. A software engineer typically writes the proposed design document using a specific design doc template approved by the team. Such documents are designed to be collaborative, so they are often shared in Google Docs, which has good collaboration tools. Some teams require such design documents to be discussed and debated at specific team meetings, where the finer points of the design can be discussed or critiqued by experts. In some respects, these design discussions act as a form of code review before any code is written.

</div></details>

谷歌的大多数团队在开始任何重大项目之前都需要一份经过批准的设计文件。软件工程师通常使用团队批准的特定设计文档模板编写建议的设计文档。这类文档被设计成协作性的，所以它们经常在谷歌Docs中被共享，后者有很好的协作工具。有些团队要求在特定的团队会议上讨论这些设计文件，专家可以在会议上讨论或批评设计的细节。在某些方面，这些设计讨论可以作为编写任何代码之前的一种代码审查形式。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Because the development of a design document is one of the first processes an engineer undertakes before deploying a new system, it is also a convenient place to ensure that various concerns are covered. The canonical design document templates at Google require engineers to consider aspects of their design such as security implications, internationalization, storage requirements and privacy concerns, and so on. In most cases, such parts of those design documents are reviewed by experts in those domains.
</div></details>

因为设计文档的开发是工程师部署新系统之前的第一个过程，所以它也是一个方便的地方，可以确保各种关注都被涵盖。谷歌规范的设计文档模板要求工程师考虑其设计的各个方面，如安全含义、国际化、存储需求和隐私问题等。在大多数情况下，这些设计文件的这些部分是由这些领域的专家审查的。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
A good design document should cover the goals of the design, its implementation strategy, and propose key design decisions with an emphasis on their individual trade-offs. The best design documents suggest design goals and cover alternative designs, denoting their strong and weak points.
</div></details>

一个好的设计文件应该涵盖设计的目标、实现策略，并提出关键的设计决策，并强调它们各自的权衡。最好的设计文件会提出设计目标，并涵盖备选设计，指出它们的优缺点。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
A good design document, once approved, also acts not only as a historical record, but as a measure of whether the project successfully achieved its goals. Most teams archive their design documents in an appropriate location within their team documents so that they can review them at a later time. It’s often useful to review a design document before a product is launched to ensure that the stated goals when the design document was written remain the stated goals at launch (and if they do not, either the document or the product can be adjusted accordingly).
</div></details>

一份优秀的设计文件，一旦获得批准，不仅是一份历史记录，也是衡量项目是否成功实现目标的标准。大多数团队将他们的设计文档存档在团队文档中适当的位置，这样他们就可以在稍后的时候查看它们。在产品发布前回顾设计文件是非常有用的，以确保设计文件所陈述的目标与发布时所陈述的目标保持一致(如果不是这样，那么文件或产品都可以进行相应的调整)。


### 教程 Tutorials

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Every software engineer, when they join a new team, will want to get up to speed as quickly as possible. Having a tutorial that walks someone through the setup of a new project is invaluable; “Hello World” has established itself is one of the best ways to ensure that all team members start off on the right foot. This goes for documents as well as code. Most projects deserve a “Hello World” document that assumes nothing and gets the engineer to make something “real” happen.
</div></details>
每个软件工程师，当他们加入一个新的团队时，都会想要尽快跟上进度。拥有指导某人如何设置新项目的教程是非常宝贵的;“Hello World”已经建立了自己的最佳方式，以确保所有团队成员从正确的开始。这适用于文档和代码。大多数项目都需要一个“Hello World”文档，它不假设任何事情，并让工程师实现一些“真实”的事情。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Often, the best time to write a tutorial, if one does not yet exist, is when you first join a team. (It’s also the best time to find bugs in any existing tutorial you are following.) Get a notepad or other way to take notes, and write down everything you need to do along the way, assuming no domain knowledge or special setup constraints; after you’re done, you’ll likely know what mistakes you made during the process—and why —and can then edit down your steps to get a more streamlined tutorial. Importantly, write everything you need to do along the way; try not to assume any particular setup, permissions, or domain knowledge. If you do need to assume some other setup, state that clearly in the beginning of the tutorial as a set of prerequisites.
</div></details>
通常，编写教程的最佳时间(如果还没有教程的话)是在你第一次加入团队的时候。(这也是在现有教程中发现漏洞的最佳时机。)用一个记事本或其他方法做笔记，写下一路上你需要做的所有事情，假设没有领域知识或特殊的设置限制;在你完成之后，你可能会知道你在这个过程中犯了什么错误——为什么——然后编辑你的步骤，得到一个更精简的教程。重要的是，写下你在写作过程中需要做的所有事情;尽量不要假设任何特定的设置、权限或领域知识。如果你确实需要一些其他的设置，请在教程的开始部分清楚地说明作为一组先决条件。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Most tutorials require you to perform a number of steps, in order. In those cases, number those steps explicitly. If the focus of the tutorial is on the user (say, for external developer documentation), then number each action that a user needs to undertake. Don’t number actions that the system may take in response to such user actions. It is critical and important to number explicitly every step when doing this. Nothing is more annoying than an error on step 4 because you forget to tell someone to properly authorize their username, for example.
</div></details>
大多数教程要求您按顺序执行许多步骤。在这些情况下，明确地为这些步骤编号。如果教程的重点是用户(例如，对于外部开发人员文档)，那么就为用户需要执行的每个操作编号。不要对系统响应这些用户操作时可能采取的操作进行编号。在这样做的时候，明确地为每一步编号是至关重要的。没有什么比第4步的错误更恼人的了，因为您忘记告诉某人正确授权他们的用户名，例如。

#### 示例:一个糟糕的教程（ A bad tutorial）

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

1. Download the package from our server at http://example.com
2. Copy the shell script to your home directory
3. Execute the shell script
4. The foobar system will communicate with the authentication system
5. Once authenticated, foobar will bootstrap a new database named “baz”
6. Test “baz” by executing a SQL command on the command line
7. Type: CREATE DATABASE my_foobar_db;
</div></details>

1. 从我们的服务器http://example.com下载这个包
2. 将shell脚本复制到您的主目录
3.执行shell脚本
4. foobar系统将与认证系统进行通信
5. 一旦通过身份验证，foobar将引导一个名为“baz”的新数据库。
6. 通过在命令行上执行SQL命令来测试“baz”
7. 类型:创建数据库my_foobar_db;


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In the preceding procedure, steps 4 and 5 happen on the server end. It’s unclear whether the user needs to do anything, but they don’t, so those side effects can be mentioned as part of step 3. As well, it’s unclear whether step 6 and step 7 are different. (They aren’t.) Combine all atomic user operations into single steps so that the user knows they need to do something at each step in the process. Also, if your tutorial has user-visible input or output, denote that on separate lines (often using the convention of a monospaced bold font).
</div></details>

上述步骤中，步骤4和步骤5发生在服务器端。目前还不清楚用户是否需要做什么，但他们没有，所以这些副作用可以作为第三步的一部分提到。同样，我们也不清楚第六步和第七步是否不同。(他们不是。)将所有原子用户操作合并成单个步骤，这样用户就知道他们需要在流程的每个步骤中做一些事情。此外，如果教程有用户可见的输入或输出，请在单独的行中表示(通常使用等宽粗体字体)。

#### 示例: 将上述教程调整一下（A bad tutorial made better）

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

1. Download the package from our server at http://example.com:
$ curl -I http://example.com
2. Copy the shell script to your home directory:
$ cp foobar.sh ~
3. Execute the shell script in your home directory:
$ cd ~; foobar.sh
The foobar system will first communicate with the authentication system. Once authenticated, foobar will bootstrap a new database named “baz” and open an input shell.
4. Test “baz” by executing a SQL command on the command line:

``` baz:$ CREATE DATABASE my_foobar_db;```

</div></details>
1. 从我们的服务器http://example.com:下载这个包
```bash
 $ curl -I http://example.com
```

2. 复制shell脚本到你的主目录:
```bash
$ cp foobar.sh ~
```

3. 在主目录下执行shell脚本:
```
$ cd ~;foobar.sh
```

foobar系统将首先与认证系统通信。一旦通过身份验证，foobar将引导一个名为“baz”的新数据库，并打开一个输入shell。

4. 通过在命令行上执行SQL命令来测试" baz ":
```
$ CREATE DATABASE my_foobar_db;
```


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Note how each step requires specific user intervention. If, instead, the tutorial had a focus on some other aspect (e.g., a document about the “life of a server”), number those steps from the perspective of that focus (what the server does).
</div></details>
当需要用户操作时需要详细说明每一步应该怎么做。相反，如果教程关注的是其他方面(例如，关于“服务器的生命”的文档)，那么就从关注的角度(服务器做什么)对这些步骤进行编号。

### 概念性文档 Conceptual Documentation

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Conceptual DocumentationSome code requires deeper explanations or insights than can be obtained simply by reading the reference documentation. In those cases, we need conceptual documentation to provide overviews of the APIs or systems. Some examples of conceptual documentation might be a library overview for a popular API, a document describing the life cycle of data within a server, and so on. In almost all cases, a conceptual document is meant to augment, not replace, a reference documentation set. Often this leads to duplication of some information, but with a purpose: to promote clarity. In those cases, it is not necessary for a conceptual document to cover all edge cases (though a reference should cover those cases religiously). In this case, sacrificing some accuracy is acceptable for clarity. The main point of a conceptual document is to impart understanding.
</div></details>

概念性文档有些代码需要更深入的解释或见解，而不是仅仅通过阅读参考文档就能获得。在这些情况下，我们需要概念性文档来提供api或系统的概述。概念性文档的一些示例可能是流行API的库概述，描述服务器中数据生命周期的文档，等等。几乎在所有情况下，概念文档都是用来扩充而不是取代参考文档集的。这通常会导致一些信息的重复，但目的是:提高清晰度。在这些情况下，没有必要用概念文档来涵盖所有的边缘情况(尽管参考文献应该严格地涵盖这些情况)。在这种情况下，牺牲一些准确性是可以接受的清晰度。概念性文件的要点是传达理解。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
“Concept” documents are the most difficult forms of documentation to write. As a result, they are often the most neglected type of document within a software engineer’s toolbox. One problem engineers face when writing conceptual documentation is that it often cannot be embedded directly within the source code because there isn’t a canonical location to place it. Some APIs have a relatively broad API surface area, in which case, a file comment might be an appropriate place for a “conceptual” explanation of the API. But often, an API works in conjunction with other APIs and/or modules. The only logical place to document such complex behavior is through a separate conceptual document. If comments are the unit tests of documentation, conceptual documents are the integration tests.
</div></details>
“概念”文档是最难编写的文档形式。因此，它们常常是软件工程师工具箱中最容易被忽视的文档类型。工程师在编写概念性文档时面临的一个问题是，它常常不能直接嵌入到源代码中，因为没有一个规范的位置来放置它。有些API具有相对广泛的API表面积，在这种情况下，文件注释可能是对API进行“概念性”解释的合适位置。但通常，一个API与其他API和/或模块一起工作。记录这种复杂行为的唯一合理的地方是通过一个单独的概念文档。如果注释是文档的单元测试，那么概念文档就是集成测试。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Even when an API is appropriately scoped, it often makes sense to provide a separate conceptual document. For example, Abseil’s StrFormat library covers a variety of concepts that accomplished users of the API should understand. In those cases, both internally and externally, we provide a format concepts document.
</div></details>
即使API的作用域是适当的，提供单独的概念文档通常也是有意义的。例如，Abseil的StrFormat库涵盖了API用户应该理解的各种概念。在这些情况下，无论是内部还是外部，我们都提供格式概念文档。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
A concept document needs to be useful to a broad audience: both experts and novices alike. Moreover, it needs to emphasize clarity, so it often needs to sacrifice completeness (something best reserved for a reference) and (sometimes) strict accuracy. That’s not to say a conceptual document should intentionally be inaccurate; it just means that it should focus on common usage and leave rare usages or side effects for reference documentation.
</div></details>
概念文档需要对广大读者有用:专家和新手都一样。此外，它需要强调清晰度，所以它经常需要牺牲完整性(最好保留给参考)和(有时)严格的准确性。这并不是说概念性文件应该故意不准确;这只是意味着它应该关注常见的用法，而将很少的用法或副作用留给参考文档。

### 引导页 Landing Pages

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Most engineers are members of a team, and most teams have a “team page” somewhere on their company’s intranet. Often, these sites are a bit of a mess: a typical landing page might contain some interesting links, sometimes several documents titled “read this first!”, and some information both for the team and for its customers. Such documents start out useful but rapidly turn into disasters; because they become so cumbersome to maintain, they will eventually get so obsolete that they will be fixed by only the brave or the desperate.
</div></details>
大多数工程师都是一个团队的成员，而且大多数团队在公司内部网的某个地方都有一个“团队页面”。通常，这些站点都有点乱:一个典型的登录页面可能包含一些有趣的链接，有时几个标题为“先读这个!”，以及为团队和客户提供的一些信息。这些文件一开始很有用，但很快就变成了灾难;由于维护起来太麻烦，它们最终会变得过时，只有勇敢者或走投无路者才能修复它们。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Luckily, such documents look intimidating, but are actually straightforward to fix: ensure that a landing page clearly identifies its purpose, and then include only links to other pages for more information. If something on a landing page is doing more than being a traffic cop, it is not doing its job. If you have a separate setup document, link to that from the landing page as a separate document. If you have too many links on the landing page (your page should not scroll multiple screens), consider breaking up the pages by taxonomy, under different sections.
</div></details>

幸运的是，这样的文档看起来很吓人，但实际上很容易修复:确保登陆页面清楚地表明其目的，然后只包含指向其他页面的链接以获取更多信息。如果登陆页面上的某些东西不仅仅是一个交警，那么它就没有完成它的工作。如果你有一个单独的设置文档，从登录页链接到它作为一个单独的文档。如果登陆页上有太多的链接(页面不应该滚动多个屏幕)，考虑将页面按分类分开，放在不同的部分下。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Most poorly configured landing pages serve two different purposes: they are the “goto” page for someone who is a user of your product or API, or they are the home page for a team. Don’t have the page serve both masters—it will become confusing. Create a separate “team page” as an internal page apart from the main landing page. What the team needs to know is often quite different than what a customer of your API needs to know.
</div></details>
大多数配置糟糕的引导页面有两个不同的目的:它们是产品或API用户的“转到”页面，或者是团队的主页。不要让页面同时服务于两个主页面——这会让人感到困惑。创建一个独立的“团队页面”作为内部页面，而不是主引导页面。团队需要知道的东西通常与API的客户需要知道的东西有很大的不同。

## 文档评审 Documentation Reviews

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
At Google, all code needs to be reviewed, and our code review process is well understood and accepted. In general, documentation also needs review (though this is less universally accepted). If you want to “test” whether your documentation works, you should generally have someone else review it. 
A technical document benefits from three different types of reviews, each emphasizing different aspects:
</div></details>

在谷歌，所有的代码都需要审查，我们的代码审查过程被很好地理解和接受。一般来说，文档也需要审查(尽管这不是普遍接受的)。如果你想“测试”你的文档是否有效，你通常应该让其他人来检查它。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

- A technical review, for accuracy. This review is usually done by a subject matter expert, often another member of your team. Often, this is part of a code review itself. 
- An audience review, for clarity. This is usually someone unfamiliar with the domain. This might be someone new to your team or a customer of your API. 
- A writing review, for consistency. This is often a technical writer or volunteer.
</div></details>

技术文档受益于三种不同类型的审查，每一种都强调不同的方面:

- 技术审查，以确保准确性。审查通常由主题专家完成，通常是团队的另一名成员。通常，这是代码审查本身的一部分。
- 观众评论，为了清晰。这通常是不熟悉该领域的人。这可能是您团队的新成员，也可能是API的客户。
- 写作回顾，保持一致性。这通常是一个技术作家或志愿者。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Of course, some of these lines are sometimes blurred, but if your document is high profile or might end up being externally published, you probably want to ensure that it receives more types of reviews. (We’ve used a similar review process for this book.) Any document tends to benefit from the aforementioned reviews, even if some of those reviews are ad hoc. That said, even getting one reviewer to review your text is preferable to having no one review it.
</div></details>

当然，其中的一些界线有时是模糊的，但是如果您的文档是高调的，或者最终可能会被外部发布，那么您可能希望确保它接受更多类型的审查。(我们对这本书使用了类似的审查过程。)任何文档都倾向于从上述的审查中受益，即使其中一些审查是临时的。也就是说，即使只有一个审稿人来审阅你的文章，也比没有人来审阅要好。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Importantly, if documentation is tied into the engineering workflow, it will often improve over time. Most documents at Google now implicitly go through an audience review because at some point, their audience will be using them, and hopefully letting you know when they aren’t working (via bugs or other forms of feedback).
</div></details>
重要的是，如果文档被绑定到工程工作流中，它通常会随着时间的推移而改进。谷歌上的大多数文档现在都隐含地要经过用户审查，因为在某种程度上，他们的用户会使用这些文档，并希望通过错误或其他形式的反馈让您知道它们何时不能工作。


<details> <summary>origin</summary>
<div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

**<p align='center'>Case Study: The Developer Guide Library</p>**

As mentioned earlier, there were problems associated with having most (almost all) engineering documentation contained within a shared wiki: little ownership of important documentation, competing documentation, obsolete information, and difficulty in filing bugs or issues with documentation. But this problem was not seen in some documents: the Google C++ style guide was owned by a select group of senior engineers (style arbiters) who managed it. The document was kept in good shape because certain people cared about it. They implicitly owned that document. The document was also canonical: there was only one C++ style guide.

As previously mentioned, documentation that sits directly within source code is one way to promote the establishment of canonical documents; if the documentation sits alongside the source code, it should usually be the most applicable (hopefully). At Google, each API usually has a separate g3doc directory where such documents live (written as Markdown files and readable within our Code Search browser). Having the documentation exist alongside the source code not only establishes de facto ownership, it makes the documentation seem more wholly “part” of the code.

Some documentation sets, however, cannot exist very logically within source code. A “C++ developer guide” for Googlers, for example, has no obvious place to sit within the source code. There is no master “C++” directory where people will look for such information. In this case (and others that crossed API boundaries), it became useful to create standalone documentation sets in their own depot. Many of these culled together associated existing documents into a common set, with common navigation and look-and-feel. Such documents were noted as “Developer Guides” and, like the code in the codebase, were under source control in a specific documentation depot, with this depot organized by topic rather than API. Often, technical writers managed these develop

Over time, these developer guides became canonical. Users who wrote competing or supplementary documents became amenable to adding their documents to the canonical document set after it was established, and then deprecating their competing documents. Eventually, the C++ style guide became part of a larger “C++ Developer Guide.” As the documentation set became more comprehensive and more authoritative, its quality also improved. Engineers began logging bugs because they knew someone was maintaining these documents. Because the documents were locked down under source control, with proper owners, engineers also began sending changelists directly to the technical writers.

The introduction of go/ links (see Chapter 3) allowed most documents to, in effect, more easily establish themselves as canonical on any given topic. Our C++ Developer Guide became established at “go/cpp,” for example. With better internal search, go/ links, and the integration of multiple documents into a common documentation set, such canonical documentation sets became more authoritative and robust over time.
</div></details>


<div style='border:1px solid #EEE pading:10px'>

**<p align='center'>案例研究:开发人员指南库(The Developer Guide Library)</p>**

正如前面提到的，将大多数(几乎所有)工程文档包含在一个共享的wiki中会带来一些问题:重要文档的所有权很小，相互竞争的文档，过时的信息，以及难以将bug或问题归档到文档中。但是这个问题并没有出现在一些文档中:谷歌c++风格指南是由一组高级工程师(风格仲裁人)管理的。这份文件保存得很好，因为有些人关心它。他们暗中拥有那份文件。该文档也是规范的:只有一个c++风格指南。

如前所述，直接位于源代码中的文档是促进规范文档的建立的一种方法;如果文档与源代码放在一起，那么它通常应该是最适用的(希望如此)。在谷歌，每个API通常都有一个单独的g3doc目录存放这些文档(以Markdown文件的形式编写，在我们的代码搜索浏览器中可读)。将文档与源代码放在一起不仅建立了事实上的所有权，而且使文档看起来更像是代码的“一部分”。

然而，有些文档集不能在源代码中很有逻辑地存在。例如，面向谷歌人的“c++开发人员指南”在源代码中没有明显的位置。没有“c++”主目录供人们查找此类信息。在这种情况下(以及其他跨越API边界的情况)，在它们自己的存储库中创建独立的文档集变得很有用。其中许多都将现有文档关联到一个公共集，具有通用的导航和外观。这些文档被标记为“开发人员指南”，就像代码库中的代码一样，在一个特定的文档库中处于源代码控制之下，这个文档库是按主题而不是API组织的。通常，技术写手负责这些开发

随着时间的推移，这些开发人员指南成为规范。编写竞争文档或补充文档的用户在建立规范文档集后，愿意将其文档添加到规范文档集，然后弃用其竞争文档。最终，c++风格指南成为了更大的“c++开发人员指南”的一部分。随着文档集变得更加全面和权威，其质量也得到了提高。工程师开始记录错误，因为他们知道有人在维护这些文档。由于文档在源代码控制下被锁定，有了合适的所有者，工程师也开始直接向技术作者发送变更列表。

``go/links``的引入(见第3章)实际上让大多数文档更容易成为任何给定主题的规范。例如，我们的c++开发者指南是在“go/cpp”建立的。通过更好的内部搜索、go/链接以及将多个文档集成到公共文档集中，这样的规范文档集随着时间的推移变得更加权威和健壮。
</div>

### 文档的哲学 Documentation Philosophy

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Caveat: the following section is more of a treatise on technical writing best practices (and personal opinion) than of “how Google does it.” Consider it optional for software engineers to fully grasp, though understanding these concepts will likely allow you to more easily write technical information.
</div></details>

注意:以下部分更多的是关于技术写作最佳实践(和个人观点)的论述，而不是关于“谷歌是如何做到的”。对于软件工程师来说，完全掌握这些概念是可选的，不过理解这些概念可能会让您更容易地编写技术信息。

### WHO, WHAT, WHEN, WHERE, and WHY

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Most technical documentation answers a “HOW” question. How does this work? How do I program to this API? How do I set up this server? As a result, there’s a tendency for software engineers to jump straight into the “HOW” on any given document and ignore the other questions associated with it: the WHO, WHAT, WHEN, WHERE, and WHY. It’s true that none of those are generally as important as the HOW—a design document is an exception because an equivalent aspect is often the WHY—but without a proper framing of technical documentation, documents end up confusing. Try to address the other questions in the first two paragraphs of any document: 
- WHO was discussed previously: that’s the audience. But sometimes you also need to explicitly call out and address the audience in a document. Example: “This document is for new engineers on the Secret Wizard project.” 
- WHAT identifies the purpose of this document: “This document is a tutorial designed to start a Frobber server in a test environment.” Sometimes, merely writing the WHAT helps you frame the document appropriately. If you start adding information that isn’t applicable to the WHAT, you might want to move that information into a separate document. 
- WHEN identifies when this document was created, reviewed, or updated. Documents in source code have this date noted implicitly, and some other publishing schemes automate this as well. But, if not, make sure to note the date on which the document was written (or last revised) on the document itself. 
- WHERE is often implicit as well, but decide where the document should live. Usually, the preference should be under some sort of version control, ideally with the source code it documents. But other formats work for different purposes as well. At Google, we often use Google Docs for easy collaboration, particularly on design issues. At some point, however, any shared document becomes less of a discussion and more of a stable historical record. At that point, move it to someplace more permanent, with clear ownership, version control, and responsibility. 
- WHY sets up the purpose for the document. Summarize what you expect someone to take away from the document after reading it. A good rule of thumb is to establish the WHY in the introduction to a document. When you write the summary, verify whether you’ve met your original expectations (and revise accordingly).
</div></details>

大多数技术文档都回答了“如何”的问题。这是如何工作的?我如何对这个API编程?我如何设置这个服务器?因此，软件工程师倾向于直接跳到任何给定文档的“如何”，而忽略与之相关的其他问题:WHO、WHAT、WHEN、WHERE和WHY。的确，这些内容都不如“如何”重要——设计文档是个例外，因为与之相当的内容通常是“为什么”——但如果没有适当的技术文档框架，文档最终会令人困惑。试着在任何文件的前两段中解决其他问题:
- 世卫组织之前讨论过:这就是受众。但是，有时您还需要在文档中明确地向听众发出呼吁。例如:“本文档适用于Secret Wizard项目的新工程师。”
- 确定本文档的目的:“本文档是一个教程，旨在在测试环境中启动Frobber服务器。”有时，仅仅编写WHAT就可以帮助您适当地构建文档。如果开始添加不适用于WHAT的信息，则可能希望将该信息移动到单独的文档中。
- WHEN标识何时创建、审查或更新文档。源代码中的文档隐式地标注了这个日期，其他一些发布方案也自动标注了这个日期。但是，如果没有的话，一定要在文档本身上注明文档编写(或最后修改)的日期。
- WHERE通常也是隐式的，但决定文档应该放在哪里。通常，首选项应该处于某种版本控制之下，理想情况下是与它所记录的源代码一起。但其他格式也适用于不同的目的。在谷歌，我们经常使用谷歌文档来轻松协作，特别是在设计问题上。然而，在某种程度上，任何共享文档都不再是讨论，而是稳定的历史记录。在这一点上，将它移到更永久的地方，具有明确的所有权、版本控制和责任。
- 为什么设定文件的目的。在阅读完文档后，总结你希望别人从文档中获得的内容。一个好的经验法则是在文档的介绍中建立WHY。当你写总结的时候，验证一下你是否达到了最初的期望(并相应地修改)。

### The Beginning, Middle, and End

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
All documents—indeed, all parts of documents—have a beginning, middle, and end. Although it sounds amazingly silly, most documents should often have, at a minimum, those three sections. A document with only one section has only one thing to say, and very few documents have only one thing to say. Don’t be afraid to add sections to your document; they break up the flow into logical pieces and provide readers with a roadmap of what the document covers. 

Even the simplest document usually has more than one thing to say. Our popular “C++ Tips of the Week” have traditionally been very short, focusing on one small piece of advice. However, even here, having sections helps. Traditionally, the first section denotes the problem, the middle section goes through the recommended solutions, and the conclusion summarizes the takeaways. Had the document consisted of only one section, some readers would doubtless have difficulty teasing out the important points. 

Most engineers loathe redundancy, and with good reason. But in documentation, redundancy is often useful. An important point buried within a wall of text can be difficult to remember or tease out. On the other hand, placing that point at a more prominent location early can lose context provided later on. Usually, the solution is to introduce and summarize the point within an introductory paragraph, and then use the rest of the section to make your case in a more detailed fashion. In this case, redundancy helps the reader understand the importance of what is being stated.
</div></details>

所有文档——实际上，文档的所有部分——都有开头、中间和结尾。尽管这听起来非常愚蠢，但大多数文档通常至少应该有这三个部分。只有一个部分的文档只有一件事要说，很少有文档只有一件事要说。不要害怕在文档中添加章节;它们将流程分解成逻辑部分，并为读者提供文档涵盖内容的路线图。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Even the simplest document usually has more than one thing to say. Our popular “C++ Tips of the Week” have traditionally been very short, focusing on one small piece of advice. However, even here, having sections helps. Traditionally, the first section denotes the problem, the middle section goes through the recommended solutions, and the conclusion summarizes the takeaways. Had the document consisted of only one section, some readers would doubtless have difficulty teasing out the important points. 
</div></details>
即使是最简单的文件通常也有不止一件事要说。我们流行的“每周c++技巧”通常都很短，只关注一条小建议。然而，即使在这里，分段也有帮助。传统上，第一部分表示问题，中间部分介绍推荐的解决方案，而结论则是总结结论。如果这份文件只包含一个部分，一些读者无疑将难以梳理出重点。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Most engineers loathe redundancy, and with good reason. But in documentation, redundancy is often useful. An important point buried within a wall of text can be difficult to remember or tease out. On the other hand, placing that point at a more prominent location early can lose context provided later on. Usually, the solution is to introduce and summarize the point within an introductory paragraph, and then use the rest of the section to make your case in a more detailed fashion. In this case, redundancy helps the reader understand the importance of what is being stated.
</div></details>
大多数工程师讨厌冗余，这是有充分理由的。但是在文档中，冗余通常是有用的。在一堆文字中，一个重要的知识点很难记住或梳理出来。另一方面，过早地将该点放置在更突出的位置可能会失去随后提供的上下文。通常，解决方案是在引言段中介绍和总结要点，然后使用该部分的其余部分以更详细的方式说明你的情况。在这种情况下，冗余可以帮助读者理解所陈述内容的重要性。

### 优秀文档的要素 The Parameters of Good Documentation

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
There are usually three aspects of good documentation: completeness, accuracy, and clarity. You rarely get all three within the same document; as you try to make a document more “complete,” for example, clarity can begin to suffer. If you try to document every possible use case of an API, you might end up with an incomprehensible mess. For programming languages, being completely accurate in all cases (and documenting all possible side effects) can also affect clarity. For other documents, trying to be clear about a complicated topic can subtly affect the accuracy of the document; you might decide to ignore some rare side effects in a conceptual document, for example, because the point of the document is to familiarize someone with the usage of an API, not provide a dogmatic overview of all intended behavior.
</div></details>

好的文档通常有三个方面:完整性、准确性和清晰度。你很少会在同一个文档中同时看到这三种情况;例如，当你试图使一份文件更“完整”时，清晰度就会开始受到影响。如果您试图记录API的每一个可能的用例，您可能会以难以理解的混乱而告终。对于编程语言来说，在所有情况下完全准确(以及记录所有可能的副作用)也会影响清晰度。对于其他文档，试图清楚地了解一个复杂的主题可能会微妙地影响文档的准确性;例如，您可能决定忽略概念性文档中一些罕见的副作用，因为该文档的重点是让人们熟悉API的用法，而不是提供所有预期行为的教条概述。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In each case, a “good document” is defined as the document that is doing its intended job. As a result, you rarely want a document doing more than one job. For each document (and for each document type), decide on its focus and adjust the writing appropriately. Writing a conceptual document? You probably don’t need to cover every part of the API. Writing a reference? You probably want this complete, but perhaps must sacrifice some clarity. Writing a landing page? Focus on organization and keep discussion to a minimum. All of this adds up to quality, which, admittedly, is stubbornly difficult to accurately measure
</div></details>

在每种情况下，“良好的文档”都被定义为执行其预期工作的文档。因此，您很少希望文档执行多个任务。对于每个文档(以及每种文档类型)，确定其重点并适当调整写作。写一个概念性的文档?您可能不需要涵盖API的每个部分。写一个参考?您可能想要完整，但可能必须牺牲一些清晰度。写登陆页面?注重组织，尽量减少讨论。所有这些都导致了质量的提高，不可否认，质量是很难精确测量的

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
How can you quickly improve the quality of a document? Focus on the needs of the audience. Often, less is more. For example, one mistake engineers often make is adding design decisions or implementation details to an API document. Much like you should ideally separate the interface from an implementation within a welldesigned API, you should avoid discussing design decisions in an API document. Users don’t need to know this information. Instead, put those decisions in a specialized document for that purpose (usually a design doc).
</div></details>
如何快速提高文档的质量?关注观众的需求。通常，少即是多。例如，工程师经常犯的一个错误是向API文档添加设计决策或实现细节。就像在设计良好的API中理想地将接口与实现分离一样，也应该避免在API文档中讨论设计决策。用户不需要知道这些信息。相反地，你应该将这些决定放在专门的文件中(通常是设计文件)。

### 失效的文档 Deprecating Documents

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Just like old code can cause problems, so can old documents. Over time, documents become stale, obsolete, or (often) abandoned. Try as much as possible to avoid abandoned documents, but when a document no longer serves any purpose, either remove it or identify it as obsolete (and, if available, indicate where to go for new information). Even for unowned documents, someone adding a note that “This no longer works!” is more helpful than saying nothing and leaving something that seems authoritative but no longer works. 
</div></details>

就像旧代码会导致问题一样，旧文档也会导致问题。随着时间的推移，文档变得陈旧、过时，或者(通常)被抛弃。尽量避免丢弃文档，但是当文档不再起任何作用时，要么将其删除，要么将其标识为过时的(如果可用，则指出从哪里获取新信息)。甚至对于无主文档，也有人添加了一条注释:“这不再有效了!”这比什么都不说，留下一些看似权威但不再有效的内容更有帮助。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
At Google, we often attach “freshness dates” to documentation. Such documents note the last time a document was reviewed, and metadata in the documentation set will send email reminders when the document hasn’t been touched in, for example, three months. Such freshness dates, as shown in the following example—and tracking your documents as bugs—can help make a documentation set easier to maintain over time, which is the main concern for a document:
</div></details>
在谷歌，我们经常在文件中附上“新鲜度日期”。这类文档会记录文档的最后一次审阅时间，文档集中的元数据会在文档没有被修改(例如三个月)时发送电子邮件提醒。如下例所示，这种新鲜度日期——以及跟踪文档中的bug——可以帮助文档集更容易维护，这是文档的主要关注点:

```
<!--*
# Document freshness: For more information, see go/fresh-source.
freshness: { owner: `username` reviewed: '2019-02-27' }
*-->
```

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Users who own such a document have an incentive to keep that freshness date current (and if the document is under source control, that requires a code review). As a result, it’s a low-cost means to ensure that a document is looked over from time to time. At Google, we found that including the owner of a document in this freshness date within the document itself with a byline of “Last reviewed by...” led to increased adoption as well.
</div></details>

拥有此类文档的用户有动力保持最新的更新日期(如果文档处于源代码控制之下，则需要进行代码审查)。因此，它是一种低成本的方法，以确保不时地查看文档。在谷歌，我们发现在这个最新日期的文档中包含文档的所有者，署名为“Last review by…”这也导致了拥有者数量的增加。

## 什么时候需要技术写手 When Do You Need Technical Writers?

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
When Google was young and growing, there weren’t enough technical writers in software engineering. (That’s still the case.) Those projects deemed important tended to receive a technical writer, regardless of whether that team really needed one. The idea was that the writer could relieve the team of some of the burden of writing and maintaining documents and (theoretically) allow the important project to achieve greater velocity. This turned out to be a bad assumption.
</div></details>

当谷歌还很年轻和成长的时候，软件工程领域没有足够的技术写手。(现在仍然如此。)那些被认为重要的项目往往会接收技术写手，而不管这个团队是否真的需要。这个想法是，作者可以减轻团队的一些编写和维护文档的负担，并且(理论上)允许重要的项目实现更大的速度。事实证明，这是一个糟糕的假设。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
We learned that most engineering teams can write documentation for themselves (their team) perfectly fine; it’s only when they are writing documents for another audience that they tend to need help because it’s difficult to write to another audience. The feedback loop within your team regarding documents is more immediate, the domain knowledge and assumptions are clearer, and the perceived needs are more obvious. Of course, a technical writer can often do a better job with grammar and organization, but supporting a single team isn’t the best use of a limited and specialized resource; it doesn’t scale. It introduced a perverse incentive: become an important project and your software engineers won’t need to write documents. Discouraging engineers from writing documents turns out to be the opposite of what you want to do.
</div></details>
我们了解到大多数工程团队都可以很好地为自己(他们的团队)编写文档;只有当他们为另一个读者编写文档时，他们才需要帮助，因为为另一个读者编写文档很困难。团队中关于文档的反馈循环更加直接，领域知识和假设更加清晰，感知到的需求更加明显。当然，技术写手通常可以在语法和组织方面做得更好，但支持单个团队并不是使用有限和专业资源的最佳方式;它没有规模。它引入了一种反常的激励:成为一个重要的项目，你的软件工程师就不需要编写文档了。阻止工程师编写文档与您想要做的恰恰相反。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Because they are a limited resource, technical writers should generally focus on tasks that software engineers don’t need to do as part of their normal duties. Usually, this involves writing documents that cross API boundaries. Project Foo might clearly know what documentation Project Foo needs, but it probably has a less clear idea what Project Bar needs. A technical writer is better able to stand in as a person unfamiliar with the domain. In fact, it’s one of their critical roles: to challenge the assumptions your team makes about the utility of your project. It’s one of the reasons why many, if not most, software engineering technical writers tend to focus on this specific type of API documentation.
</div></details>
因为他们是一个有限的资源，技术作家通常应该关注的任务，软件工程师不需要做他们的正常职责的一部分。通常，这涉及编写跨API边界的文档。项目Foo可能清楚地知道项目Foo需要什么文档，但它可能不太清楚项目Bar需要什么。一个技术写手更能作为一个不熟悉这个领域的人。事实上，这是他们的关键角色之一:挑战团队对项目实用性的假设。这就是为什么很多(如果不是大多数的话)软件工程技术作者倾向于关注这种特定类型的API文档的原因之一。

## 结论 Conclusion

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Google has made good strides in addressing documentation quality over the past decade, but to be frank, documentation at Google is not yet a first-class citizen. For comparison, engineers have gradually accepted that testing is necessary for any code change, no matter how small. As well, testing tooling is robust, varied and plugged into an engineering workflow at various points. Documentation is not ingrained at nearly the same level.
</div></details>

在过去的十年中，谷歌在解决文档质量问题上取得了长足的进步，但坦率地说，谷歌的文档还不是一等公民。为了进行比较，工程师们已经逐渐接受了测试对于任何代码更改都是必要的，无论更改有多小。此外，测试工具是健壮的、多样的，并且可以在不同的位置插入到工程工作流程中。文档并不是在同一个层次上根深蒂固的。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
To be fair, there’s not necessarily the same need to address documentation as with testing. Tests can be made atomic (unit tests) and can follow prescribed form and function. Documents, for the most part, cannot. Tests can be automated, and schemes to automate documentation are often lacking. Documents are necessarily subjective; the quality of the document is measured not by the writer, but by the reader, and often quite asynchronously. That said, there is a recognition that documentation is important, and processes around document development are improving. In this author’s opinion, the quality of documentation at Google is better than in most software engineering shops.

</div></details>
公平地说，对文档的处理不一定和对测试的处理一样。测试可以是原子的(单元测试)，并且可以遵循规定的形式和功能。在大多数情况下，文档不能。测试可以自动化，而自动化文档的方案常常缺乏。文件必然是主观的;文档的质量不是由作者来衡量的，而是由读者来衡量的，而且通常是相当异步的。也就是说，人们认识到文档是重要的，围绕文档开发的流程正在改进。在作者看来，谷歌的文档质量比大多数软件工程商店的文档质量要好。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
To change the quality of engineering documentation, engineers—and the entire engineering organization—need to accept that they are both the problem and the solution. Rather than throw up their hands at the state of documentation, they need to realize that producing quality documentation is part of their job and saves them time and effort in the long run. For any piece of code that you expect to live more than a few months, the extra cycles you put in documenting that code will not only help others, it will help you maintain that code as well.
</div></details>
为了改变工程文档的质量，工程师——以及整个工程组织——需要接受他们既是问题又是解决方案。他们需要意识到，生成高质量的文档是他们工作的一部分，从长远来看，这可以节省他们的时间和精力，而不是放弃文档的状态。对于任何您希望使用几个月以上的代码段，您在编写代码文档时所投入的额外周期不仅会帮助其他人，也会帮助您维护该代码。

## TL;DRs

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2"> 

- Documentation is hugely important over time and scale.
- Documentation changes should leverage the existing developer workflow.
- Keep documents focused on one purpose.
- Write for your audience, not yourself.
</div></details>

- 文档在时间和规模上都非常重要。
- 文档变更应利用现有的开发人员工作流程。
- 让文件集中在一个目的上。
- 为你的读者而写，而不是你自己。


[^1]:OK, you will need to maintain it and revise it occasionally.

[^2]:English is still the primary language for most programmers, and most technical documentation for programmers relies on an understanding of English.
