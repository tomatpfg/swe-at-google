
**<p align="right">第12章</p>**
-- -
**<p align="right">单元测试 Unit Testing</p>**

*<p align="right">作者：  Erik Kuefler</p>*
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

## 可维护的重要性 The Importance of Maintainability


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Imagine this scenario: Mary wants to add a simple new feature to the product and is able to implement it quickly, perhaps requiring only a couple dozen lines of code. But when she goes to check in her change, she gets a screen full of errors back from the automated testing system. She spends the rest of the day going through those failures one by one. In each case, the change introduced no actual bug, but broke some of the assumptions that the test made about the internal structure of the code, requiring those tests to be updated. Often, she has difficulty figuring out what the tests were trying to do in the first place, and the hacks she adds to fix them make those tests even more difficult to understand in the future. Ultimately, what should have been a quick job ends up taking hours or even days of busywork, killing Mary’s productivity and sapping her morale.
</div></details>

想象一下这样的场景:Mary希望向产品添加一个简单的新特性，并且能够快速实现它，也许只需要几十行代码。但是当她检查更改时，自动测试系统会返回一个充满错误的屏幕。剩下的时间里，她一个接一个地经历这些失败。在每种情况下，更改都没有引入实际的bug，但打破了测试对代码内部结构所做的一些假设，要求更新这些测试。通常，她在一开始就很难弄清楚这些测试试图做什么，而她为修复这些测试而添加的hack使得这些测试在未来更加难以理解。最终，本应很快完成的工作却花费了玛丽数小时甚至数天的忙碌工作，扼杀了她的工作效率，挫伤了她的士气。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Here, testing had the opposite of its intended effect by draining productivity rather than improving it while not meaningfully increasing the quality of the code under test. This scenario is far too common, and Google engineers struggle with it every day. There’s no magic bullet, but many engineers at Google have been working to develop sets of patterns and practices to alleviate these problems, which we encourage the rest of the company to follow.

</div></details>
在这里，测试产生了相反的效果，降低了生产率，而不是提高了生产率，同时也没有显著地提高被测试代码的质量。这种场景太常见了，谷歌工程师每天都在与之斗争。没有什么灵丹妙药，但谷歌的许多工程师一直在努力开发一套模式和实践来缓解这些问题，我们鼓励公司的其他人员效仿。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The problems Mary ran into weren’t her fault, and there was nothing she could have done to avoid them: bad tests must be fixed before they are checked in, lest they impose a drag on future engineers. Broadly speaking, the issues she encountered fall into two categories. First, the tests she was working with were brittle: they broke in response to a harmless and unrelated change that introduced no real bugs. Second, the tests were unclear: after they were failing, it was difficult to determine what was wrong, how to fix it, and what those tests were supposed to be doing in the first place.

</div></details>
Mary遇到的问题并不是她的错，她也没有办法避免这些问题:糟糕的测试必须在检查之前进行修复，以免给未来的工程师带来麻烦。一般来说，她遇到的问题可以分为两类。首先，她正在进行的测试是脆弱的:它们在对一个无害且不相关的更改做出反应时中断了，而这个更改并没有引入真正的bug。其次，这些测试并不明确:在它们失败后，很难确定哪里出了问题，如何修复，以及这些测试最初应该做什么。

## 防止脆性测试 Preventing Brittle Tests

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

As just defined, a brittle test is one that fails in the face of an unrelated change to production code that does not introduce any real bugs[^1]. Such tests must be diagnosed and fixed by engineers as part of their work. In small codebases with only a few engineers, having to tweak a few tests for every change might not be a big problem. But if a team regularly writes brittle tests, test maintenance will inevitably consume a larger and larger proportion of the team’s time as they are forced to comb through an increasing number of failures in an ever-growing test suite. If a set of tests needs to be manually tweaked by engineers for each change, calling it an “automated test suite” is a bit of a stretch!
</div></details>


正如刚刚定义的那样，脆弱测试是在对生产代码进行不相关且不会引入任何真正错误的更改时失败的测试[^1]。工程师必须在工作中诊断和修复此类测试。在只有少数工程师的小型代码库中，每次更改都必须调整一些测试可能不是什么大问题。但是如果一个团队经常编写脆弱的测试，测试维护将不可避免地消耗团队越来越多的时间，因为他们被迫在不断增长的测试套件中梳理越来越多的故障。如果工程师每次更改都需要手动调整一组测试，那么将其称为“自动化测试套件”有点牵强！

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Brittle tests cause pain in codebases of any size, but they become particularly acute at Google’s scale. An individual engineer might easily run thousands of tests in a single day during the course of their work, and a single large-scale change (see Chapter 22) can trigger hundreds of thousands of tests. At this scale, spurious breakages that affect even a small percentage of tests can waste huge amounts of engineering time. Teams at Google vary quite a bit in terms of how brittle their test suites are, but we’ve identified a few practices and patterns that tend to make tests more robust to change.

</div></details>
脆弱的测试会给任何规模的代码库带来痛苦，但在 Google 的规模上它们变得特别严重。单个工程师在其工作过程中可能在一天之内轻松运行数千次测试，而一次大规模更改（参见第 22 章）可能会触发数十万次测试。在这种规模下，即使是影响一小部分测试的虚假破损也会浪费大量的工程时间。 Google 的团队在测试套件的脆弱程度方面存在很大差异，但我们已经确定了一些实践和模式，这些实践和模式往往会使测试更加健壮，易于更改。

### 为不变的测试而努力 Strive for Unchanging Tests

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Before talking about patterns for avoiding brittle tests, we need to answer a question: just how often should we expect to need to change a test after writing it? Any time spent updating old tests is time that can’t be spent on more valuable work. Therefore, the ideal test is unchanging: after it’s written, it never needs to change unless the requirements of the system under test change.

What does this look like in practice? We need to think about the kinds of changes that engineers make to production code and how we should expect tests to respond to those changes. Fundamentally, there are four kinds of changes:
</div></details>

在讨论避免脆性测试的模式之前，我们需要回答一个问题:在编写测试之后，我们应该期望多长时间更改一次测试?任何花在更新旧测试上的时间都不能用于更有价值的工作。因此，理想的测试是不变的:在编写之后，它永远不需要更改，除非被测系统的需求发生变化。

这在实践中是什么样的呢?我们需要考虑工程师对产品代码所做的各种更改，以及我们应该如何期望测试对这些更改作出响应。从根本上说，有四种变化:


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Pure refactorings

When an engineer refactors the internals of a system without modifying its interface, whether for performance, clarity, or any other reason, the system’s tests shouldn’t need to change. The role of tests in this case is to ensure that the refactoring didn’t change the system’s behavior. Tests that need to be changed during a refactoring indicate that either the change is affecting the system’s behavior and isn’t a pure refactoring, or that the tests were not written at an appropriate level of abstraction. Google’s reliance on large-scale changes (described in Chapter 22) to do such refactorings makes this case particularly important for us.
</div></details>

*纯粹的重构*

<div style="margin-left:1em;margin-bottom:1em">
当工程师重构系统的内部而不修改其接口时，无论是出于性能、清晰度还是任何其他原因，系统的测试应该不需要更改。在这种情况下，测试的作用是确保重构不会改变系统的行为。在重构期间需要更改的测试表明，要么更改正在影响系统的行为，而不是纯粹的重构，要么测试没有在适当的抽象级别上编写。谷歌依赖于大规模的更改(在第22章中描述)来进行重构，这使得这种情况对我们来说特别重要。
</div>



<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

*New features*

<div style="margin-left:1em;margin-bottom:1em">
When an engineer adds new features or behaviors to an existing system, the system’s existing behaviors should remain unaffected. The engineer must write new tests to cover the new behaviors, but they shouldn’t need to change any existing tests. As with refactorings, a change to existing tests when adding new features suggest unintended consequences of that feature or inappropriate tests.
</div>
</div></details>

*新功能*

<div style="margin-left:1em;margin-bottom:1em">
当工程师向现有系统添加新特性或行为时，系统的现有行为应该不受影响。工程师必须编写新的测试来覆盖新的行为，但是他们不应该需要更改任何现有的测试。与重构一样，添加新特性时对现有测试的更改会导致该特性或不适当的测试产生意想不到的后果。
</div>


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

*Bug fixes*

<div style="margin-left:1em;margin-bottom:1em">
Fixing a bug is much like adding a new feature: the presence of the bug suggests that a case was missing from the initial test suite, and the bug fix should include that missing test case. Again, bug fixes typically shouldn’t require updates to existing tests.
</div>
</div></details>

*bug修复*

<div style="margin-left:1em;margin-bottom:1em">
修复一个bug就像添加一个新特性:bug的出现表明在最初的测试套件中丢失了一个用例，而修复的bug应该包括这个丢失的测试用例。同样，bug修复通常不需要对现有测试进行更新。
</div>


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

*Behavior changes*

<div style="margin-left:1em;margin-bottom:1em">
Changing a system’s existing behavior is the one case when we expect to have to make updates to the system’s existing tests. Note that such changes tend to be significantly more expensive than the other three types. A system’s users are likely to rely on its current behavior, and changes to that behavior require coordination with those users to avoid confusion or breakages. Changing a test in this case indicates that we’re breaking an explicit contract of the system, whereas changes in the previous cases indicate that we’re breaking an unintended contract. Lowlevel libraries will often invest significant effort in avoiding the need to ever make a behavior change so as not to break their users.
</div>
</div></details>

*行为变化*

