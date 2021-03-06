---
layout:     post
title:      Android 开发中的取舍
date: '2015-09-28'
---

刚开始做 Android 开发的时候，自己努力做到的就是 API 的使用、功能的实现以及满足产品的需求等等。但是现在，会有些不同，即使我仍然只用完成上面几件事情，就可以自称是一名熟练的 Android 工程师，然而我总觉得，工程两个字，意味着更多。  

Android 开发，或者说移动开发，具有以下几个特点：

1. 迭代快，需求繁多；
2. 永远处于变化中；
3. 移动平台的硬件软件限制。

当然还有其它的一些原因，不过上面几条是我在开发过程中遇到的主要问题。在这些客观条件的制约下，Android 开发会陷入忙于完成功能需求，而在出现变化的时候，不停打补丁的怪圈，同时，我们在面对有限的软硬件条件的时候，不得不做出一些妥协。我发现，取舍变成了 Android 开发中的一个重要主题，如何做好取舍，是一门艺术。

###代码设计

从纯语言的角度出发，Java 语言为我们提供了丰富的面向对象实现的支撑。当年在学校学习 Java 语言的时候，封装、继承、多态这三大面向对象的特点就牢记于心了，后来研读 *Thinking in Java*，算是对面向对象的思想有了初步的了解。再到后来的工作中，结合业务需求，在每一个类中，都可以从面向对象的角度出发，思考代码的实现。而面向对象和设计模式的联系又是如此紧密，很多的业务模块，是否在逻辑上、以及代码实现上，都和某个设计模式相契合呢？

我不敢说的太多了，因为我知道，有人会送我 4 个大字：**过度设计**！确实如此，面向对象告诉我们可复用、可维护、可扩展这些基本原则，设计模式又将一些精妙的最佳实践放在我们眼前。我们在 Android 开发的过程中，会不自觉地想太多。对于每个类来说，你可以考虑的问题就很多，比如最小职责原则，是应该用继承还是组合，定义为类还是接口，哪些东西应该放入抽象基类中，是否可以达到里氏替换原则方便以后的扩展，访问权限修饰符是否得当。至于整个功能模块或者整个应用，可以思考的问题就更加多了，要达到可复用、可维护、可扩展这 3 点，就要花费数倍的精力进行代码的设计以及重构。

我不会在这里讨论具体如何进行代码设计，而是谈一谈如何把握这个度，做好取舍。

###为什么要做好代码设计

软件开发有相当的工作量是搭积木，重点在于你能否找到好的积木。目前一个应用几乎所有的核心模块，包括网络请求、图片处理、数据库，甚至依赖注入、应用内通信，都有比较成熟的开源库可供使用。但是不要忘了，我们自己写的代码，也有可能在将来变成我们自己的积木。比如一个普通的 Fragment 实现类，在应用 UI 架构发生改变，或者要兼容多平台的时候，就可以成为积木。再比如，自己实现的库，做好抽象，剥离业务，就能成为各个业务需求的积木。我们在快速的开发过程中，如果能有现成的高质量的积木，可以大大节省开发成本。

并且，一个客户端往往是由多人并行开发，如果能做到上面所说的可复用，也能避免很多的重复冗余代码。否则，由于各个业务的差别，甚至是细微差别，例如可能只是传递的参数差了一两个，都可能会出现 *Ctrl-C Ctrl-V*，再简单改几行代码的情况。代码的重复冗余对 APK 包的大小的影响其实并没有太大，但是对于代码可读性的影响却相当大。有时候，你甚至无法判断，两个方法究竟是为了省事才这么写的，还是为了妥协之前的写法，又或者必须这么写，只是由于某些不可告人的秘密。

目前一个普遍的观点是，移动开发变化太快，市场的变化、功能需求的变化在推动着工程师必须不停地应对这些变化，结果就是，工程师没有时间思考代码设计，首要任务就是完成功能，就像我们语文高考写作文一样，没有时间打草稿，你必须下笔就写。但是，我们来想一下，这里的变化究竟是什么变化，是新增的功能需求吗？是新的 UI 设计吗？还是说是之前做的不符合要求，要进行改动？其实相当一部分，是对于旧有代码的改动。一部分代码，如果不停地改来改去，拿到这里，又 Copy 到那里，后续的工作会变得异常棘手。这个时候，如果能分得清，哪些是可能变化的部分，哪些是基本不可能变化的部分，对代码做适当的分离、抽象，会更好的应对来自业务的变化。简单来说，就是对产品经理屏蔽一些部分，剩下的部分暴露给他们，要改的话，也能尽可能地降低成本。

