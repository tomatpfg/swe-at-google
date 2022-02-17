## 防止脆性测试 Preventing Brittle Tests

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">

As just defined, a brittle test is one that fails in the face of an unrelated change to production code that does not introduce any real bugs[^1]. Such tests must be diagnosed and fixed by engineers as part of their work. In small codebases with only a few engineers, having to tweak a few tests for every change might not be a big problem. But if a team regularly writes brittle tests, test maintenance will inevitably consume a larger and larger proportion of the team’s time as they are forced to comb through an increasing number of failures in an ever-growing test suite. If a set of tests needs to be manually tweaked by engineers for each change, calling it an “automated test suite” is a bit of a stretch!
</div></details>


正如刚刚定义的那样，脆弱测试是在对生产代码进行不相关且不会引入任何真正错误的更改时失败的测试[^1]。工程师必须在工作中诊断和修复此类测试。在只有少数工程师的小型代码库中，每次更改都必须调整一些测试可能不是什么大问题。但是如果一个团队经常编写脆弱的测试，测试维护将不可避免地消耗团队越来越多的时间，因为他们被迫在不断增长的测试套件中梳理越来越多的故障。如果工程师每次更改都需要手动调整一组测试，那么将其称为“自动化测试套件”有点牵强！

<details> <summary>origin</summary><div style="border:1px solid #eee;padding:5px;background-color:#F2F2F2">
Brittle tests cause pain in codebases of any size, but they become particularly acute at Google’s scale. An individual engineer might easily run thousands of tests in a single day during the course of their work, and a single large-scale change (see Chapter 22) can trigger hundreds of thousands of tests. At this scale, spurious breakages that affect even a small percentage of tests can waste huge amounts of engineering time. Teams at Google vary quite a bit in terms of how brittle their test suites are, but we’ve identified a few practices and patterns that tend to make tests more robust to change.

</div></details>
脆弱的测试会给任何规模的代码库带来痛苦，但在 Google 的规模上它们变得特别严重。单个工程师在其工作过程中可能在一天之内轻松运行数千次测试，而一次大规模更改（参见第 22 章）可能会触发数十万次测试。在这种规模下，即使是影响一小部分测试的虚假破损也会浪费大量的工程时间。 Google 的团队在测试套件的脆弱程度方面存在很大差异，但我们已经确定了一些实践和模式，这些实践和模式往往会使测试更加健壮，易于更改。





[^1]: Note that this is slightly different from a flaky test, which fails nondeterministically without any change to production code.