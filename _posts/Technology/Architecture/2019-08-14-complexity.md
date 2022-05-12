---

layout: post
title: 处理复杂性
category: 架构
tags: Architecture
keywords: complexity

---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

## 简介

* TOC
{:toc}

本文主要来自对阿里巴巴高级技术专家张建飞 公众号文章《从码农到工匠》的梳理和总结。作为技术人员，我们不能忘记我们技术人的首要技术使命是治理软件复杂度。

![](/public/upload/architecture/handle_complexity.png)

关于原则和方法论，既不必刻意拔高，也不要嗤之以鼻。指导实践的不是更多的实践，而是实践后的总结和思考。

## 如何定义复杂性

[系统困境与软件复杂度，为什么我们的系统会如此复杂](https://mp.weixin.qq.com/s/npTzzZiJ_pvbXgFOTGmZ0Q)关于复杂的定义有很多种
1. 理性度量，McCabe 圈复杂度，罗列了各种维度，从而判断软件当前的开发/维护成本。
2. 感性认知，所谓复杂性，就是任何使得软件难于理解和修改的因素。表现
    1. 变更放大，看似简单的变更需要在许多不同地方进行代码修改
    2. 认知负荷，开发人员需要多少知识才能完成一项任务
    3. 未知的未知，开发人员必须获得哪些信息才能成功地执行任务

[对抗软件复杂度的战争](https://mp.weixin.qq.com/s/Dil5Ual1aI_7dsGKV0f6Ig)在《人月神话》里，作者把复杂性分为两种：

1. 本质复杂性（Essential Complexity），指的是你要解决的问题本身的复杂性，是无法避免的。
2. 附属复杂性（Accidental Complexity），是指我们在解决本质问题时，所采用的解决方案而引入的复杂性。在我们现在的系统中，90% 的工作量都是用来解决附属复杂性的。

比如做一个电商系统，成本是多少？可能几千块，也可能很多亿。如果你理解这个的答案，那意味着比较理解当前软件编程的复杂性问题。因为软件系统的复杂性会随着规模急剧上升。软件的本质复杂度实际上是问题空间（或者称之为业务）带来的，因此给软件加入越多的功能，那么它就必然会包含越多的本质复杂度。此外，每解决一个问题，就对应了一个方案，而方案的实现必然又引入新的偶然复杂度，例如为了实现支付，需要实现某种新的协议，以对接一个三方的支付系统。本质复杂度是一个方面，毕竟更多用户意味着更多的功能特性，但我们无法忽略这里的偶然复杂度
1. 其中最典型的就是分布式系统引入的偶然复杂度（调度系统、负载均衡系统、服务发现，RPC，消息系统、高可用体系）。
2. 相比于分布式系统引入的复杂度，团队的扩张更易带来偶然复杂度的急剧增长。如果企业没有严格清晰的人才招聘标准，人员入职后没有严格的技术规范培训，当所有人以不同的风格，不同的长短期目标往代码仓库中提交代码的时候，软件的复杂度就会急剧上升。
3. 团队的扩张还会带来另外一个问题，在大规模的团队中，关键干系人的目标事实上是影响软件复杂度的关键因素。我亲眼见过许多案例，其方案空间中明明放着简单的方案，但因为这个原因，当事人不得不选择复杂的方案，例如：原本方案只需要直接改动系统 A，但由于负责系统 A 的团队并没有解决该问题的动力，其他人不得不绕道去修改系统 B，C，D 来解决该问题。

[降低软件复杂性的一般原则和方法](https://mp.weixin.qq.com/s/-Gu_XkY2bZq9Lf2ZCJZPtQ)
斯坦福教授、Tcl语言发明者John Ousterhout 的著作《A Philosophy of Software Design》

$$
C=\sum_{p}c_pt_p
$$

子模块的复杂度Cp乘以该模块对应的开发时间权重值tp，累加后得到系统的整体复杂度C。也就是说，即使某个模块非常复杂，如果很少使用或修改，也不会对系统的整体复杂度造成大的影响。子模块的复杂度Cp是一个经验值，它关注几个现象：

1. 修改扩散，修改时有连锁反应。
2. 认知负担，开发人员需要多长时间来理解功能模块。
3. 不可知（Unknown Unknowns），开发人员在接到任务时，不知道从哪里入手。
4. 认知成本，是指开发人员需要多少知识才能完成一项任务。clean architecture/clean code 


[如何从容应对复杂性](https://mp.weixin.qq.com/s/8YD9sqTuZGJEpVZPYY-aBQ)软件设计的最大目标，就是降低复杂度（complexity），**具体就是设计符合业务的构造定律的演进方式**，一种可以以最小的开发维护成本， 使业务更快更好的流动发展的方式。软件复杂性来自哪里， 如何解决？
1. 不确定性：业务、技术、人员流动的不确定性
2. 无序性，如何解决：统一认知（秩序化）；系统清晰明了的结构（结构化）；业务开发流程化（标准化）
3. 规模：业务、人员、组织


[对抗软件复杂度的战争](https://mp.weixin.qq.com/s/Dil5Ual1aI_7dsGKV0f6Ig)我喜欢学习各种能力强大的编程语言，例如具备元编程能力的 Ruby 和 Scala，使用这些能力你可以尽情发挥自己的兴趣和创造力。但是我对在有一定团队规模的生产环境中使用这些编程语言持保留意见，因为除非花很大力气 Review 和控制代码风格，否则很可能 10 个人写出来的代码是 10 种风格，这种复杂度的增长是个灾难。相反，使用 Java 这种不那么灵活的语言，大家写代码的风格就比较难不一致。

## 组织层面

[技术人自己的KPI](https://mp.weixin.qq.com/s/QD50uWgnQbuzAY3EVOT_jQ)

1. 不管你们有多敬业，加多少班，在面对烂系统时，你任然会寸步难行，因为你大部分的精力不是在开发需求，还是在应对混乱。**造成这种局面，我们的技术管理者，我们的TL要负有主要责任。说的重一点，是工作上的失职，这种失职主要体现在两个方面，一个是技术不作为，另一个是业务不思考。**
2. 技术人员的疲于奔命，内因上是由于上面分析的团队技术味道的缺失，外因上主要是PD的乱作为。

![](/public/upload/architecture/technology_kpi.png)

软件研发的核心职责之一是关注软件复杂度，通过开放代码、文档，Code Review 等方式让软件复杂度的**信息透明**，并且**让所有在增加/降低复杂度的行为透明**，并且持续激励那些消除复杂度的行为。唯有如此，在微观层面的控制复杂度的方法才能得到落实。

介于宏观的技术战略和微观的工程师文化之间，存在着一块重要的决策区域，也对软件复杂度有着关键的影响，我称之为系统架构。在面对需求的时候，缺乏经验的工程师会直接想着在自己熟悉的模块中直接解决，而经验丰富的工程师会先思考一下系统上下文。仔细去分析这一复杂度形成的因素，我发现这既不是技术战略的问题，也不是微观层面工程师生产低质量代码导致，而是有其他更深层次的问题。其中的最核心的因素是，这些子系统在不同时期是归属于不同的团队的，有的甚至是不同部门的，具体来说，**当各个部门各个团队目标不一致的时候，而这个系统又不幸地被拆到各个团队，那么就不会有人会对系统整体的复杂度控制负责**。当有的团队在负责把这套系统商业化对外输出，有的团队在负责把这套系统从虚拟机模式演进到容器模式，有的团队在负责资源的成本控制，有的团队在思考全局高可用架构，而没有一个全局的架构师从整体控制概念，控制边界的时候，系统就自然而然地腐化成这样的一个状态了。康威定律所揭示的事实，就是软件架构在很大程度上是由组织的结构和协作模式决定的，这实际上已经不再是一个软件技术问题了，而是一个组织管理问题。因此，解决系统架构层面的软件复杂度问题，就必须面对组织管理的挑战。关键问题域是否有唯一的负责人？当不同的团队在同一个问题域重复建设系统的时候，如何整合团队？当已有团队为了自己的生存，不断夸大其负责系统的重要性和特殊性，如何识别这种问题？组织如何给予大家充分的安全感，让工程师愿意为了架构的合理性，放弃自己辛苦耕作的系统模块？


## 架构设计

所有的软件架构万变不离其宗，都在致力解决软件的复杂性。软件的本质是约束。商品的代码不能写在订单域，数据层的方法不能写在业务层。70年的软件发展，并没有告诉我们应该怎么做，而是教会了我们不该做什么。

我们观察地图，其实除了中国、俄罗斯以外，全世界99%的国家都是小国。分裂才是常态，统一才不正常。所以我们不应该问为什么欧洲是分裂的，而应该问为什么中国是统一的。软件的复杂性是一个基本特征，而不是偶然如此。世间万物都需要额外的能量和秩序来维持自身，所有的事物都在缓慢地分崩离析。没有外部力量的注入事物就会逐渐崩溃，这是世间万物的规律，而非我们哪里做得不对。为软件系统注入的外力就是我们的软件架构，以及我们未来的每一行代码。

![](/public/upload/architecture/architecture_category.png)

[阿里研究员：警惕软件复杂度困局](https://mp.weixin.qq.com/s/L2hviITn-MgKGWzyUdXSjg)软件设计和实现的本质是工程师相互通过“写作”来交流一些包含丰富细节的抽象概念并且不断迭代过程。越是大型系统，越需要简单性。对于真正重要的、长生命周期的软件演进，我们需要做到对于**复杂度增量**零容忍。

架构这个词似乎蕴含了一种建造和设计的意味。一个摩天大楼无论多么复杂，都是事先可以根据设计出完整详尽的图纸，按图准确施工，保证质量就能建造出来的。然而现实中的大型软件系统，却不是这么建造出来的。软件是长出来的。大型软件设计核心要素是控制复杂度。这一点非常有挑战，根本原因在于软件不是机械活动的组合，**不能**在事先通过精心的“架构设计”规避复杂度失控的风险：相同的架构图/蓝图，可以长出完完全全不同的软件来。所以说了这么多是要停留在形而上吗？并不是。我们的结论是，软件架构师最重要的工作**不是**设计软件的结构，而是**通过API，团队设计准则和对细节的关注，控制软件复杂度的增长**。

软件复杂度从根本上说可以说是一个**主观**指标，说其主观是因为软件复杂度只有在程序员需要更新、维护、排查问题的时候才有意义。复杂度指的是软件中那些让人理解和修改维护的困难程度。相应的，简单性，就是让理解和维护代码更容易的要素。因此我们将软件的复杂度分解为两个维度，都和人理解与维护软件的成本相关：认知负荷与协同成本
1. 认知负荷 cognitive load ：理解软件的接口、设计或者实现所需要的心智负担。
    1. 定义新的概念带来认知负荷，而这种认知负荷与 概念和物理世界的关联程度相关。
    2. 逻辑符合思维习惯程度：正反逻辑差异，逻辑嵌套和独立原子化组合。继承和组装差异。 
    3. 接口设计不当，比如暴露出去的方法有一些隐含约定，进而导致使用不当。也比如 有多种方式让调用者实现完全相同的功能
    4. 一个简单的修改需要在多处更新，命名（Naming的难度在于对于模型的深入思考和抽象，而这往往确实是很难的。）
2. 协同成本Collaboration cost：团队维护软件时需要在协同上额外付出的成本。
    1. 增加一个新的特性往往需要多个工程师协同配合，甚至多个团队协同配合
    2. 测试以及上线需要协调同步。交付给其他团队（包括测试团队）的代码应该包含充分的单元测试，具备良好的封装和接口描述，易于被集成测试的。然而因为 单测不足/模块测试不足，**带来的集成阶段的复杂度升高、失败率和返工率的升高**，都极大的增加了协同的成本。

真正的工程师一定在意自己的作品：我们的作品就是我们的代码。工匠精神是对每个工程师的要求。

[降低软件复杂性的一般原则和方法](https://mp.weixin.qq.com/s/-Gu_XkY2bZq9Lf2ZCJZPtQ)**解决复杂性的一般原则**，

1. 设计是迭代出来的。 好的设计是日拱一卒的结果，在日常工作中要重视设计和细节的改进。
    1. 拒绝**战术编程**。战术编程致力于完成任务，新增加特性或者修改Bug时，能解决问题就好。修改Bug时，也应该抱着设计新系统的心态，完工后让人感觉不到“修补”的痕迹。有一种观点认为，创业公司需要追求业务迭代速度和节省成本，可以容忍糟糕的设计，这是**用错误的方法去追求正确的目标**。降低开发成本最有效的方式是雇佣优秀的工程师，而不是在设计上做妥协。
    2. 设计两次。为一个类、模块或者系统的设计提供两套或更多方案，有利于我们找到最佳设计。
2. 分层 ==> 专业化分工和代码复用；每一层最多影响两层，也给维护带来了很大的便利。
3. 分模块，分模块降低了单模块的复杂性，但是也会引入新的复杂性。深模块和浅模块。

软件工程最大的成本在于维护，我们每一次代码的改动，都应该是对历史代码的一次整理，而非单一的功能堆积。当我们使用一些极端的手段来保持古老而陈腐的软件继续工作时，这是一种苟且。

## 分模块

![](/public/upload/architecture/split_module.png)

Unix操作系统文件I/O是典型的深模块，以Open函数为例，接口接受文件名为参数，返回文件描述符。但是这个接口的背后，是几百行的实现代码，用来处理文件存储、权限控制、并发控制、存储介质等等，这些对用户是不可见的。

与深模块相对的是浅模块（Shallow Module），功能简单，接口复杂。通常情况下，**浅模块无助于解决复杂性**。因为他们提供的收益（功能）被学习和使用成本抵消了。以Java I/O为例，从I/O中读取对象时，需要同时创建三个对象FileInputStream、BufferedInputStream、ObjectInputStream，其中前两个创建后不会被直接使用，这就给开发人员造成了额外的负担。默认情况下，开发人员无需感知到BufferedInputStream，缓冲功能有助于改善文件I/O性能，是个很有用的特性，可以合并到文件I/O对象里。假如我们想放弃缓冲功能，文件I/O也可以设计成提供对应的定制选项。

## 业务逻辑和技术细节的分离

[应用架构之道：分离业务逻辑和技术细节](https://mp.weixin.qq.com/s/F5BWyALjJxryZXfb8WcH9g)架构始于建筑，是因为人类发展（原始人自给自足住在树上，也就不需要架构），分工协作的需要，将目标系统按某个原则进行切分，**切分的原则，是要便于不同的角色进行并行工作**。

作为架构师，我们**最重要的价值应该是“化繁为简”**。架构师的工作就是要努力训练自己的思维，用它去理解复杂的系统，通过合理的分解和抽象，使那些系统不再那么难懂。

![](/public/upload/architecture/application_architecture.JPG)

六边形架构、洋葱圈架构、以及COLA架构的核心职责就是要做核心业务逻辑和技术细节的分离和解耦。

## 业务逻辑抽象

按照Wikipedia上的解释，抽象是指为了某种目的，对一个概念或一种现象包含的信息进行过滤，移除不相关的信息，只保留与某种最终目的相关的信息。例如，一个 皮质的足球 ，我们可以过滤它的质料等信息，得到更一般性的概念，也就是 球 。从另外一个角度看，抽象就是简化事物，抓住事物本质的过程。

OOP可以看作一种在程序中包含各种独立而又互相调用的对象的思想，这与传统的思想刚好相反：**传统的程序设计主张将程序看作一系列函数的集合**，或者直接就是一系列对电脑下达的指令。

当发现有些东西就是不能归到一个类别中时，我们应该怎么办呢？此时，我们可以通过拔高一个抽象层次的方式，让它们在更高抽象层次上产生逻辑关系。比如，你可以合乎逻辑地将苹果和梨归类概括为水果，也可以将桌子和椅子归类概括为家具。但是怎样才能将苹果和椅子放在同一组中呢？仅仅提高一个抽象层次是不够的，因为上一个抽象层次是水果和家具的范畴。因此，你必须提高到更高的抽象层次，比如将其概括为“商品”。

在程序设计中，也是一样，如果在一个类或者一个函数中涉及过多的内容和概念，我们大脑也会显得不知所措，会觉得很复杂，不能理解。将一个大方法，按照逻辑关系，整理成一组更高层次的小而内聚的子程序的集合，那么整个代码逻辑就会呈现出完全不一样的风貌，显得干净、容易理解的多。

分层是一种常见的根据系统中的角色（职责拆分）和组织代码单元的常规实践。

[你写的代码是别人的噩梦吗？从领域建模的必要性谈起](https://mp.weixin.qq.com/s/UHrJ-6ruC_HkhUXvWvDX0A)

## 写复杂业务逻辑的方法论

很多人认为做业务开发显得没那么有挑战性，但其实正好相反。最难解决的bug是无法重现的bug，最难处理的问题域是不确定性的问题域。业务往往是最复杂的，面向不确定性设计才是最复杂的设计。软件工程学科最难的事情是抽象，因为它没有标准、没有方法、甚至没有对错。

### 过程分解和对象建模相结合

![](/public/upload/architecture/structure_and_object.jpg)

### ”能力下沉“

[阿里高级技术专家方法论：如何写复杂业务代码？](https://mp.weixin.qq.com/s/pdjlf9I73sXDr30t-5KewA)

![](/public/upload/architecture/ability_down.jpg)

一般来说实践DDD有两个过程：

1. 套概念阶段：了解了一些DDD的概念，然后在代码中“使用”Aggregation Root，Bounded Context，Repository等等这些概念。更进一步，也会使用一定的分层策略。然而这种做法一般对复杂度的治理并没有多大作用。
2. 融会贯通阶段：术语已经不再重要，理解DDD的本质是统一语言、边界划分和面向对象分析的方法。

指导下沉有两个关键指标：

1. 复用性，复用性是告诉我们When（什么时候该下沉了），即有重复代码的时候
2. 内聚性，内聚性是告诉我们How（要下沉到哪里），功能有没有内聚到恰当的实体上，有没有放到合适的层次上（因为Domain层的能力也是有两个层次的，一个是Domain Service这是相对比较粗的粒度，另一个是Domain的Model这个是最细粒度的复用）。

### 矩阵思维

[面对复杂业务，if-else coder 如何升级？](https://mp.weixin.qq.com/s/u7geoZNpLtfr_crkXVHjAg)业务的差异性是if-else的根源。要如何消除这些讨厌的if-else呢？我们可以考虑以下两种方式：
1. 多态扩展：利用面向对象的多态特性，实现代码的复用和扩展。
2. 代码分离：对不同的场景，使用不同的流程代码实现。这样很清晰，但是可维护性不好。

结构化思维有用、很有用、非常有用，只是它更多关注的是单向维度的事情。比如我要拆解业务流程，我要分解老板给我的工作安排，我要梳理测试用例，都是单向维度的。而复杂性，通常不仅仅是一个维度上的复杂，而**是在多个维度上的交叉复杂性**。当问题涉及的要素比较多，彼此关联关系很复杂的时候，两个维度肯定会比一个维度要来的清晰，这也是为什么说矩阵思维是比结构化思维更高层次的思维方式。

![](/public/upload/architecture/matrix_analysis.png)

除了工作，生活中也到处可见多维思考的重要性。比如，我们说浪费可耻，应该把盘子舔的很干净，岂不知加上时间维度之后，你当前的舔盘，后面可能要耗费更多的资源和精力去减肥，反而会造成更大的浪费。我们说代码写的丑陋，是因为要“快速”支撑业务，加上时间维度之后，这种临时的妥协，换来的是意想不到的bug，线上故障，以及无止尽的996。简单的思考是“点”状的，比如舔盘、代码堆砌就是当下的“点”；好一点的思考是“线”状，**加上时间线之后，不难看出“点”是有问题的**；再全面一些的思考是“面”（二维）；更体系化的思考是“体”（三维）；比如，RFM模型就是一个很不错的三维模型。可惜的是，在表达上，我们人类只能在二维的空间里去模拟三维，否则四维可能会更加有用。

我们在做矩阵分析的时候，纵轴可以选择使用业务场景，横轴是备选维度，可以是受场景影响的业务流程（如文章中的商品流程矩阵图），也可以是受场景影响的业务属性（如文章中的订单组成要素矩阵图），或者任何其它不同性质的“东西”。

![](/public/upload/architecture/battle_complexity.png)

“业务理解-->领域建模-->流程分解-->多维分析”是体力，是因为实现它们就像是在做填空题，只要你愿意花时间，再复杂的业务都可以按部就班的清晰起来。PS: 有了思维模型，就是在做填空题

### 自上而下的结构化分解+自下而上的面向对象分析

[一文教会你如何写复杂业务代码](https://mp.weixin.qq.com/s/nUQ1HfK0vwFMiGflhnw_AQ)
1. 说实话，能想到分而治之的工程师，已经做的不错了，至少比没有分治思维要好很多。我也见过复杂程度相当的业务，连分解都没有，就是一堆方法和类的堆砌。使用过程分解之后的代码，比以前的代码更清晰、更容易维护了。不过，还有两个问题值得我们去关注一下：
    1. 领域知识被割裂肢解。过程化拆解导致没有一个聚合领域知识的地方。每个 Use Case 的代码只关心自己的处理流程，知识没有沉淀。相同的业务逻辑会在多个 Use Case 中被重复实现，导致代码重复度高，即使有复用，最多也就是抽取一个 util，代码对业务语义的表达能力很弱，从而影响代码的可读性和可理解性。
    2. 代码的业务表达能力缺失。试想下，在过程式的代码中，所做的事情无外乎就是取数据 -- 做计算 -- 存数据，在这种情况下，要如何通过代码显性化的表达我们的业务呢？说实话，很难做到，因为我们缺失了模型，以及模型之间的关系。脱离模型的业务表达，是缺少韵律和灵魂的。
2. 在系统中引入了更加贴近现实的对象模型（CombineBackOffer 继承 BackOffer），对象模型更加清晰的还原了业务语义，多态可以消除我们代码中的大部分的 if-else。

    ![](/public/upload/architecture/divide.png)

在现实业务中，很多的功能都是用例特有的（Use case specific）的，如果“盲目”的使用 Domain 收拢业务并不见得能带来多大的益处。相反，这种收拢会导致 Domain 层的膨胀过厚，不够纯粹，反而会影响复用性和表达能力。

我们承认模型不是一次性设计出来的，而是迭代演化出来的。不强求一次就能设计出 Domain 的能力，也不需要强制要求把所有的业务功能都放到 Domain 层，而是采用实用主义的态度，即只对那些需要在多个场景中需要被复用的能力进行抽象下沉，而不需要复用的，就暂时放在 App 层的 Use Case 里就好了。PS：Use Case 是《架构整洁之道》里面的术语，简单理解就是响应一个 Request 的处理过程。

![](/public/upload/architecture/capability_sink.png)

复用性是告诉我们 When（什么时候该下沉了），即有重复代码的时候。内聚性是告诉我们 How（要下沉到哪里），功能有没有内聚到恰当的实体上(甚至要新建一个实体 来容纳很多类似于 `StringUtils.equals("xx","xx")` 的逻辑)，有没有放到合适的层次上（因为 Domain 层的能力也是有两个层次的，一个是 Domain Service 这是相对比较粗的粒度，另一个是 Domain 的 Model 这个是最细粒度的复用）。

有过程分解要好于没有分解，过程分解+对象模型要好于仅仅是过程分解。做不好业务开发的，也做不好技术底层开发，反之亦然。业务开发一点都不简单，只是我们很多人把它做‘简单’了

![](/public/upload/architecture/capability.png)

## 错误的应对方式

[对抗软件复杂度的战争](https://mp.weixin.qq.com/s/Dil5Ual1aI_7dsGKV0f6Ig)面对效率地不断下降，研发团队的管理者必须做点什么。不幸的是，很多管理者并不明白效率的降低是由软件复杂度的上升造成的，更没有冷静地去思考复杂度蔓延直至爆炸的根因是什么，于是我们看到许多管理者其肤浅的应对方式收效甚微，甚至起到了反作用。
1. 最常见的错误方式是设置一个不可更改的 Deadline，用来倒逼研发团队交付功能。但无数经验告诉我们，软件研发就是在质量、范围和时间这个三角中求取权衡。研发团队短期可以通过加班，牺牲假期等手段来争取一些时间（长期加班实际有百害无一利），但如果这个时间限制过于苛刻，那必然就要牺牲需求范围和软件质量。当需求范围不可缩减的时候，唯一可以被牺牲的就只有质量了，这实际就意味着在很短的时间内往系统中倾泻大量的偶然复杂度。
2. 另一种做法是用“更先进”的技术去替换现有系统的技术，例如用 Java 的微服务体系技术去替换 PHP + Golang 体系的技术；或者用支撑过成功商业产品的中台类技术去替换原来的微服务体系技术；或者简单到用云产品去替换自建的开源服务。这些做法背后的基本逻辑是，“更先进”的技术在成功的商业场景中被验证过，因此可以被直接拿来解决现有的问题。但在现实情况下，决策者往往忽略了当前的问题是否是“更先进”的技术可以解决的问题。如果现有的系统服务的用户在迅速增长，Scalablity 面临了严重的困境，那么这个答案是肯定的；如果现有的系统的稳定性堪忧，经常不可用且严重影响了用户体验，那么这个答案是肯定的。但是，如果现有的软件系统面临着研发效率下降问题，那么“更先进”的技术不仅帮不了什么忙，还会因为新老技术的切换给系统增加偶然复杂度。

## 《能力陷阱》

发展人际关系让你觉得卑鄙，虚伪？当我们把人际关系定义为本质上是为了实现自我利益时，甚至有些肮脏时，会限制我们的领导力，限制我们的发展前进，限制我们扩展视野，会阻碍我们了解新想法、发展自己其他方面才能的机会。

Be true to yourself or Shape-shifters （"坚持做自己”，还是”随机应变“）? 对于那些“坚持做自己”的人，作者说你要先知道那个You的定义是什么，是过去的“你”，先现在的“你”，还是未来的“你”呢？是那个固步自封，不敢改变的你，还是那个越来越圆融，知道如何应对变化，可以随机应变的你呢？ 所以，坚持做自己很多时候也是自己欺骗自己，不敢跳出舒适圈，不敢做出改变的谎言。Shape-shifters（随机应变者）是指那些很自然可以适应新环境的人，**他们并不会产生一种觉得自己很虚假的内疚感**。“随机应变者”有一个核心的自我价值观和目标，他们不担心转变自己会对自己的信仰造成影响。

时间是有限的，越是在最忙的时候，越需要空出一些时间来思考做这些事情的原因和目的，不能一直闷着头做。时间安排好，并不是一件特别困难的事。**领导者，应该把注意点放在一些非常重要的事情上。然后，其它的时间都要用来自我提升**，而不是那些“没有回报的事情”。就是要多花时间在自我提升（学习，写作，分享）上，多花时间对外产生连接，拓展人际网络，扩大影响力，吸引更多人才，从而为团队和组织带来更大贡献