有一个现实情况是，大家跳槽很快，尤其是市场对移动开发工程师的需求非常旺盛，很多人都是一两年换一家公司。即使大家都不跳槽，你和同事维护的功能模块也有可能发生变化。很多工程师不怕写代码，最怕维护代码。为什么会这样，就是因为维护代码需要看懂之前的逻辑，同时又有很多看得见的看不见的大坑小坑，有些代码，你哪怕看明白了，也不敢改，更可怕的是，看都看不明白，不知道怎么改。这个时候，如果有设计良好的清晰的代码，哪怕不需要文档和注释，都会减少大量的维护成本。

最后一点，变化，这个变化比刚才说到的变化更加宽泛抽象。设计模式要解决的一个基本问题，就是处理代码中变化的部分。弄明白了哪些是变化的，哪些是不变的，再把变化的部分抽离开来，会让代码更加健壮，也利于将来的扩展。一旦我们做到了对修改关闭，对扩展开放，维护代码的人，可以在之前良好的基础上，继续写出可复用、可维护、可扩展的代码，而不是在一滩烂泥上不停地打补丁。这个就像算法的复杂度一样，问题规模越大，越能体现算法的优劣，对于我们来说，应用持续开发的时间越长，越能体现代码设计的优劣。

###为什么要避免过度设计 & 如何避免

上面说了那么多，并不代表我愿意在代码设计上投入过多的时间，考虑每一个可能变化的元素，将代码写的尽善尽美。这样做的成本是无法接受的，尤其是在移动开发领域，这么做的代价，很可能就是脱离现实，在竞争对手前面先死。另外也要明白一点，没有人可以一次性写出完美的代码，或者说根本不存在完美的代码。产品开发的每个阶段，都有各自的特点和侧重点。刚开始的时候，要尽快地开发出产品原型发现问题，或者投入市场进行试错，而这一点是 Google X 实验室的一个重要原则。初始阶段以后，很多阶段应该是交错的，功能的完善、代码的重构，或者完全停下来重新思考，是应该根据需要进行的。

过度设计很可能导致的一个问题，是矫枉过正。太追求完美，反而导致初期投入成本过大，后期由于各种变化，并未得到什么好处。甚至于，后面为了适应这个完美的架构，需要持续耗费大量的精力。这显然不是目前互联网公司能够承受的。

另外一点，移动开发是有别于传统软件开发的。移动平台具有自身的特点，比如硬件性能的限制、操作系统的限制、使用场景的限制等等。如果不考虑这些实际情况，一味追求应用本身的实现，会收效甚微甚至适得其反。比如，每个应用可以使用的内存资源相当有限，尤其是对于一些视频、图片的应用，吃内存就像吃月饼一样，几口就没了。这个时候，要学会取舍，也许有一种写法，相当炫酷，语法上惊为天人，但是内存的消耗却是数倍，那么简单的写法可能更加合适。不同水平的工程师的接受、学习以及产出能力也不尽相同，一个高大上的框架，未必能如你所愿，让整个应用的代码都高大上起来。

其实谈到如何避免过度设计，是没有那么难的，答案很简单，不过度设计就行了。真正难的是，如果做好取舍，在一个具体的情况下，工程师可能要考虑很多问题，比如代码的质量、工期、需求的稳定性、将来可能的变化，这个时候，不同的写法，都会很有道理。我时常会有这种时候，这个时候，我经常和同事进行讨论，请教一些资深的工程师，看看他们的想法。一个人想问题，经常会有遗漏的方面。多倾听意见，会帮助我们更好的思考。但是这种问题，往往没有一个标准答案，所谓取舍，就如这个词本身的意义一样，是很困难的。

###后话

这个题目，我应该每隔一段时间，就思考一下。我目前的经验还是太少，知识储备也很浅，对这个问题的认知还是相当局限。再等一两年，说不定我会自己把这篇文章中的很多观点推翻。如果你有什么想法，也欢迎发邮件（gejiaheng@gmail.com）和我讨论。
