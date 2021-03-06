# 设计原则
我们编写了这个文档，以便您更好地了解我们如何确定React的作用，以及React的作用，以及我们的发展理念是什么。虽然我们很高兴看到社区的贡献，但是我们不可能选择违反这些原则中的一个或多个的途径。

> **注意：** 本文假定对React有深入的理解。它描述了React本身的设计原则，而不是React组件或应用程序。 
> 有关React的介绍，请查看[Thinking in React][1]。

### 组成
React的关键特征是组件的组成。不同人写的组件应该一起工作。对于我们来说重要的是你可以添加功能到一个组件，而不会在整个代码库造成波动的变化。

例如，应该可以将一些本地状态引入到组件中，而不改变使用它的任何组件。同样，在必要时应该可以添加一些初始化和拆卸代码给任何组件。

在组件中使用状态或生命周期钩子并没有什么坏处。像任何强大的功能一样，它们应该适度使用，但我们无意删除它们。相反，我们认为它们是使React有用的组成部分。我们可能会在将来启用更多的功能模式，但是本地状态和生命周期钩子都将成为该模型的一部分。

组件通常被描述为“只是函数”，但在我们看来，它们不仅仅是有用的。在React中，组件描述任何可组合的行为，这包括渲染，生命周期和状态。一些像Relay这样的外部库增加了其他的职责，例如描述数据依赖关系。这些想法有可能以某种形式回到React中。

### 常见的抽象
一般来说，我们拒绝添加可以在用户空间中实现的功能。我们不想用无用的库代码来臃肿你的应用程序。不过，也有例外。

例如，如果React没有提供对本地状态或生命周期钩子的支持，人们就会为它们创建自定义的抽象。当多个抽象竞争时，React不能强制或利用其中任何一个的属性。它必须与最低的共同标准一起工作。

这就是为什么有时我们添加功能到React本身。如果我们注意到许多组件以不兼容或低效的方式实现某个特性，我们可能更愿意将其烧成React。我们不这么做。当我们这样做时，这是因为我们有信心提高抽象层次有利于整个生态系统。状态，生命周期钩子，跨浏览器事件规范化就是很好的例子。

我们经常和社区讨论这样的改善建议。您可以通过React问题跟踪器上的“大图”标签找到一些讨论。

### Escape Hatches(逃生舱)
react是务实的。这是由在Facebook写的产品的需求驱动。虽然它受到一些尚未完全成为主流的范例（如函数式编程）的影响，但对于具有不同技能和经验水平的广泛开发人员而言，它仍是一个明确的目标。

如果我们想要抛弃一个我们不喜欢的模式，我们有责任考虑所有现有的使用案例，并在我们弃用它之前教育社区有关替代方案。如果某些对构建应用程序有用的模式很难以声明的方式表达，我们将为其提供一个必要的API。如果我们无法找到一个在许多应用程序中找到必要的API的完美API，只要稍后有可能摆脱它，我们将提供一个临时的subpar工作API，并为将来的改进留下了空间。

### 稳定性
我们重视API的稳定性。在Facebook上，我们有超过2万个使用React的组件。包括Twitter和Airbnb在内的许多其他公司也是React的重要用户。这就是为什么我们通常不愿意改变公共API或行为。

但是，我们认为“无变化”意义上的稳定被高估了。它很快变成停滞。相反，我们更喜欢稳定性，因为“它在生产中被大量使用，当某些东西发生变化时，就有一个清晰的（最好是自动的）迁移路径。”

当我们弃用一个模式时，我们研究了它在Facebook的内部使用情况并添加了弃用警告。他们让我们评估变化的影响。如果我们发现现在还为时过早，有时我们会退出，而我们需要更有策略地思考如何让代码库达到为这一改变做好准备的程度。

如果我们确信这种​​变化不会造成太大的破坏性，并且迁移策略对于所有用例都是可行的，那么我们会向开源社区发布弃用警告。我们与Facebook以外的React的许多用户密切联系，并且监视流行的开源项目并指导他们修复这些弃用。

鉴于Facebook React代码库的庞大规模，成功的内部迁移通常是其他公司不会遇到问题的良好指标。尽管如此，有时候人们会指出我们还没有想到的其他用例，并且为它们添加逃生舱，或者重新思考我们的方法。

没有一个很好的理由，我们不会贬低任何东西。我们认识到，有时候贬值警告会引起沮丧，但我们增加了这些贬值，因为弃用清理了我们和社区中许多人认为有价值的改进和新功能。

例如，我们在React 15.2.0中添加了关于未知DOM道具的警告。很多项目都受到这个影响。但是修复这个警告是非常重要的，这样我们就可以将对自定义属性的支持引入到React中。我们添加的每个弃用背后都有这样的原因。

