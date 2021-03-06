# 第六章：类的设计

类是面向对象编程（OOP）范式中最重要的抽象。因为 OOP 是 Kotlin 中最流行的范式，所以类对我们来说也非常重要。本章讲解类的设计，而非工程架构设计，因为那需要花更多的内容，而且已经有很多关于这个主题的书了，比如 Robert C.Martin 的《架构整洁之道》 或者 Erich Gamma、John Vlissides、Ralph Johnson和Richard Helm的《设计模式》。相反的，我们将主要谈论 Kotlin 类所遵循的、满足的合约 —— 应该如何去使用 Kotlin 结构，以及在使用它们时应该期望从我们这里得到什么。什么时候、如何使用继承？我们期望如何使用数据类？什么时候我们应该使用函数类型而不是单一方法的接口？ `equals`、`hashCode`、 `compareTo` 的合约是什么？ 什么时候我们应该用扩展来替代成员？这些就是我要在该章节中解答的问题。它们都很重要，因为破坏它们可能会导致严重的问题，而遵循它们将帮助你使代码更加安全、清晰。