<div style="margin-left:1em;margin-bottom:1em">
当我们希望更新系统的现有测试时，更改系统的现有行为是一种情况。请注意，这种更改往往比其他三种类型的更改要昂贵得多。系统的用户很可能依赖于其当前的行为，而对该行为的更改需要与这些用户进行协调，以避免混乱或破坏。在这种情况下，更改测试表明我们正在破坏一个明确的系统契约，而在以前的情况下更改表明我们正在破坏一个非预期的契约。低级库通常会投入大量精力来避免进行行为更改，以免破坏用户。
</div>

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The takeaway is that after you write a test, you shouldn’t need to touch that test again as you refactor the system, fix bugs, or add new features. This understanding is what makes it possible to work with a system at scale: expanding it requires writing only a small number of new tests related to the change you’re making rather than potentially having to touch every test that has ever been written against the system. Only breaking changes in a system’s behavior should require going back to change its tests, and in such situations, the cost of updating those tests tends to be small relative to the cost of updating all of the system’s users.
</div></details>

结论是，在编写测试之后，在重构系统、修复bug或添加新特性时，不应该再碰那个测试。这种理解使大规模的系统工作成为可能:扩展它只需要编写少量与您所做的更改相关的新测试，而不是潜在地必须触及每一个针对系统编写的测试。只有破坏系统行为的更改才需要返回更改它的测试，在这种情况下，更新这些测试的成本相对于更新所有系统用户的成本来说是很小的。


### 通过公共API测试 Test via Public APIs

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Now that we understand our goal, let’s look at some practices for making sure that tests don’t need to change unless the requirements of the system being tested change. By far the most important way to ensure this is to write tests that invoke the system being tested in the same way its users would; that is, make calls against its public API rather than its implementation details. If tests work the same way as the system’s users, by definition, change that breaks a test might also break a user. As an additional bonus, such tests can serve as useful examples and documentation for users.
</div></details>
现在我们已经理解了我们的目标，让我们看看一些实践，以确保测试不需要更改，除非被测试系统的需求发生更改。到目前为止，确保这一点的最重要的方法是编写测试，以与用户相同的方式调用被测试的系统;也就是说，对其公共API而不是其实现细节进行调用。如果测试的工作方式与系统用户相同，那么根据定义，破坏测试的更改也可能破坏用户。另外，这样的测试可以作为有用的示例和文档提供给用户。


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Consider Example 12-1, which validates a transaction and saves it to a database
</div></details>
考虑示例12-1，它验证事务并将其保存到数据库
```Java
public void processTransaction(Transaction transaction) {
 if (isValid(transaction)) {
 saveToDatabase(transaction);
 }
}
private boolean isValid(Transaction t) {
 return t.getAmount() < t.getSender().getBalance();
}
private void saveToDatabase(Transaction t) {
 String s = t.getSender() + "," + t.getRecipient() + "," + t.getAmount();
 database.put(t.getId(), s);
}
public void setAccountBalance(String accountName, int balance) {
 // Write the balance to the database directly
}
public void getAccountBalance(String accountName) {
 // Read transactions from the database to determine the account balance
}
A tempting way to test this code would be to remove the “private” visibility modifiers
and test the implementation logic directly, as demonstrated in Example 12-2.
Example 12-2. A naive test of a transaction API’s implementation
@Test
public void emptyAccountShouldNotBeValid() {
 assertThat(processor.isValid(newTransaction().setSender(EMPTY_ACCOUNT)))
 .isFalse();
}
@Test
public void shouldSaveSerializedData() {
 processor.saveToDatabase(newTransaction()
 .setId(123)
 .setSender("me")
 .setRecipient("you")
 .setAmount(100));
 assertThat(database.get(123)).isEqualTo("me,you,100");
}
```


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
A tempting way to test this code would be to remove the “private” visibility modifiers and test the implementation logic directly, as demonstrated in Example 12-2. 

Example 12-2. A naive test of a transaction API’s implementation
</div></details>

测试这段代码的一个诱人的方法是删除``private``可见性修饰符，并直接测试实现逻辑，如例12-2所示。

示例12-2 对事务API实现的简单测试
```Java
@Test
public void emptyAccountShouldNotBeValid() {
 assertThat(processor.isValid(newTransaction().setSender(EMPTY_ACCOUNT)))
 .isFalse();
}
@Test
public void shouldSaveSerializedData() {
 processor.saveToDatabase(newTransaction()
 .setId(123)
 .setSender("me")
 .setRecipient("you")
 .setAmount(100));
 assertThat(database.get(123)).isEqualTo("me,you,100");
}
```


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

This test interacts with the transaction processor in a much different way than its real users would: it peers into the system’s internal state and calls methods that aren’t publicly exposed as part of the system’s API. As a result, the test is brittle, and almost any refactoring of the system under test (such as renaming its methods, factoring them out into a helper class, or changing the serialization format) would cause the test to break, even if such a change would be invisible to the class’s real users.

Instead, the same test coverage can be achieved by testing only against the class’s public API, as shown in Example 12-3.[^2]

Example 12-3. Testing the public API

</div></details>
此测试与事务处理器的交互方式与实际用户的交互方式非常不同:它查看系统的内部状态，并调用未作为系统API的一部分公开的方法。测试结果,是脆弱的,几乎所有的重构被测系统(如重命名它的方法,分解成一个助手类,或改变的序列化格式)会导致测试打破,即使这样的变化将无形的类的真实用户。

相反，同样的测试覆盖可以通过只针对类的公共API进行测试来实现，如例12-3所示。[^2]
例12-3.测试公共API

```Java
@Test
public void shouldTransferFunds() {
 processor.setAccountBalance("me", 150);
 processor.setAccountBalance("you", 20);
 processor.processTransaction(newTransaction()
 .setSender("me")
 .setRecipient("you")
 .setAmount(100));
 assertThat(processor.getAccountBalance("me")).isEqualTo(50);
 assertThat(processor.getAccountBalance("you")).isEqualTo(120);
}
@Test
public void shouldNotPerformInvalidTransactions() {
 processor.setAccountBalance("me", 50);
 processor.setAccountBalance("you", 20);
 processor.processTransaction(newTransaction()
 .setSender("me")
 .setRecipient("you")
 .setAmount(100));
 assertThat(processor.getAccountBalance("me")).isEqualTo(50);
```

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Tests using only public APIs are, by definition, accessing the system under test in the same manner that its users would. Such tests are more realistic and less brittle because they form explicit contracts: if such a test breaks, it implies that an existing user of the system will also be broken. Testing only these contracts means that you’re free to do whatever internal refactoring of the system you want without having to worry about making tedious changes to tests.
</div></details>

根据定义，仅使用公共api的测试以与用户相同的方式访问被测系统。这样的测试更现实，也更不脆弱，因为它们形成了明确的契约:如果这样的测试失败，就意味着系统的现有用户也会失败。只测试这些契约意味着您可以自由地对系统进行任何您想要的内部重构，而不必担心对测试进行乏味的更改。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
It’s not always clear what constitutes a “public API,” and the question really gets to the heart of what a “unit” is in unit testing. Units can be as small as an individual function or as broad as a set of several related packages/modules. When we say “public API” in this context, we’re really talking about the API exposed by that unit to third parties outside of the team that owns the code. This doesn’t always align with the notion of visibility provided by some programming languages; for example, classes in Java might define themselves as “public” to be accessible by other packages in the same unit but are not intended for use by other parties outside of the unit. Some languages like Python have no built-in notion of visibility (often relying on conventions like prefixing private method names with underscores), and build systems like Bazel can further restrict who is allowed to depend on APIs declared public by the programming language.
</div></details>

并不总是清楚什么构成了“公共API”，这个问题真正触及到单元测试中的“单元”的核心。单元可以小到一个单独的函数，也可以大到几个相关包/模块的集合。当我们在这个上下文中提到“公共API”时，我们实际上是在谈论由该单元向拥有代码的团队之外的第三方公开的API。这并不总是与一些编程语言提供的可见性概念相一致;例如，Java中的类可以将自己定义为“public”，以供同一单元中的其他包访问，但不打算供单元外的其他方使用。像Python这样的一些语言没有内置的可见性概念(通常依赖于一些约定，比如在私有方法名前加上下划线)，并且构建系统，比如Bazel，可以进一步限制谁可以依赖编程语言声明的公共api。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Defining an appropriate scope for a unit and hence what should be considered the public API is more art than science, but here are some rules of thumb:
</div></details>
为一个单元定义一个合适的范围，因此什么应该被认为是公共API更像是艺术而不是科学，但这里有一些经验法则:

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

- If a method or class exists only to support one or two other classes (i.e., it is a “helper class”), it probably shouldn’t be considered its own unit, and its functionality should be tested through those classes instead of directly.
</div></details>

•如果一个方法或类只支持一个或两个其他类(例如，它是一个“helper类”)，它可能不应该被认为是它自己的单元，它的功能应该通过这些类而不是直接测试。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

- If a package or class is designed to be accessible by anyone without having to consult with its owners, it almost certainly constitutes a unit that should be tested directly, where its tests access the unit in the same way that the users would.
</div></details>

- 如果一个包或类被设计成任何人都可以访问，而不需要与它的所有者协商，它几乎肯定会构成一个单元，应该直接测试，在这里，它的测试访问单元的方式与用户一样。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

- If a package or class can be accessed only by the people who own it, but it is designed to provide a general piece of functionality useful in a range of contexts (i.e., it is a “support library”), it should also be considered a unit and tested directly. This will usually create some redundancy in testing given that the support library’s code will be covered both by its own tests and the tests of its users. However, such redundancy can be valuable: without it, a gap in test coverage could be introduced if one of the library’s users (and its tests) were ever removed.
</div></details>