当我们添加弃用警告时，我们会保留当前主要版本的其余部分，并在下一个主要版本中更改行为。如果涉及大量的重复性手动工作，我们会发布一个codemod脚本，使其大部分更改自动化。 Codemods使我们能够在一个庞大的代码库中没有停滞地前进，我们也鼓励你使用它们。

您可以找到我们在[react-codemod][2]存储库中发布的codemod。

### 互通性
我们高度重视与现有系统的互操作性和逐步采用。 Facebook有一个巨大的非React代码库。它的网站使用了名为XHP的服务器端组件系统，React之前的内部UI库以及React本身。对于我们来说，任何产品团队都可以开始使用React来实现一个小功能，而不是重写他们的代码来打赌。

这就是为什么React提供逃生舱与可变模型一起工作，并试图与其他UI库一起工作的原因。您可以将现有的命令式UI包装到声明式组件中，反之亦然。这对逐步采用至关重要。

### Scheduling（调度）
即使您的组件被描述为函数，当您使用React时，也不会直接调用它们。每个组件都会返回需要呈现内容的描述，并且该描述可能包含用户编写的组件（如<LikeButton>）和平台特定的组件（如<div>）。在未来的某个时间点，React将“展开”<LikeButton>，并根据组件的呈现结果递归地将更改应用于UI树。

这是一个微妙的区别，但却是一个强大的区别。既然你不调用这个组件函数，但让React调用它，这意味着React有权力在必要时延迟调用它。在当前实现中，React递归地遍历树，并在单个tick中调用整个更新树的render函数。但是，将来可能会延迟一些更新以避免丢帧。

这是React设计中的一个常见主题。当新数据可用时，一些流行的库实现了“push”方法，其中计算被执行。然而，React坚持“pull”的方法，计算可以延迟到必要的时候。

React不是一个通用的数据处理库。这是一个建立用户界面的库。我们认为它是唯一定位在一个应用程序来知道现在哪些计算是相关的，哪些不是。

如果有什么东西在屏幕外，我们可以推迟任何与之相关的逻辑。如果数据比帧速率更快到达，我们可以合并批量更新。我们可以优先考虑来自用户交互的工作（比如点击按钮造成的动画）而不是重要的后台工作（比如刚刚从网络加载的新内容），以避免丢失帧。

要清楚，我们现在没有利用这个权利。然而，像这样做的自由是为什么我们更喜欢控制调度，为什么setState（）是异步的。从概念上讲，我们认为它是“安排更新”。

如果我们让用户直接使用功能反应规划的一些变体中常见的“推”式范例来构建视图，那么对调度的控制将更难获得。我们想要拥有“胶水”代码。

React的一个关键目标是在退回React之前执行的用户代码的数量是最小的。这确保了React保留了根据对UI所了解的内容来安排和拆分块的功能。

有一个内部的笑话，React应该被称为“Schedule”，因为React不想被完全“reactive”。

### 开发者体验
提供良好的开发人员经验对我们非常重要。

例如，我们维护[React DevTools][3]，通过它可以检查Chrome和Firefox中的React组件树。我们听说，它为Facebook工程师和社区带来了巨大的生产力提升。

我们也尽量多花一点时间来提供有用的开发者警告。例如，如果您以某种浏览器不理解的方式嵌套标记，或者在API中出现常见的拼写错误，则React会在开发中警告您。开发者警告和相关检查是React开发版本比生产版本慢的主要原因。

我们在Facebook内部看到的使用模式帮助我们了解常见的错误，以及如何尽早地预防这些错误。当我们添加新的功能时，我们试图预测常见的错误并警告它们。

我们一直在寻找改善开发者体验的方法。我们喜欢听取您的建议，并接受您的贡献，使其更好。

### 调试
当出现问题时，重要的是你有面包屑来追踪代码库中的错误来源。在React中，props和state就是面包屑。

如果您在屏幕上看到错误，您可以打开React DevTools，找到负责渲染的组件，然后查看道具和状态是否正确。如果是的话，你知道问题出在组件的render（）函数中，或者是render（）调用的某个函数。问题是孤立的。

如果状态错误，则知道该问题是由该文件中的一个setState（）调用引起的。这也是相对简单的定位和修复，因为通常在一个文件中只有一些setState（）调用。

如果props是错的，你可以在巡视员身上穿过树，寻找那些首先通过坏props“毒死”的组件。

这种跟踪任何用户界面的能力是以当前props和state的形式产生的，对React来说非常重要。这是一个明确的设计目标，即状态不是被封闭在闭包和组合器中，而是直接用于React。

