
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