- 如果一个包或类只能被拥有它的人访问，但它被设计为在一系列上下文中提供一个通用的功能块(例如，它是一个“支持库”)，那么它也应该被视为一个单元并直接测试。这通常会在测试中产生一些冗余，因为支持库的代码将被它自己的测试和它的用户的测试覆盖。然而，这样的冗余是有价值的:如果没有冗余，当库的一个用户(及其测试)被删除时，可能会引入测试覆盖的缺口。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
At Google, we’ve found that engineers sometimes need to be persuaded that testing via public APIs is better than testing against implementation details. The reluctance is understandable because it’s often much easier to write tests focused on the piece of code you just wrote rather than figuring out how that code affects the system as a whole. Nevertheless, we have found it valuable to encourage such practices, as the extra upfront effort pays for itself many times over in reduced maintenance burden. Testing against public APIs won’t completely prevent brittleness, but it’s the most important thing you can do to ensure that your tests fail only in the event of meaningful changes to your system.
</div></details>
在谷歌，我们发现工程师有时需要被说服，通过公共api进行测试比根据实现细节进行测试更好。这种不情愿是可以理解的，因为通常更容易编写测试集中在您刚刚编写的代码片段上，而不是弄清楚代码如何影响整个系统。然而，我们发现鼓励这样的实践是很有价值的，因为额外的前期工作在减少维护负担方面是值得的。针对公共api进行测试并不能完全避免脆弱性，但这是确保测试仅在系统发生有意义的更改时才会失败的最重要的事情。

### 测试状态，而不是交互 Test State, Not Interactions

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Another way that tests commonly depend on implementation details involves not which methods of the system the test calls, but how the results of those calls are verified. In general, there are two ways to verify that a system under test behaves as expected. With state testing, you observe the system itself to see what it looks like after invoking with it. With interaction testing, you instead check that the system took an expected sequence of actions on its collaborators in response to invoking it. Many tests will perform a combination of state and interaction validation.
</div></details>
测试通常依赖于实现细节的另一种方式并不涉及测试调用系统的哪些方法，而是如何验证这些调用的结果。通常，有两种方法来验证被测试的系统是否按预期运行。使用状态测试，您可以观察系统本身，看看调用它之后它是什么样子。使用交互测试，您可以检查系统对其协作者采取预期的动作序列以响应调用它。许多测试将执行状态和交互验证的组合。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Interaction tests tend to be more brittle than state tests for the same reason that it’s more brittle to test a private method than to test a public method: interaction tests check how a system arrived at its result, whereas usually you should care only what the result is. Example 12-4 illustrates a test that uses a test double (explained further in Chapter 13) to verify how a system interacts with a database.
</div></details>
交互测试往往比状态测试更脆弱，这与测试私有方法比测试公共方法更脆弱的原因是一样的:交互测试检查系统是如何得到它的结果的，而通常您应该只关心结果是什么。例12-4演示了一个使用test double(在第13章中进一步解释)来验证系统如何与数据库交互的测试。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Example 12-4. A brittle interaction test
</div></details>
例12-4 脆性相互作用试验
```java
@Test
public void shouldWriteToDatabase() {
 accounts.createUser("foobar");
 verify(database).put("foobar");
}
```

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

The test verifies that a specific call was made against a database API, but there are a couple different ways it could go wrong:

- If a bug in the system under test causes the record to be deleted from the database shortly after it was written, the test will pass even though we would have wanted it to fail.

- If the system under test is refactored to call a slightly different API to write an equivalent record, the test will fail even though we would have wanted it to pass.

Example 12-5. Testing against state

</div></details>
这个测试验证一个特定的调用是否针对一个数据库API，但是有几种不同的方法可能出错:

- 如果测试系统中的一个bug导致记录在写入后不久就从数据库中删除，那么即使我们希望它失败，测试也会通过。

- 如果被测试的系统被重构，调用一个稍微不同的API来写一个等价的记录，测试将会失败，即使我们希望它通过。

例12-5. 测试相反的状态
```Java
@Test
public void shouldCreateUsers() {
 accounts.createUser("foobar");
 assertThat(accounts.getUser("foobar")).isNotNull();
}
```


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

This test more accurately expresses what we care about: the state of the system under test after interacting with it.

The most common reason for problematic interaction tests is an over reliance on mocking frameworks. These frameworks make it easy to create test doubles that record and verify every call made against them, and to use those doubles in place of real objects in tests. This strategy leads directly to brittle interaction tests, and so we tend to prefer the use of real objects in favor of mocked objects, as long as the real objects are fast and deterministic.

<div> <image src='./pic/bird.png' width=60 style="float:left"/>
For a more extensive discussion of test doubles and mocking frameworks, when they should be used, and safer alternatives, see  <a href='./Chapter-13-0.md'>Chapter 13.</a>
</div>
</div></details>
这个测试更准确地表达了我们所关心的:与被测系统交互后的状态。

有问题的交互测试最常见的原因是过度依赖mock框架。这些框架使得创建测试双精度对象变得很容易，该测试双精度对象记录和验证对它们进行的每个调用，并在测试中使用这些双精度对象代替实际对象。这种策略直接导致脆性交互测试，因此我们倾向于使用真实对象而不是模拟对象，只要真实对象速度快且具有确定性。

<div> <image src='./pic/bird.png' width=60 style="float:left"/>
关于测试双功能和mock框架的更广泛讨论，以及它们应该在什么时候使用，以及更安全的替代方案，请参见 <a href='./Chapter-13-0.md'>第13章.</a>
</div>


## 编写清晰的测试 Writing Clear Tests

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Sooner or later, even if we’ve completely avoided brittleness, our tests will fail. Failure is a good thing—test failures provide useful signals to engineers, and are one of the main ways that a unit test provides value.
</div></details>
无论如何，即使我们已经完全避免了脆性，我们的测试也会失败。失败是一件好事——测试失败为工程师提供有用的信号，并且是单元测试提供价值的主要方式之一。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Test failures happen for one of two reasons:[^3]

- The system under test has a problem or is incomplete. This result is exactly what tests are designed for: alerting you to bugs so that you can fix them.

- The test itself is flawed. In this case, nothing is wrong with the system under test, but the test was specified incorrectly. If this was an existing test rather than one that you just wrote, this means that the test is brittle. The previous section discussed how to avoid brittle tests, but it’s rarely possible to eliminate them entirely.
</div></details>

测试失败的发生有两个原因:[^3]

- 被测系统存在问题或不完整。这个结果正是设计测试的目的:提醒您错误，以便您可以修复它们。

- 测试本身存在缺陷。在这种情况下，被测试的系统没有任何问题，但是指定的测试不正确。如果这是一个现有的测试，而不是您刚刚编写的测试，这意味着该测试是脆弱的。上一节讨论了如何避免脆性测试，但几乎不可能完全消除它们。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

When a test fails, an engineer’s first job is to identify which of these cases the failure falls into and then to diagnose the actual problem. The speed at which the engineer can do so depends on the test’s clarity. A clear test is one whose purpose for existing and reason for failing is immediately clear to the engineer diagnosing a failure. Tests fail to achieve clarity when their reasons for failure aren’t obvious or when it’s difficult to figure out why they were originally written. Clear tests also bring other benefits, such as documenting the system under test and more easily serving as a basis for new tests.
</div></details>
当测试失败时，工程师的第一项工作是确定故障落在哪些情况下，然后诊断实际问题。工程师能够做到这一点的速度取决于测试的清晰度。清晰的测试是指存在的目的和失败的原因能够立即让诊断失败的工程师清楚的测试。当测试失败的原因不明显或者很难找出最初编写它们的原因时，测试就不能达到清晰的目的。清晰的测试还会带来其他的好处，比如记录测试下的系统，更容易作为新测试的基础。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

Test clarity becomes significant over time. Tests will often outlast the engineers who wrote them, and the requirements and understanding of a system will shift subtly as it ages. It’s entirely possible that a failing test might have been written years ago by an engineer no longer on the team, leaving no way to figure out its purpose or how to fix it. This stands in contrast with unclear production code, whose purpose you can usually determine with enough effort by looking at what calls it and what breaks when it’s removed. With an unclear test, you might never understand its purpose, since removing the test will have no effect other than (potentially) introducing a subtle hole in test coverage.
</div></details>
随着时间的推移，测试清晰度变得非常重要。测试通常会比编写测试的工程师更持久，而且随着系统的老化，对系统的需求和理解也会发生微妙的变化。失败的测试完全有可能是几年前由不再在团队中的工程师编写的，这样就没有办法弄清楚测试的目的或如何修复它。这与不明确的产品代码形成了鲜明对比，产品代码的目的通常可以通过查看是什么调用它以及当它被删除时是什么中断来确定。对于一个不明确的测试，您可能永远不会理解它的目的，因为除去测试将没有任何效果，除了(潜在地)在测试覆盖中引入一个微妙的漏洞。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
In the worst case, these obscure tests just end up getting deleted when engineers can’t figure out how to fix them. Not only does removing such tests introduce a hole in test coverage, but it also indicates that the test has been providing zero value for perhaps the entire period it has existed (which could have been years).

</div></details>
在最坏的情况下，这些模糊的测试最终会被删除，因为工程师无法找出如何修复它们。移除这样的测试不仅会在测试覆盖上引入一个漏洞，而且它还表明，该测试可能在其存在的整个时期(可能是几年)提供了零值。

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
For a test suite to scale and be useful over time, it’s important that each individual test in that suite be as clear as possible. This section explores techniques and ways of thinking about tests to achieve clarity.
</div></details>
为了让测试套件随时间扩展并发挥作用，套件中的每个单独测试都尽可能清晰是非常重要的。本节将探讨测试的技术和方法，以实现测试的清晰性。

