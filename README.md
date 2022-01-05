# parse C programs with clang
原文地址：http://amnoid.de/tmp/clangtut/tut.html  

written by [Nico Weber](mailto:nicolasweber@gmx.de)

## 介绍

来自clang [官方网址](http://clang.llvm.org/):

```
Clang项目的目标是为LLVM编译器创建一个新的C、C++、Objective C and Objective C++的前端。
```

这是什么意思呢？前端是一个程序，它可以接受一段代码，并将其从一个flat string转换为同一程序的结构化、树形表示形式——也就是抽象语法树（下称AST）。

一旦你有了一个程序的AST，你可以用这个程序做很多没有AST很难做的事情。例如，在没有AST的情况下重命名变量是困难的：你不能简单地搜索旧变量名并用新变量名替换它，因为这会造成过度更改（例如，具有相同变量名但位于不同命名空间中的变量等等）。而使用AST的话很简单：原则上，您只需更改右侧VariableDeclaration AST节点的名称字段，然后将AST再转换回字符串。（实际上，对于某些代码库来说，这有点困难）。

前端已经存在了几十年。那么，clang有什么特别之处呢？我认为最有趣的部分是clang使用了一个库设计，这意味着你可以很容易地将clang嵌入到你自己的程序中（我是说“很容易”。本教程中的大多数程序都在50行以下）。本教程将告诉您如何做到这一点。