虽然用户界面是动态的，但是我们相信props和state的同步渲染（）函数将调试从猜测变成了一个无聊而有限的过程。我们希望在React中保留这个约束，尽管它使复杂动画等一些用例更难。

### 配置
我们发现全局运行时配置选项是有问题的。

例如，偶尔要求我们实现一个像React.configure（options）或者React.register（component）这样的函数。然而，这带来了很多问题，我们不知道解决这些问题的好方法。

如果有人从第三方组件库调用这样的功能呢？如果一个React应用程序嵌入了另一个React应用程序，并且它们所需的配置不兼容？第三方组件如何指定它需要特定的配置？我们认为全局配置不适合工作。由于组合是React的核心，因此我们不提供代码中的全局配置。

但是，我们确实在构建级别上提供了一些全局配置。例如，我们提供独立的开发和生产版本。我们也可能会在未来添加一个性能分析版本，我们可以考虑其他构建标记。

### 超越DOM
我们看到React的价值在于它允许我们编写bug较少的组件，并且组合良好。 DOM是React的原始渲染目标，但React Native对Facebook和社区同样重要。

呈现器不可知是React的重要设计约束。它在内部表示中增加了一些开销。另一方面，对核心的任何改进都可以跨平台进行转换。

拥有单一的编程模型可以让我们围绕产品而不是平台组建工程团队。到目前为止，这种权衡对我们来说是值得的。

### 履行
我们尽可能提供优雅的API。我们不太关心实现是否优雅。现实世界远非完美，在合理的程度上，如果用户不需要写这些丑陋的代码，那么我们更喜欢把丑陋的代码放入库中。当我们评估新的代码时，我们正在寻找一个正确，高性能的实现，并提供良好的开发者体验。优雅是次要的。

相比巧妙的代码，我们更喜欢无聊的代码。代码是一次性的，经常改变。所以重要的是，除非绝对必要，否则不会引入新的内部抽象。详细的代码，易于移动，更改和删除优于代码过早抽象，难以改变。

### 优化工具
一些常用的API有冗长的名字。例如，我们使用componentDidMount（）而不是didMount（）或onMount（）。这是故意的。目标是使与图书馆的互动点显而易见。

在像Facebook这样庞大的代码库中，能够搜索特定API的使用是非常重要的。我们重视不同的详细名称，特别是应该谨慎使用的功能。例如，dangerouslySetInnerHTML在代码审查中很难忽略。

优化搜索也很重要，因为我们依靠codemods来做出重大改变。我们希望在代码库中应用大量的自动化更改非常简单和安全，而独特的详细名称可帮助我们实现这一点。同样，独特的名称可以很容易地编写有关使用React的自定义lint规则，而不用担心潜在的误报。

JSX起着类似的作用。虽然React并不需要它，但是我们为了审美和实用的原因在Facebook上广泛使用它。

在我们的代码库中，JSX为他们处理React元素树的工具提供了明确的提示。这使得可以添加构建时优化，如提升常量元素，安全lint和codemod内部组件使用，并将JSX源位置包含在警告中。

### 内部测试
我们尽力解决社区提出的问题。不过，我们可能会优先考虑人们在Facebook内部也遇到的问题。也许与直觉相反，我们认为这是社区可以在React上下注的主要原因。

沉重的内部使用让我们有信心，明天React不会消失。在Facebook上创建了React来解决它的问题。它为公司带来了有形的商业价值，并在许多产品中使用。 Dogfooding这意味着我们的愿景保持锐利，我们有一个重点方向前进。

这并不意味着我们忽视了社区提出的问题。例如，我们增加了对Web组件和SVG的支持，尽管我们不依赖于其中的任何一个。我们正在积极聆听您的痛点，并尽我们所能解决他们的问题。 React对我们来说是一个特殊的社区，我们很荣幸能够回馈。

在Facebook上发布了许多开源项目之后，我们了解到，试图让每个人都高兴，同时又制作了重点不太好的项目。相反，我们发现挑选一小部分观众，并专注于让他们高兴，带来了积极的净效应。这正是我们用React做的，到目前为止，解决Facebook产品团队遇到的问题已经转化为开源社区。

这种方法的不足之处在于，有时我们没有把足够的重点放在Facebook团队不需要处理的事情上，比如“入门”的经验。我们清楚地意识到这一点，我们正在考虑如何改进，以使社区中的每个人都受益，而不会像以前那样犯同样的错误。

  [1]: https://reactjs.org/docs/thinking-in-react.html
  [2]: https://github.com/reactjs/react-codemod
  [3]: https://github.com/facebook/react-devtools