### 让你的测试完整而简洁 Make Your Tests Complete and Concise

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Two high-level properties that help tests achieve clarity are completeness and con‐ ciseness. A test is complete when its body contains all of the information a reader needs in order to understand how it arrives at its result. A test is concise when it con‐ tains no other distracting or irrelevant information. Example 12-6 shows a test that is neither complete nor concise:
 Example 12-6. An incomplete and cluttered test
</div></details>

两个有助于测试实现清晰的高级属性是完整性和简洁性。当一个测试的主体包含了读者为了理解它是如何得到它的结果所需要的所有信息时，这个测试就完成了。当一个测试不包含其他干扰或无关的信息时，它是简洁的。例12-6显示了一个既不完整也不简洁的测试:
例12-6 一个不完整且混乱的测试
```java
@Test
public void shouldPerformAddition() {
 Calculator calculator = new Calculator(new RoundingStrategy(),
 "unused", ENABLE_COSINE_FEATURE, 0.01, calculusEngine, false);
 int result = calculator.calculate(newTestCalculation());
 assertThat(result).isEqualTo(5); // Where did this number come from?
}
```

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
The test is passing a lot of irrelevant information into the constructor, and the actual important parts of the test are hidden inside of a helper method. The test can be made more complete by clarifying the inputs of the helper method, and more concise by using another helper to hide the irrelevant details of constructing the calculator, as illustrated in Example 12-7.

Example 12-7. A complete, concise test
</div></details>
测试将大量不相关的信息传递到构造函数中，而测试的实际重要部分隐藏在helper方法中。明确helper方法的输入可以使测试更完整，使用另一个helper隐藏构造计算器的无关细节可以使测试更简洁，如例12-7所示。

例12-7。一个完整而简洁的测试
```java
@Test
public void shouldPerformAddition() {
 Calculator calculator = newCalculator();
 int result = calculator.calculate(newCalculation(2, Operation.PLUS, 3));
  assertThat(result).isEqualTo(5);
}
```

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Ideas we discuss later, especially around code sharing, will tie back to completeness and conciseness. In particular, it can often be worth violating the DRY (Don’t Repeat Yourself) principle if it leads to clearer tests. Remember: a test’s body should contain all of the information needed to understand it without containing any irrelevant or distracting information.
</div></details>

