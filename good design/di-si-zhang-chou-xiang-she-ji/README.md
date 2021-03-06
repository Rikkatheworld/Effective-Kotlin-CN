# 第四章：抽象设计

抽象是编程世界中最重要的概念之一。在面向对象编程中， 抽象是三个核心概念之一（还有封装和继承）。在函数式编程社区中，人们通常会说我在编程中所做的一切就是抽象和组合（composition）。如你所见，我们都在认真地对待抽象。什么是抽象？ 我发现维基百科的解释就很有帮助：

> 抽象是泛化的过程或结果，是属性的去除，是观念与对象的距离 （https://en.wikipedia.org/wiki/Abstraction\_(disambiguation)）

换句话所，我们所说的抽象是指一种用来隐藏复杂性的简化形式。编程的一个基本例子就是接口。它是一个类的抽象，因为它仅表达了一个特征的子集， 具体的说，它是一组方法和属性。

![](<../../.gitbook/assets/image (11).png>)

对于每个实例，没有单一的抽象。比如对象就会有很多抽象，它可以通过实现多个接口或多个超类来表示。这是抽象的一个不可分割的特征：它决定什么应该隐藏，什么应该暴露。

![](<../../.gitbook/assets/image (6) (1).png>)

### 编程中的抽象

我们经常忘记我们在编程中如何去抽象一切。当我们输入一个数字时，很容易忘记它实际上是由 0 和 1表示的，当我们键入某个 String 时，很容易忘记它是一个复杂的对象，其中的每个字符都是用定义好的字符集来表示的，例如 UTF-8。

设计抽象不仅仅是分离模块或库。无论何时定义一个函数，都将其实现隐藏在该函数的签名后面，这就是一种抽象！

让我们思考一个问题：如果你不能使用 `maxOf` 来定义一个方法，来返回两个数字中最大的一个，应该怎么办：

```kotlin
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

当然，我们可以不用定义这个函数，只要编写完成整的表达式，并且永远不要显式的提到 `maxOf`，如下代码：

```kotlin
val biggest = if (x > y) x else y 

val height =
    if (minHeight > calculatedHeight) minHeight
    else calculatedHeight
```

然而，这将使我们处于不利的境地，它迫使我们始终在语言中刚好是原语的特定操作级别上工作（在本例中是比较操作），而不是在更高级别的层面上工作。 我们的程序能够计算出哪个数更大，但我们的语言却无法表达选择更大的数的这种概念。直到第8个版本，Java 还缺乏在列表上轻松表示映射（`map`）的功能，我们反而必须使用重复的结构来表达这个概念：

```kotlin
// Java
List<String> names = new ArrayList<>();
for (User user : users) {
    names.add(user.getName());
}
```

在 Kotlin 中，从一开始我们就可以用一个简单的函数来表示它：

```kotlin
val names = users.map { it.name }
```

惰性属性初始化模式至今仍不能在 Java 中表示，而在 Kotlin 中我们可以使用属性委托来做到：

```kotlin
val connection by lazy { makeConnection() }
```

天知道还有多少的概念是我们不知道的，它们是如何被提取和表达出来的。

一个强大的编程语言应该具备的特征之一就是：通过给一些常规的模式予以名称，从而建立抽象概念。在最基本的编程形式中，我们是通过提取函数、使用委托、抽象类等来实现的。因此，我们可以直接根据抽象进行工作。

### 以车喻码

开车的时候会遇到很多事情，它需要发动机、交流发电机、悬挂系统和许多其他元件的协调工作。 想象一下，如果驾驶一辆车需要实时理解和遵循这些元素，那该有多难！值得庆幸的是，我们不用这样。 作为一名司机，我们所需知道的就是如何使用汽车提供的接口 —— 方向盘、变速杆、踏板， 来操作车辆。引擎盖下的一切都可以改变，一个机械师可以在我们不知道的情况下，把汽油变成天然气，再变成柴油。 随着汽车引入越来越多的电子元件和特殊系统，大部分的接口仍然保持不变。 随着引擎盖下的这些变化，汽车的性能可能也会发生变化，但无论如何，我们都能驾驭汽车。

汽车有定义明确的接口，尽管组件很复杂，但使用起来很简单。 方向盘代表左右方向改变的抽象，换挡器代表前后方向改变的抽象，油门踏板代表加速，刹车代表减速。这些都是我们在汽车上所需要的，这些都是抽象，它是一种魔法：隐藏了所有发生在引擎盖下的事情。因此，用户不需要了解汽车的构造，他们只需要如何驾驶它，同样，汽车制造者或爱好者可以改变车里的一切，只要能够正常驾驶就没有问题。 记住这个比喻，我们将在整个章节中提到它。

类似地，在编程中，我们主要使用抽象来：

* 隐藏复杂性
* 组织我们的代码
* 给与发明者改变的自由

第一个的原因已经在_第三章：可重用性_中描述过了，我认为现在已经很清楚为什么提取函数、类或委托来重用通用逻辑或通用算法是很重要的。 在_第26条：每个函数都应该基于一个抽象层次来编写_，我们将看到如何使用抽象来组织代码，在_第27条：使用抽象来保护代码不受更改_，我们将看到如何使用抽象来给我们更改的自由。然后我们将用本章的其余部分来创建和使用抽象。

这是要一个相当高层次的章节，在本章后， 我们将在_第五章：对象的创建_和_第六章：类的设计_中讨论面向对象的一些更具体的方面。它们将深入到类实现和使用的更深入的方法层面，但它们的知识点都是基于本章的。
