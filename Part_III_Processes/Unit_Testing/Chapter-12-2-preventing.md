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

```java
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
```java
@Test
public void shouldCreateUsers() {
 accounts.createUser("foobar");
 assertThat(accounts.getUser("foobar")).isNotNull();
}
```


<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

This test more accurately expresses what we care about: the state of the system under test after interacting with it.

The most common reason for problematic interaction tests is an over reliance on mocking frameworks. These frameworks make it easy to create test doubles that record and verify every call made against them, and to use those doubles in place of real objects in tests. This strategy leads directly to brittle interaction tests, and so we tend to prefer the use of real objects in favor of mocked objects, as long as the real objects are fast and deterministic.

<div> <image src='../pic/bird.png' width=60 style="float:left"/>
For a more extensive discussion of test doubles and mocking frameworks, when they should be used, and safer alternatives, see  <a href='./Chapter-13-0.md'>Chapter 13.</a>
</div>
</div></details>
这个测试更准确地表达了我们所关心的:与被测系统交互后的状态。

有问题的交互测试最常见的原因是过度依赖mock框架。这些框架使得创建test double对象变得很容易，该测试test double记录和验证对它们进行的每个调用，并在测试中使用这些test double代替实际对象。这种策略直接导致脆性交互测试，因此我们倾向于使用真实对象而不是模拟对象，只要真实对象速度快且具有确定性。

<div> <image src='../pic/bird.png' width=60 style="float:left"/>
关于test double和mock框架的更广泛讨论，以及它们应该在什么时候使用，以及更安全的替代方案，请参见 <a href='./Chapter-13-0.md'>第13章.</a>
</div>




[^1]: Note that this is slightly different from a flaky test, which fails nondeterministically without any change to production code.
[^2]: This is sometimes called the "[Use the front door first principle.](https://oreil.ly/8zSZg)”