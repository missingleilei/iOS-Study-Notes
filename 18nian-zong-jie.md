### 1. Swift和Objective-c语言的区别是什么？

这个问题其实很说简单其实很简单，但是重点却不是这个问题本身，而是那位面试官的原话`当你使用Swift编程的时候，你就不能使用Objective-C的思路去使用Swift了，你要知道Swift的特性是什么，不单单是针对OC的特性，而是这门语言本身的特性`， 确实，在听完这个回答之后，其实我非常的震撼，因为我以前确实都是在用使用Objective-C的方式去使用它，而忽略了这门语言自己独有的特性！！！

我从官网Swift的介绍中截取了它的几点特性：

```
- Closures unified with 
function
 pointers
- Tuples and multiple 
return
 values
- Generics
- Fast and concise iteration over a range or collection
- Structs that support methods, extensions, and protocols
- Functional programming patterns, e.g., map and filter
- Native error handling using try / catch / throw

```

其实在使用swift的使用，如果真的想去使用好，就得去在使用的过程中去思考_Swift的特性_有哪些，你有没有用到这些特点，如果你没有使用，那么你和使OC有区别吗？这是我需要考虑的事情，希望看到我文章的你也可以考虑考虑~

个人感觉可以从两个地方出发去充分使用Swift的特性：首先是它的语法特性（如上所写：闭包；元组；快速的迭代；支持方法，扩展，协议的结构体；函数式编程；原生的错误处理）；然后呢，就是编程思想的使用了，Swift的核心的编程思想，我觉得有两个一个是`函数式编程思想`，一个就是`面向协议的编程思想`，当然还有一种思想是`AOP(面向切面编程)` ，当然还有很多其他的思想，但是我所知的这三者应该是最常见的，希望我，也希望大家，在使用`Swift`的时候，去思考思考自己使用了这些`Swift`特性没有？使用了这些编程思想没有？如果没有考虑这些东西，就要去自我审查一下了~

### 2.在每一个方案的时候，一定要去考虑考虑你的算法是最优的吗？还有更好的解决方案吗？

确实，很多时候，在考虑方案的时候，甚至是基本的业务逻辑，同一个问题，是一定会有更好的方案的！但是在现在我却并没有去思考比较好的算法去解决问题，而是大多数情况下采用的是非常基本的面向过程的编程思维，这是不可取的也是不正确的方式。

那我在下阶段可能就去要去花费时间去仔细的钻研钻研算法这一块了，算法本身不就是程序员所必须掌握的基础技能吗？

### 3. 谈到组件化的方式，你提到了有通过路由的方式，协议的方式，以及Target-Action的方式等，来实现iOS项目的组件化的思想，聊聊他们？

当然这个问题是我抽象出来处理的问题，实际上呢，面试官的原话是什么呢？

```
你说你知道这三种方式，你觉得Target-Action的方式最好，那么其余的两种方式呢？你使用过吗？他们的建立一定是为了解决某些问题，如果没有足够的了解的话，你又如何知道在具体情况下使用哪种方案呢？

```

犹如醍醐灌顶，每一种方式都有它本身所拥有的好处，并不是说某一种方式就是最优的，那基于这个问题，其实还可以衍生很多知识点，希望大家都对架构这一块去研究研究吧，写出低耦合高内聚的代码。

### 4. 关于项目的构架MVP,MVVM,MVC，它们各自是什么，它们的优点分别是什么？

不同的软件的构架是针对于不同的项目的，没有一种构架是完美的，当我们使用习惯了MVVM的构架之后，它就一定是最好的吗？你使用它的目的是什么？你有没有充分利用了MVVM，还是说你只是使用了简单的文件夹层次的划分？MVC固然有坏处，那是你真的充分的理解了MVC的构架吗？

### 5. 知其然而知其所以然，SDWebImage库，你有没有阅读源码，当你去使用它的时候，它的流程你可以说出来，但是每一个异步下载图片的框架都是这个流程啊，它的优点在哪里？相比于其他的框架，它具备什么特点呢？

哎，不问不知道，一问就感觉自己非常的菜了，在使用第三方库的过程中根本就没有去仔细的思考这些问题，那么自己如何可以提高呢？看来使用第三方库的时候，去看看源码也是一个非常值得做的事情啊！
