[DocumentSrc](http://www.critterai.org/projects/nmgen_study/)

# 学习：导航网格生成

> 该项目不再积极维护。

NMGen研究是对[Recast](https://github.com/memononen/recastnavigation)的静态网格功能的Java改编，用于**研究和实验**。NMGen将任意的三角形网格作为输入，并生成表示源网格可达表面的数据。

本文档提供一个关于NMGen生成导航网格过程的完整概述。

"A navigation mesh is an abstract data structure used in artificial intelligence applications to aid agents in path-finding through large spaces. ... Meshes are typically implemented as graphs, opening their use to a large number of algorithms defined on these structures." -[Wikipedia](http://en.wikipedia.org/wiki/Navigation_mesh)

[img]

在参加了[AIGameDev.com](http://aigamedev.com/)的Recast大师班后，我开始对导航网格的生成感兴趣。两种非常有效的学习某物的方法，一是将它翻译成另外一种东西，在本例中是Java，另一种是向其他人解释此物为何，在本例中是完整的文档和可视化。

本文会对任何想要使用NMGen生成导航网格有更清晰了解的人有非常大的帮助。对那些想要更深入了解的人，NMGen源码被注释到了极致。

## 特别鸣谢

对[Mikko Mononen]将Recast变成让任何人都可以学习和使用。
对[AIGameDev.com](http://aigamedev.com/)的Alex J. Champandard展示Mikko和他的工作。

## 设计优先

（以重要性排序）
1. 个人学习。特别是计算几何学，体素化，以及相关的数据结构。
2. 对其它人的学习有用。
3. 对后续项目有用。例如：图形理论的学习和相关搜索算法。
4. 表现

基于这些优先级，文档会更详细，变量名会更具体，代码结构会更简洁。

## 此文档并非（限制）

第一也是最重要的一点，NMGen是改编，而不是[Recast](https://github.com/memononen/recastnavigation)的接口。Recast是高效且简洁的代码。由于NMGen的最高优先级是算法的学习，Recast的功能可能会被抛弃、重新组织、使用等。特别是以下：
1. NMGen仅涵盖Recast1.4版本中的静态网格功能。Recast有比NMGen中用到的更多的功能。Mikko也在不断改进Recast。
2. NMGen中大约有80%到90%的算法是与Recast匹配的，作为一个学习工具，已重构大部分的变量名和数据结构以用于提高实用性。
3. 高级的数据结构已经被改编用于适应Java标准。代码更倾向于面向对象而不是Recast所使用的面向过程。

NMGen功能齐全。但它也仅仅只是个原型。在没有调整和测试前它不适合在专业的项目中使用。
