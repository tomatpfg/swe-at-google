
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


[^3]: These are also the same two reasons that a test can be “flaky.” Either the system under test has a nondeterministic fault, or the test is flawed such that it sometimes fails when it should pass.
[^4]: See https://testing.googleblog.com/2014/04/testing-on-toilet-test-behaviors-not.html and https://dannorth.net/introducing-bdd.
[^5]: Furthermore, a feature (in the product sense of the word) can be expressed as a collection of behaviors
[^6]:  These components are sometimes referred to as “arrange,” “act,” and “assert.”