GitFlow 是由 Vincent Driessen 提出的一个 git操作流程标准。包含如下几个关键分支：

* **master**
  ：主分支develop：主开发分支，包含确定即将发布的代码；
* **feature**
  ：新功能分支，一般一个新功能对应一个分支，对于功能的拆分需要比较合理，以避免一些后面不必要的代码冲突；
* **release**
  ：发布分支，发布时候用的分支，一般测试时候发现的 bug 在这个分支进行修复；
* **hotfix**
  ：热修复分支，紧急修 bug 的时候用。

GitFlow 的优势有如下几点：

* **并行开发**
  ：GitFlow可以很方便的实现并行开发。每个新功能都会建立一个新的 feature分支，从而和已经完成的功能隔离开来，而且只有在新功能完成开发的情况下，其对应的 feature分支才会合并到主开发分支上（也就是我们经常说的develop分支）。另外，如果你正在开发某个功能，同时又有一个新的功能需要开发，你只需要提交当前 feature 的代码，然后创建另外一个feature 分支并完成新功能开发。然后再切回之前的 feature 分支即可继续完成之前功能的开发。
* **协作开发**
  ：GitFlow 还支持多人协同开发，因为每个 feature 分支上改动的代码都只是为了让某个新的 feature 可以独立运行。同时我们也很容易知道每个人都在干啥。
* **发布阶段**
  ：当一个新 feature 开发完成的时候，它会被合并到 develop 分支，这个分支主要用来暂时保存那些还没有发布的内容，所以如果需要再开发新的 feature，我们只需要从 develop 分支创建新分支，即可包含所有已经完成的 feature 。
* **支持紧急修复**
  ：GitFlow 还包含了 hotfix 分支。这种类型的分支是从某个已经发布的 tag 上创建出来并做一个紧急的修复，而且这个紧急修复只影响这个已经发布的 tag，而不会影响到你正在开发的新 feature。