我们稍后讨论的想法，特别是关于代码共享的想法，将与完整性和简洁性联系起来。特别是，如果违背DRY (Don 't Repeat Yourself)原则可以带来更清晰的测试，那么违背DRY原则通常是值得的。记住:测试的主体应该包含理解它所需的所有信息，而不包含任何无关或分散注意力的信息。

### 对行为进行测试，而不是方法 Test Behaviors, Not Methods

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
The first instinct of many engineers is to try to match the structure of their tests to the structure of their code such that every production method has a corresponding test method. This pattern can be convenient at first, but over time it leads to problems: as the method being tested grows more complex, its test also grows in complexity and becomes more difficult to reason about. For example, consider the snippet of code in Example 12-8, which displays the results of a transaction.

Example 12-8. A transaction snippet
</div></details>

许多工程师的第一反应是尝试将测试的结构与代码的结构相匹配，这样每个生产方法都有相应的测试方法。这种模式一开始可能很方便，但随着时间的推移，它会导致问题:随着被测试的方法变得越来越复杂，它的测试也变得越来越复杂，越来越难以推理。例如，考虑示例12-8中的代码片段，它显示了事务的结果。

例12-8。一个事务代码片段
```java
public void displayTransactionResults(User user, Transaction transaction) {
 ui.showMessage("You bought a " + transaction.getItemName());
 if (user.getBalance() < LOW_BALANCE_THRESHOLD) {
 ui.showMessage("Warning: your balance is low!");
 }
}
```

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
It wouldn’t be uncommon to find a test covering both of the messages that might be shown by the method, as presented in Example 12-9.

Example 12-9. A method-driven test
</div></details>
在示例12-9中，可以发现一个包含方法可能显示的两个消息的测试，这并不罕见。

例12-9。测试驱动方法
```java
@Test
public void testDisplayTransactionResults() {
 transactionProcessor.displayTransactionResults(
 newUserWithBalance(
 LOW_BALANCE_THRESHOLD.plus(dollars(2))),
 new Transaction("Some Item", dollars(3)));
 assertThat(ui.getText()).contains("You bought a Some Item");
 assertThat(ui.getText()).contains("your balance is low");
}
```

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>

With such tests, it’s likely that the test started out covering only the first method. Later, an engineer expanded the test when the second message was added (violating the idea of unchanging tests that we discussed earlier). This modification sets a bad precedent: as the method under test becomes more complex and implements more functionality, its unit test will become increasingly convoluted and grow more and more difficult to work with.

The problem is that framing tests around methods can naturally encourage unclear tests because a single method often does a few different things under the hood and might have several tricky edge and corner cases. There’s a better way: rather than writing a test for each method, write a test for each behavior.[^4] A behavior is any guarantee that a system makes about how it will respond to a series of inputs while in a particular state.[^5] Behaviors can often be expressed using the words “given,” “when,” and “then”: “Given that a bank account is empty, when attempting to withdraw money from it, then the transaction is rejected.” The mapping between methods and behaviors is many-to-many: most nontrivial methods implement multiple behaviors, and some behaviors rely on the interaction of multiple methods. The previous example can be rewritten using behavior-driven tests, as presented in Example 12-10.

Example 12-10. A behavior-driven test
</div></details>
在这样的测试中，测试可能一开始只覆盖第一种方法。稍后，当添加第二条消息时，工程师扩展了测试(违反了我们前面讨论的不变测试的想法)。这个修改设置了一个不好的先例:随着测试中的方法变得更加复杂，实现了更多的功能，它的单元测试将变得越来越复杂，越来越难以使用。


问题在于，围绕方法构建测试会自然而然地鼓励不明确的测试，因为单一的方法通常会在幕后做一些不同的事情，并且可能会有一些棘手的边缘和角落情况[^4]。有一种更好的方法:为每个行为编写一个测试，而不是为每个方法编写一个测试[^5]。行为是系统在特定状态下如何响应一系列输入的任何保证。行为通常可以用“给定”、“当”和“然后”来表达:“假设一个银行账户是空的，当试图从它里面取钱时，那么交易就会被拒绝。”方法和行为之间的映射是多对多的:大多数重要的方法实现多个行为，一些行为依赖于多个方法的交互。可以使用行为驱动测试重写前面的例子，如例12-10所示。

例12-10。行为驱动测试

```java
@Test
public void displayTransactionResults_showsItemName() {
 transactionProcessor.displayTransactionResults(
 new User(), new Transaction("Some Item"));
 assertThat(ui.getText()).contains("You bought a Some Item");
}
@Test
public void displayTransactionResults_showsLowBalanceWarning() {
 transactionProcessor.displayTransactionResults(
 newUserWithBalance(
 LOW_BALANCE_THRESHOLD.plus(dollars(2))),
 new Transaction("Some Item", dollars(3)));
 assertThat(ui.getText()).contains("your balance is low");
}
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
The extra boilerplate required to split apart the single test is more than worth it, and the resulting tests are much clearer than the original test. Behavior-driven tests tend to be clearer than method-oriented tests for several reasons. First, they read more like natural language, allowing them to be naturally understood rather than requiring laborious mental parsing. Second, they more clearly express cause and effect because each test is more limited in scope. Finally, the fact that each test is short and descriptive makes it easier to see what functionality is already tested and encourages engineers to add new streamlined test methods instead of piling onto existing methods.

</div></details>

分割单个测试所需的额外样板是非常值得的，结果比之前的测试更清晰。出于几个原因，行为驱动的测试往往比面向方法的测试更清晰。首先，它们读起来更像自然语言，使它们能够自然地被理解，而不需要费力的心理解析。其次，它们更清楚地表达因果关系，因为每个测试的范围更有限。最后，每个测试都很短且描述性强，这一事实使我们更容易看到哪些功能已经被测试，并鼓励工程师添加新的简化的测试方法，而不是堆积在现有的方法上。

#### 结构测试强调行为 Structure tests to emphasize behaviors

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>

Thinking about tests as being coupled to behaviors instead of methods significantly affects how they should be structured. Remember that every behavior has three parts: a “given” component that defines how the system is set up, a “when” component that defines the action to be taken on the system, and a “then” component that validates the result[^6]. Tests are clearest when this structure is explicit. Some frameworks like Cucumber and Spock directly bake in given/when/then. Other languages can use whitespace and optional comments to make the structure stand out, such as that shown in Example 12-11.

Example 12-11. A well-structured test
</div></details>

将测试看作是与行为而不是与方法相耦合的，这将极大地影响测试的结构。记住，每个行为都有三个部分:一个“give”组件，它定义了系统是如何设置的;一个“when”组件，它定义了要在系统上执行的操作;一个“then”组件，它验证结果[^6]。当这个结构是明确的时，测试是最清楚的。有些框架，如Cucumber和Spock，直接在给定/当/然后中进行烘焙。其他语言可以使用空格和可选注释来突出这个结构，如例12-11所示。

示例12-11。一个结构良好的测试
```java
@Test
public void transferFundsShouldMoveMoneyBetweenAccounts() {
 // Given two accounts with initial balances of $150 and $20
 Account account1 = newAccountWithBalance(usd(150));
 Account account2 = newAccountWithBalance(usd(20));
 // When transferring $100 from the first to the second account
 bank.transferFunds(account1, account2, usd(100));
 // Then the new account balances should reflect the transfer
 assertThat(account1.getBalance()).isEqualTo(usd(50));
 assertThat(account2.getBalance()).isEqualTo(usd(120));
}
```

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
This level of description isn’t always necessary in trivial tests, and it’s usually sufficient to omit the comments and rely on whitespace to make the sections clear. However, explicit comments can make more sophisticated tests easier to understand. This pattern makes it possible to read tests at three levels of granularity:

1. A reader can start by looking at the test method name (discussed below) to get a rough description of the behavior being tested.
2. If that’s not enough, the reader can look at the given/when/then comments for a formal description of the behavior.
3. Finally, a reader can look at the actual code to see precisely how that behavior is expressed.

This pattern is most commonly violated by interspersing assertions among multiple calls to the system under test (i.e., combining the “when” and “then” blocks). Merging the “then” and “when” blocks in this way can make the test less clear because it makes it difficult to distinguish the action being performed from the expected result.
</div></details>

这种级别的描述在简单的测试中并不总是必要的，通常省略注释并依靠空格来使部分清晰就足够了。然而，显式注释可以使更复杂的测试更容易理解。这种模式可以在三个粒度级别读取测试:

1. 读者可以从查看测试方法名称(下面讨论)开始，以获得被测试行为的粗略描述。
2. 如果这还不够，读者可以查看给定的/when/then注释，以获得对该行为的正式描述。
3. 最后，读者可以查看实际的代码，以准确地了解该行为是如何表达的。

在对被测试系统的多个调用之间散布断言(例如，结合“when”和“then”块)，通常会违反这种模式。以这种方式合并“then”和“when”块会使测试变得不那么清晰，因为它很难将正在执行的操作与预期的结果区分开来。

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>

When a test does want to validate each step in a multistep process, it’s acceptable to define alternating sequences of when/then blocks. Long blocks can also be made more descriptive by splitting them up with the word “and.” Example 12-12 shows what a relatively complex, behavior-driven test might look like.

Example 12-12. Alternating when/then blocks within a test
</div></details>

当测试确实想要验证多步骤过程中的每个步骤时，可以定义When /then块的交替序列。长块还可以通过使用“和”来分隔，使其更具描述性。例12-12显示了一个相对复杂的、行为驱动的测试可能是什么样的。
12-12示例。when/then在测试中阻塞交替
```java
@Test
public void shouldTimeOutConnections() {
 // Given two users
 User user1 = newUser();
 User user2 = newUser();
 // And an empty connection pool with a 10-minute timeout
 Pool pool = newPool(Duration.minutes(10));
 // When connecting both users to the pool
 pool.connect(user1);
 pool.connect(user2);
 // Then the pool should have two connections
 assertThat(pool.getConnections()).hasSize(2);
 // When waiting for 20 minutes
 clock.advance(Duration.minutes(20));
 // Then the pool should have no connections
 assertThat(pool.getConnections()).isEmpty();
 // And each user should be disconnected
 assertThat(user1.isConnected()).isFalse();
 assertThat(user2.isConnected()).isFalse();
}
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
When writing such tests, be careful to ensure that you’re not inadvertently testing multiple behaviors at the same time. Each test should cover only a single behavior, and the vast majority of unit tests require only one “when” and one “then” block.
</div></details>
在编写这样的测试时，要小心确保您没有在同一时间无意地测试多个行为。每个测试应该只覆盖单一的行为，并且绝大多数单元测试只需要一个“when”和一个“then”块。

#### 在正在测试的行为之后命名测试 Name tests after the behavior being tested

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Method-oriented tests are usually named after the method being tested (e.g., a test for the updateBalance method is usually called testUpdateBalance). With more focused behavior-driven tests, we have a lot more flexibility and the chance to convey useful information in the test’s name. The test name is very important: it will often be the first or only token visible in failure reports, so it’s your best opportunity to communicate the problem when the test breaks. It’s also the most straightforward way to express the intent of the test.

A test’s name should summarize the behavior it is testing. A good name describes both the actions that are being taken on a system and the expected outcome. Test names will sometimes include additional information like the state of the system or its environment before taking action on it. Some languages and frameworks make this easier than others by allowing tests to be nested within one another and named using strings, such as in Example 12-13, which uses Jasmine.

Example 12-13. Some sample nested naming patterns
</div></details>
面向方法的测试通常以被测试的方法命名(例如，updateBalance方法的测试通常称为testUpdateBalance)。有了更集中的行为驱动测试，我们有更多的灵活性和机会在测试的名称中传递有用的信息。测试名称非常重要:它通常是失败报告中第一个或唯一可见的标记，因此当测试中断时，它是您沟通问题的最佳机会。这也是表达测试意图的最直接的方式。

测试的名称应该总结它所测试的行为。一个好的名称既可以描述在系统上执行的操作，也可以描述预期的结果。测试名称有时会包含额外的信息，比如在对其采取行动之前系统或其环境的状态。有些语言和框架允许测试彼此嵌套并使用字符串命名，从而使这一点比其他语言和框架更容易实现，例如在示例12-13中，它使用了Jasmine。

例12 - 13。一些嵌套命名模式示例

```javascript
describe("multiplication", function() {
 describe("with a positive number", function() {
    var positiveNumber = 10;
    it("is positive with another positive number", function() {
      expect(positiveNumber * 10).toBeGreaterThan(0);
    });
    it("is negative with a negative number", function() {
      expect(positiveNumber * -10).toBeLessThan(0);
    });
  });
  describe("with a negative number", function() {
    var negativeNumber = 10;
    it("is negative with a positive number", function() {
      expect(negativeNumber * 10).toBeLessThan(0);
    });
    it("is positive with another negative number", function() {
      expect(negativeNumber * -10).toBeGreaterThan(0);
    });
  });  
});
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Other languages require us to encode all of this information in a method name, leading to method naming patterns like that shown in Example 12-14.

Example 12-14. Some sample method naming patterns
</div></details>
其他语言要求我们将所有这些信息编码到一个方法名中，导致方法命名模式如例12-14所示。

例12-14。一些示例方法命名模式
```bash
multiplyingTwoPositiveNumbersShouldReturnAPositiveNumber
multiply_postiveAndNegative_returnsNegative
divide_byZero_throwsException
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Names like this are much more verbose than we’d normally want to write for methods in production code, but the use case is different: we never need to write code that calls these, and their names frequently need to be read by humans in reports. Hence, the extra verbosity is warranted.

Many different naming strategies are acceptable so long as they’re used consistently within a single test class. A good trick if you’re stuck is to try starting the test name with the word “should.” When taken with the name of the class being tested, this naming scheme allows the test name to be read as a sentence. For example, a test of a BankAccount class named shouldNotAllowWithdrawalsWhenBalanceIsEmpty can be read as “BankAccount should not allow withdrawals when balance is empty.” By reading the names of all the test methods in a suite, you should get a good sense of the behaviors implemented by the system under test. Such names also help ensure that the test stays focused on a single behavior: if you need to use the word “and” in a test name, there’s a good chance that you’re actually testing multiple behaviors and should be writing multiple tests!

</div></details>

这样的名称比我们通常希望在生产代码中为方法编写的名称要冗长得多，但用例不同:我们从不需要编写调用这些方法的代码，而且它们的名称经常需要在报告中被人读取。因此，额外的冗长是必要的。
只要在一个测试类中一致地使用许多不同的命名策略，它们都是可以接受的。如果您卡住了，一个很好的技巧是尝试以单词“should”开始测试名称。当使用被测试类的名称时，这个命名方案允许将测试名称读为一个句子。例如，一个名为``shouldnotallowswhenbalanceisempty``的``BankAccount``类测试可以被读为“当余额为空时，``BankAccount``不允许取款”。通过阅读套件中所有测试方法的名称，您应该能够很好地了解被测试系统所实现的行为。这样的名称还有助于确保测试专注于单一的行为:如果您需要在测试名称中使用“和”这个词，那么很有可能您实际上正在测试多个行为，并且应该编写多个测试!

### 不要在测试中使用逻辑 Don’t Put Logic in Tests
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Clear tests are trivially correct upon inspection; that is, it is obvious that a test is doing the correct thing just from glancing at it. This is possible in test code because each test needs to handle only a particular set of inputs, whereas production code must be generalized to handle any input. For production code, we’re able to write tests that ensure complex logic is correct. But test code doesn’t have that luxury—if you feel like you need to write a test to verify your test, something has gone wrong

Complexity is most often introduced in the form of logic. Logic is defined via the imperative parts of programming languages such as operators, loops, and conditionals. When a piece of code contains logic, you need to do a bit of mental computation to determine its result instead of just reading it off of the screen. It doesn’t take much logic to make a test more difficult to reason about. For example, does the test in Example 12-15 look correct to you?
Example 12-15. Logic concealing a bug

</div></details>

清晰的测试在检查时通常是正确的;也就是说，很明显，仅仅从瞥一眼就可以看出测试是正确的。这在测试代码中是可能的，因为每个测试只需要处理一组特定的输入，而生产代码必须通用以处理任何输入。对于产品代码，我们能够编写确保复杂逻辑正确的测试。但是测试代码没有这种严格的要求——如果您觉得需要编写一个测试来验证您的测试，那么一定是出了问题

复杂性通常以逻辑的形式出现。逻辑是通过编程语言的命令式部分来定义的，比如运算符、循环和条件。当一段代码包含逻辑时，你需要做一些心理计算来确定它的结果，而不是仅仅从屏幕上阅读它。不需要太多的逻辑就可以使测试变得更加难以推理。例如，例12-15中的测试看起来正确吗?
例12-15. 通过逻辑隐藏bug

```java
@Test
public void shouldNavigateToAlbumsPage() {
 String baseUrl = "http://photos.google.com/";
 Navigator nav = new Navigator(baseUrl);
 nav.goToAlbumPage();
 assertThat(nav.getCurrentUrl()).isEqualTo(baseUrl + "/albums");
}
```

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
There’s not much logic here: really just one string concatenation. But if we simplify the test by removing that one bit of logic, a bug immediately becomes clear, as demonstrated in Example 12-16.

Example 12-16. A test without logic reveals the bug
</div></details>
这里没有太多的逻辑:实际上只是一个字符串连接。但是如果我们通过删除这一点逻辑来简化测试，一个错误就会立刻清晰起来，如例12-16所示。

例12-16。没有逻辑的测试会揭示错误
```java
@Test
public void shouldNavigateToPhotosPage() {
 Navigator nav = new Navigator("http://photos.google.com/");
 nav.goToPhotosPage();
 assertThat(nav.getCurrentUrl()))
 .isEqualTo("http://photos.google.com//albums"); // Oops!
}
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
When the whole string is written out, we can see right away that we’re expecting two slashes in the URL instead of just one. If the production code made a similar mistake, this test would fail to detect a bug. Duplicating the base URL was a small price to pay for making the test more descriptive and meaningful (see the discussion of DAMP versus DRY tests later in this chapter).

If humans are bad at spotting bugs from string concatenation, we’re even worse at spotting bugs that come from more sophisticated programming constructs like loops and conditionals. The lesson is clear: in test code, stick to straight-line code over clever logic, and consider tolerating some duplication when it makes the test more descriptive and meaningful. We’ll discuss ideas around duplication and code sharing later in this chapter.
</div></details>

当写出整个字符串时，我们可以马上看到URL中需要两个斜杠而不是一个。如果产品代码犯了类似的错误，这个测试将无法检测到错误。为了使测试更具描述性和更有意义(请参阅本章后面关于DAMP和DRY测试的讨论)，复制基本URL是一个很小的代价。

如果人类不善于从字符串连接中发现错误，那么我们更不善于发现来自更复杂的编程结构(如循环和条件)的错误。教训很清楚:在测试代码中，坚持直线代码而不是聪明的逻辑，并考虑容忍一些重复，以使测试更具描述性和更有意义。我们将在本章后面讨论关于复制和代码共享的想法。

### 编写清晰的失败消息 Write Clear Failure Messages

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>

One last aspect of clarity has to do not with how a test is written, but with what an engineer sees when it fails. In an ideal world, an engineer could diagnose a problem just from reading its failure message in a log or report without ever having to look at the test itself. A good failure message contains much the same information as the test’s name: it should clearly express the desired outcome, the actual outcome, and any relevant parameters.

Here’s an example of a bad failure message:
``Test failed: account is closed``
Did the test fail because the account was closed, or was the account expected to be closed and the test failed because it wasn’t? A better failure message clearly distinguishes the expected from the actual state and gives more context about the result:

</div></details>

最后一个清晰的方面与如何编写测试无关，而是与工程师在测试失败时看到的情况有关。在理想的情况下，工程师可以通过阅读日志或报告中的失败消息来诊断问题，而不必查看测试本身。一个好的失败消息包含与测试名称相同的信息:它应该清楚地表达期望的结果、实际的结果和任何相关的参数。

下面是一个失败消息的例子:
``Test failed: account is closed``

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Did the test fail because the account was closed, or was the account expected to be closed and the test failed because it wasn’t? A better failure message clearly distinguishes the expected from the actual state and gives more context about the result:
</div></details>
测试失败是因为关闭了帐户，还是期望关闭帐户而测试失败是因为没有关闭帐户?一个更好的失败消息可以清楚地区分预期状态和实际状态，并给出关于结果的更多上下文:
```
Expected an account in state CLOSED, but got account:
 <{name: "my-account", state: "OPEN"}
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Good libraries can help make it easier to write useful failure messages. Consider the assertions in Example 12-17 in a Java test, the first of which uses classical JUnit asserts, and the second of which uses Truth, an assertion library developed by Google:

Example 12-17. An assertion using the Truth library

</div></details>
好的库可以帮助编写有用的失败消息。考虑Java测试中的示例12-17中的断言，第一个使用了经典的JUnit断言，第二个使用了由谷歌开发的断言库Truth:

例12 - 17。使用Truth库的断言
```bash
Set<String> colors = ImmutableSet.of("red", "green", "blue");
assertTrue(colors.contains("orange")); // JUnit
assertThat(colors).contains("orange"); // Truth
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Because the first assertion only receives a Boolean value, it is only able to give a generic error message like “expected but was ,” which isn’t very informative in a failing test output. Because the second assertion explicitly receives the subject of the assertion, it is able to give a much more useful error message: AssertionError: should have contained .”

Not all languages have such helpers available, but it should always be possible to manually specify the important information in the failure message. For example, test assertions in Go conventionally look like Example 12-18.

Example 12-18. A test assertion in Go
</div></details>

因为第一个断言只接收一个布尔值，所以它只能给出一个泛型的错误消息，如“expected but was”，这在失败的测试输出中不是很有用。因为第二个断言显式地接收断言的主语，所以它能够给出一个更有用的错误消息:AssertionError: should have contains。”

并不是所有的语言都有这样的助手，但是应该总是可以在失败消息中手动指定重要信息。例如，Go中的测试断言通常看起来像例12-18。

例12-18。Go中的测试断言
```go
result := Add(2, 3)
if result != 5 {
 t.Errorf("Add(2, 3) = %v, want %v", result, 5)
}
```
## 测试和代码共享:DAMP，而不是DRY Tests and Code Sharing: DAMP, Not DRY
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
One final aspect of writing clear tests and avoiding brittleness has to do with code sharing. Most software attempts to achieve a principle called DRY—“Don’t Repeat Yourself.” DRY states that software is easier to maintain if every concept is canonically represented in one place and code duplication is kept to a minimum. This approach is especially valuable in making changes easier because an engineer needs to update only one piece of code rather than tracking down multiple references. The downside to such consolidation is that it can make code unclear, requiring readers to follow chains of references to understand what the code is doing.

In normal production code, that downside is usually a small price to pay for making code easier to change and work with. But this cost/benefit analysis plays out a little differently in the context of test code. Good tests are designed to be stable, and in fact you usually want them to break when the system being tested changes. So DRY doesn’t have quite as much benefit when it comes to test code. At the same time, the costs of complexity are greater for tests: production code has the benefit of a test suite to ensure that it keeps working as it becomes complex, whereas tests must stand by themselves, risking bugs if they aren’t self-evidently correct. As mentioned earlier, something has gone wrong if tests start becoming complex enough that it feels like they need their own tests to ensure that they’re working properly.
</div></details>

编写清晰的测试和避免脆弱性的最后一个方面与代码共享有关。大多数软件都试图实现一个叫做DRY的原则——“不要重复自己”。DRY表示，如果每个概念都被规范化地表示在一个地方，并且将代码复制保持在最低限度，那么软件就更容易维护。这种方法在简化更改方面特别有价值，因为工程师只需要更新一段代码，而不需要跟踪多个引用。这种合并的缺点是，它会使代码不清晰，要求读者遵循引用链来理解代码正在做什么。

在正常的产品代码中，为了使代码更容易更改和使用，这种缺点通常是一个很小的代价。但是这种成本/收益分析在测试代码的上下文中表现得有点不同。好的测试被设计成是稳定的，事实上，当被测试的系统发生变化时，您通常希望它们被破坏。所以当涉及到测试代码时，DRY并没有那么多的好处。同时，对于测试来说，复杂性的代价更大:产品代码有一个测试套件的好处，可以确保它在变得复杂时继续工作，而测试必须独立运行，如果它们不是自明无误的话，就有出错的风险。正如前面提到的，如果测试开始变得非常复杂，以至于感觉需要自己的测试来确保它们正常工作，那么就会出现问题。
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>

Instead of being completely DRY, test code should often strive to be DAMP—that is, to promote “Descriptive And Meaningful Phrases.” A little bit of duplication is OK in tests so long as that duplication makes the test simpler and clearer. To illustrate, Example 12-19 presents some tests that are far too DRY.

Example 12-19. A test that is too DRY

</div></details>
测试代码不应该是完全DRY的，而应该努力做到damp——也就是说，提倡“描述性和有意义的短语”。在测试中有一点重复是可以的，只要重复可以使测试更简单、更清晰。举例说明，例12-19给出了一些非常DRY的测试。

示例-12-19。太DRY的测试
```java
@Test
public void shouldAllowMultipleUsers() {
 List<User> users = createUsers(false, false);
 Forum forum = createForumAndRegisterUsers(users);
 validateForumAndUsers(forum, users);
}
@Test
public void shouldNotAllowBannedUsers() {
 List<User> users = createUsers(true);
 Forum forum = createForumAndRegisterUsers(users);
 validateForumAndUsers(forum, users);
}
// Lots more tests...
private static List<User> createUsers(boolean... banned) {
 List<User> users = new ArrayList<>();
 for (boolean isBanned : banned) {
 users.add(newUser()
 	.setState(isBanned ? State.BANNED : State.NORMAL)
 	.build());
 }
 return users;
}
private static Forum createForumAndRegisterUsers(List<User> users) {
 Forum forum = new Forum();
  for (User user : users) {
 	try {
 		forum.register(user);
 	} catch(BannedUserException ignored) {}
 }
 return forum;
}
private static void validateForumAndUsers(Forum forum, List<User> users) {
 assertThat(forum.isReachable()).isTrue();
 for (User user : users) {
 	assertThat(forum.hasRegisteredUser(user)).isEqualTo(user.getState() == State.BANNED);
 }
}
```

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
The problems in this code should be apparent based on the previous discussion of clarity. For one, although the test bodies are very concise, they are not complete: important details are hidden away in helper methods that the reader can’t see without having to scroll to a completely different part of the file. Those helpers are also full of logic that makes them more difficult to verify at a glance (did you spot the bug?). The test becomes much clearer when it’s rewritten to use DAMP, as shown in Example 12-20.

Example 12-20. Tests should be DAMP
</div></details>
根据前面关于清晰度的讨论，这段代码中的问题应该是很明显的。首先，尽管测试体非常简洁，但它们并不完整:重要的细节隐藏在helper方法中，如果不滚动到文件的完全不同的部分，读者就无法看到这些方法。这些助手还充满了逻辑，使它们更难以一眼验证(您发现错误了吗?)如例12-20所示，当它被重写为使用DAMP时，测试变得更加清晰。

示例12-20。测试应采用DAMP
```java
@Test
public void shouldAllowMultipleUsers() {
 User user1 = newUser().setState(State.NORMAL).build();
 User user2 = newUser().setState(State.NORMAL).build();
 Forum forum = new Forum();
 forum.register(user1);
 forum.register(user2);
 assertThat(forum.hasRegisteredUser(user1)).isTrue();
 assertThat(forum.hasRegisteredUser(user2)).isTrue();
}
@Test
public void shouldNotRegisterBannedUsers() {
 User user = newUser().setState(State.BANNED).build();
 Forum forum = new Forum();
 try {
 forum.register(user);
 } catch(BannedUserException ignored) {}
 assertThat(forum.hasRegisteredUser(user)).isFalse();
}
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
These tests have more duplication, and the test bodies are a bit longer, but the extra verbosity is worth it. Each individual test is far more meaningful and can be understood entirely without leaving the test body. A reader of these tests can feel confident that the tests do what they claim to do and aren’t hiding any bugs.

DAMP is not a replacement for DRY; it is complementary to it. Helper methods and test infrastructure can still help make tests clearer by making them more concise, factoring out repetitive steps whose details aren’t relevant to the particular behavior being tested. The important point is that such refactoring should be done with an eye toward making tests more descriptive and meaningful, and not solely in the name of reducing repetition. The rest of this section will explore common patterns for sharing code across tests.
</div></details>

这些测试有更多的重复，并且测试主体稍微长一些，但是额外的冗长是值得的。每个单独的测试都更有意义，并且无需离开测试体就可以完全理解。阅读这些测试的读者可以确信这些测试能够完成它们所声称的工作，并且没有隐藏任何bug。

DAMP不是DRY的替代品;它是对它的补充。``Helper``方法和测试基础结构仍然可以通过简洁化使测试更清晰，排除那些细节与被测试的特定行为无关的重复步骤，从而使测试更清晰。重要的一点是，这样的重构应该着眼于使测试更具描述性和更有意义，而不是一味的减少重复。本节的其余部分将探讨跨测试共享代码的通用模式。

### 共享值 Shared Values

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Many tests are structured by defining a set of shared values to be used by tests and then by defining the tests that cover various cases for how these values interact. Example 12-21 illustrates what such tests look like.

Example 12-21. Shared values with ambiguous names

</div></details>
许多测试都是通过定义一组用于测试的共享值，然后定义覆盖这些值如何交互的各种情况的测试来构建的。例12-21说明了这样的测试是什么样的。

示例12-21。命名不明确的共享值
```java
private static final Account ACCOUNT_1 = Account.newBuilder()
 .setState(AccountState.OPEN).setBalance(50).build();
private static final Account ACCOUNT_2 = Account.newBuilder()
 .setState(AccountState.CLOSED).setBalance(0).build();
private static final Item ITEM = Item.newBuilder()
 .setName("Cheeseburger").setPrice(100).build();
// Hundreds of lines of other tests...
@Test
public void canBuyItem_returnsFalseForClosedAccounts() {
 assertThat(store.canBuyItem(ITEM, ACCOUNT_1)).isFalse();
}
@Test
public void canBuyItem_returnsFalseWhenBalanceInsufficient() {
 assertThat(store.canBuyItem(ITEM, ACCOUNT_2)).isFalse();
}
```

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>

This strategy can make tests very concise, but it causes problems as the test suite grows. For one, it can be difficult to understand why a particular value was chosen for a test. In Example 12-21, the test names fortunately clarify which scenarios are being tested, but you still need to scroll up to the definitions to confirm that ACCOUNT_1 and ACCOUNT_2 are appropriate for those scenarios. More descriptive constant names (e.g.,CLOSED_ACCOUNT and ACCOUNT_WITH_LOW_BALANCE) help a bit, but they still make it more difficult to see the exact details of the value being tested, and the ease of reusing these values can encourage engineers to do so even when the name doesn’t exactly describe what the test needs.

Engineers are usually drawn to using shared constants because constructing individual values in each test can be verbose. A better way to accomplish this goal is to construct data using helper methods (see Example 12-22) that require the test author to specify only values they care about, and setting reasonable defaults[^7] for all other values. This construction is trivial to do in languages that support named parameters, but languages without named parameters can use constructs such as the Builder pattern to emulate them (often with the assistance of tools such as AutoValue):

Example 12-22. Shared values using helper methods

</div></details>
这种策略可以使测试非常简洁，但随着测试套件的增长，它会导致问题。首先，很难理解为什么要为测试选择一个特定的值。在示例12-21中，测试名称幸运地澄清了正在测试的场景，但是您仍然需要向上滚动到定义，以确认ACCOUNT_1和ACCOUNT_2适用于这些场景。更具描述性的常数名称(例如,CLOSED_ACCOUNT和ACCOUNT_WITH_LOW_BALANCE)有所帮助,但他们仍然使其更难以看到的具体细节正在测试的价值,和易于重用这些值可以鼓励工程师这样做即使名称不确切描述测试需求。

工程师通常倾向于使用共享常量，因为在每个测试中构造单独的值可能很冗长。实现这一目标的更好方法是使用帮助方法(参见示例12-22)来构造数据，这些方法要求测试作者只指定他们关心的值，并为所有其他值设置合理的默认值[^7]。这种构造在支持命名参数的语言中是很简单的，但是不支持命名参数的语言可以使用诸如Builder模式之类的构造来模拟它们(通常借助AutoValue等工具):

例12-22.使用helper方法共享值
```c++
# A helper method wraps a constructor by defining arbitrary defaults for
# each of its parameters.
def newContact(
 firstName="Grace", lastName="Hopper", phoneNumber="555-123-4567"):
 return Contact(firstName, lastName, phoneNumber)
# Tests call the helper, specifying values for only the parameters that they
# care about.
def test_fullNameShouldCombineFirstAndLastNames(self):
 def contact = newContact(firstName="Ada", lastName="Lovelace")
 self.assertEqual(contact.fullName(), "Ada Lovelace")
// Languages like Java that don’t support named parameters can emulate them
// by returning a mutable "builder" object that represents the value under
// construction.
private static Contact.Builder newContact() {
 return Contact.newBuilder()
 .setFirstName("Grace")
 .setLastName("Hopper")
 .setPhoneNumber("555-123-4567");
}
// Tests then call methods on the builder to overwrite only the parameters
// that they care about, then call build() to get a real value out of the
// builder.
@Test
public void fullNameShouldCombineFirstAndLastNames() {
 Contact contact = newContact()
 .setFirstName("Ada")
 .setLastName("Lovelace")
 .build();
 assertThat(contact.getFullName()).isEqualTo("Ada Lovelace");
}
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Using helper methods to construct these values allows each test to create the exact values it needs without having to worry about specifying irrelevant information or conflicting with other tests.
</div></details>

使用帮助器方法来构造这些值允许每个测试创建它需要的确切值，而不必担心指定不相关的信息或与其他测试冲突。

### 共享设置 Shared Setup

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
A related way that tests shared code is via setup/initialization logic. Many test frameworks allow engineers to define methods to execute before each test in a suite is run. Used appropriately, these methods can make tests clearer and more concise by obviating the repetition of tedious and irrelevant initialization logic. Used inappropriately, these methods can harm a test’s completeness by hiding important details in a separate initialization method.

The best use case for setup methods is to construct the object under tests and its collaborators. This is useful when the majority of tests don’t care about the specific arguments used to construct those objects and can let them stay in their default states. The same idea also applies to stubbing return values for test doubles, which is a concept that we explore in more detail in Chapter 13.
</div></details>

测试共享代码的一个相关方法是通过设置/初始化逻辑。许多测试框架允许工程师在运行套件中的每个测试之前定义要执行的方法。如果使用得当，这些方法可以避免重复繁琐和无关的初始化逻辑，从而使测试更加清晰和简洁。如果使用不当，这些方法会在单独的初始化方法中隐藏重要的细节，从而损害测试的完整性。

setup方法的最佳用例是在测试和它的合作者下构造对象。当大多数测试不关心用于构造这些对象的特定参数，并且可以让它们保持默认状态时，这是很有用的。同样的思想也适用于测试双精度浮点数的存根返回值，这个概念我们将在第13章中详细讨论。
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
One risk in using setup methods is that they can lead to unclear tests if those tests begin to depend on the particular values used in setup. For example, the test in Example 12-23 seems incomplete because a reader of the test needs to go hunting to discover where the string “Donald Knuth” came from.

Example 12-23. Dependencies on values in setup methods
</div></details>

使用setup方法的一个风险是，如果这些测试开始依赖于setup中使用的特定值，则可能导致不清楚的测试。例如，示例12-23中的测试似乎不完整，因为测试同学需要查找字符串“Donald Knuth”来自哪里。

例12-23。对setup方法中的值的依赖关系
```java
private NameService nameService;
private UserStore userStore;
@Before
public void setUp() {
 nameService = new NameService();
 nameService.set("user1", "Donald Knuth");
 userStore = new UserStore(nameService);
}
// [... hundreds of lines of tests ...]
@Test
public void shouldReturnNameFromService() {
 UserDetails user = userStore.get("user1");
 assertThat(user.getName()).isEqualTo("Donald Knuth");
}
```
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Tests like these that explicitly care about particular values should state those values directly, overriding the default defined in the setup method if need be. The resulting test contains slightly more repetition, as shown in Example 12-24, but the result is far more descriptive and meaningful.

Example 12-24. Overriding values in setup mMethods
</div></details>

像这样明确关心特定值的测试应该直接声明这些值，如果需要重写在setup方法中定义的默认值。结果测试包含了更多的重复，如例12-24所示，但是结果更具描述性，也更有意义。

例12 - 24。重写setup mMethods中的值

```java
private NameService nameService;
private UserStore userStore;
@Before
public void setUp() {
 nameService = new NameService();
 nameService.set("user1", "Donald Knuth");
 userStore = new UserStore(nameService);
}
@Test
public void shouldReturnNameFromService() {
 nameService.set("user1", "Margaret Hamilton");
 UserDetails user = userStore.get("user1");
 assertThat(user.getName()).isEqualTo("Margaret Hamilton");
}
```
### 共享Helper和验证 Shared Helpers and Validation

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
The last common way that code is shared across tests is via “helper methods” called from the body of the test methods. We already discussed how helper methods can be a useful way for concisely constructing test values—this usage is warranted, but other types of helper methods can be dangerous.

One common type of helper is a method that performs a common set of assertions against a system under test. The extreme example is a validate method called at the end of every test method, which performs a set of fixed checks against the system under test. Such a validation strategy can be a bad habit to get into because tests using this approach are less behavior driven. With such tests, it is much more difficult to determine the intent of any particular test and to infer what exact case the author had in mind when writing it. When bugs are introduced, this strategy can also make them more difficult to localize because they will frequently cause a large number of tests to start failing.
</div></details>
跨测试共享代码的最后一种常见方式是通过从测试方法体调用“助手方法”。我们已经讨论了helper方法如何成为简洁构造测试值的有用方法—这种用法是有必要的，但其他类型的helper方法可能很危险。

一种常见的助手类型是针对被测试系统执行一组公共断言的方法。极端的例子是在每个测试方法结束时调用一个验证方法，它对被测试的系统执行一组固定的检查。这样的验证策略可能是一个坏习惯，因为使用这种方法的测试较少受行为驱动。有了这样的测试，就很难确定任何特定测试的意图，也很难推断出作者在编写测试时的确切情况。当引入错误时，这种策略也会使本地化错误变得更加困难，因为它们经常会导致大量测试开始失败。

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>

More focused validation methods can still be useful, however. The best validation helper methods assert a single conceptual fact about their inputs, in contrast to general-purpose validation methods that cover a range of conditions. Such methods can be particularly helpful when the condition that they are validating is conceptually simple but requires looping or conditional logic to implement that would reduce clarity were it included in the body of a test method. For example, the helper method in Example 12-25 might be useful in a test covering several different cases around account access.

Example 12-25. A conceptually simple test
</div></details>
然而，更集中的验证方法仍然是有用的。与覆盖一系列条件的通用验证方法不同，最好的验证助手方法断言关于其输入的一个概念性事实。当要验证的条件在概念上很简单，但需要循环或条件逻辑来实现(如果包含在测试方法的主体中，会降低清晰度)时，这种方法尤其有用。例如，例12-25中的helper方法可能在一个测试中有用，该测试涵盖了关于帐户访问的几种不同情况。

例12-25。一个概念简单的测试
```java
private void assertUserHasAccessToAccount(User user, Account account) {
 for (long userId : account.getUsersWithAccess()) {
		if (user.getId() == userId) {
			return;
		}
	}
 fail(user.getName() + " cannot access " + account.getName());
}
```

### 定义测试基础设施 Defining Test Infrastructure

<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
The techniques we’ve discussed so far cover sharing code across methods in a single test class or suite. Sometimes, it can also be valuable to share code across multiple test suites. We refer to this sort of code as test infrastructure. Though it is usually more valuable in integration or end-to-end tests, carefully designed test infrastructure can make unit tests much easier to write in some circumstances.

Custom test infrastructure must be approached more carefully than the code sharing that happens within a single test suite. In many ways, test infrastructure code is more similar to production code than it is to other test code given that it can have many callers that depend on it and can be difficult to change without introducing breakages. Most engineers aren’t expected to make changes to the common test infrastructure while testing their own features. Test infrastructure needs to be treated as its own separate product, and accordingly, test infrastructure must always have its own tests.

Of course, most of the test infrastructure that most engineers use comes in the form of well-known third-party libraries like JUnit. A huge number of such libraries are available, and standardizing on them within an organization should happen as early and universally as possible. For example, Google many years ago mandated Mockito as the only mocking framework that should be used in new Java tests and banned new tests from using other mocking frameworks. This edict produced some grumbling at the time from people comfortable with other frameworks, but today, it’s universally seen as a good move that made our tests easier to understand and work with.
</div></details>
到目前为止，我们讨论的技术涵盖了单个测试类或套件中方法之间的共享代码。有时，跨多个测试套件共享代码也是有价值的。我们将这类代码称为测试基础设施。虽然它通常在集成或端到端测试中更有价值，但精心设计的测试基础设施可以使单元测试在某些情况下更容易编写。

与在单个测试套件中发生的代码共享相比，必须更加小心地处理定制测试基础设施。在许多方面，测试基础设施代码与生产代码比与其他测试代码更相似，因为它可以有许多依赖于它的调用者，并且很难在不引入中断的情况下进行更改。大多数工程师在测试他们自己的特性时并不期望对公共测试基础结构进行更改。测试基础架构需要被视为其独立的产品，因此，测试基础架构必须始终拥有自己的测试。

当然，大多数工程师使用的大多数测试基础设施都是以众所周知的第三方库(如JUnit)的形式出现的。有大量这样的库可供使用，在一个组织内对它们进行标准化应该尽早且普遍地进行。例如，谷歌多年前就规定Mockito是新Java测试中应该使用的唯一模拟框架，并禁止新测试使用其他模拟框架。这一规定在当时引起了那些熟悉其他框架的人的一些抱怨，但是在今天，它被普遍认为是一个很好的举措，使我们的测试更容易理解和使用。


## 结论 Conclusion
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>
Unit tests are one of the most powerful tools that we as software engineers have to make sure that our systems keep working over time in the face of unanticipated changes. But with great power comes great responsibility, and careless use of unit testing can result in a system that requires much more effort to maintain and takes much more effort to change without actually improving our confidence in said system. Unit tests at Google are far from perfect, but we’ve found tests that follow the practices outlined in this chapter to be orders of magnitude more valuable than those that don’t. We hope they’ll help you to improve the quality of your own tests!
</div></details>

单元测试是最强大的工具之一，作为软件工程师，我们必须确保我们的系统在面对不可预期的更改时能够持续工作。但是，功能越强大，责任就越大，粗心的使用单元测试可能导致系统需要付出更多的努力来维护和更改，而实际上并没有提高我们对该系统的信心。谷歌的单元测试远非完美，但我们发现，遵循本章中概述的实践的测试比不遵循这些实践的测试更有价值。我们希望它们能帮助您提高自己测试的质量!

## TL;DRs
<details> <summary>origin</summary><div style='border:1px solid #eee;padding:5px;background-color:#F2F2F2'>

- Strive for unchanging tests.
- Test via public APIs.
- Test state, not interactions.
- Make your tests complete and concise.
- Test behaviors, not methods.
- Structure tests to emphasize behaviors.
- Name tests after the behavior being tested.
- Don’t put logic in tests.
- Write clear failure messages.
- Follow DAMP over DRY when sharing code for tests.
</div></details>

- 争取不变的测试。
- 通过公共api进行测试。
- 测试状态，而不是交互。
- 让你的测试完整而简洁。
- 测试行为，而不是方法。
- 结构测试，强调行为。
- 在被测试的行为之后命名测试。
- 不要在测试中使用逻辑。
- 写清楚失败消息。
- 在共享测试代码时遵循DAMP而不是DRY。

[^1]: Note that this is slightly different from a flaky test, which fails nondeterministically without any change to production code.
[^2]: This is sometimes called the "[Use the front door first principle.](https://oreil.ly/8zSZg)”
[^3]: These are also the same two reasons that a test can be “flaky.” Either the system under test has a nondeterministic fault, or the test is flawed such that it sometimes fails when it should pass.
[^4]: See https://testing.googleblog.com/2014/04/testing-on-toilet-test-behaviors-not.html and https://dannorth.net/introducing-bdd.
[^5]: Furthermore, a feature (in the product sense of the word) can be expressed as a collection of behaviors
[^6]:  These components are sometimes referred to as “arrange,” “act,” and “assert.”
[^7]: In many cases, it can even be useful to slightly randomize the default values returned for fields that aren’t explicitly set. This helps to ensure that two different instances won’t accidentally compare as equal, and makes it more difficult for engineers to hardcode dependencies on the defaults
