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

[^7]: In many cases, it can even be useful to slightly randomize the default values returned for fields that aren’t explicitly set. This helps to ensure that two different instances won’t accidentally compare as equal, and makes it more difficult for engineers to hardcode dependencies on the defaults
