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

