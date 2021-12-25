## 集合

在Scala中集合是一个通用概念，包括了数组，列表，集合(Set)和 字典(Map)

不可变集合与可变集合

```scala
// 同时引入两个集合会导致歧义
import scala.collection.immutable.Set
import scala.colllection.mutable.Set
```

如果要合并两个Set来创建一个新的Set，那么我们可以使用++()方法
```scala
val merged = set1 ++ set2
```
集合的交集
```scala
val intersection = set1 & set2
```

**Map**

初始化

```scala
val map = Map("name" -> "nongfu", "type" -> "water")
```

在Map的get方法中，获取一个键的值可能不存在，因此其返回的是一个`Option`

此外，我们可以使用apply()方法来获取一个键的值。需要牢记的是，这是我们在类或者实例后加上圆括号时，Scala调用的方法。但是，和get()方法不同的是，apply()方法返回的不是Option[T]，而是返回的值（即T）。小心使用——确保将代码放置在一个try-catch代码块中

要添加值，请使用updated()方法。因为我们使用的是不可变集合，所以updated()方法不会影响原来的Map

**List**

如果我们想要前插一个元素，即将一个元素放在当前List的前面，我们可以使用特殊的`::()`方法
```scala
val numbers = List(1,2,3)
val modified = 4 :: numbers
```

需要注意的是，将元素或者列表追加到另外一个列表中，实际上调用的是后者的前缀方法。这样做的原因是，与遍历到列表的最后一个元素相比，访问列表的头部元素要快得多。事半功倍。

```scala
val numbers = List(1,2,3)
val merged = numbers ::: List(4)
```

如果方法名以冒号（:）结尾，那么调用的目标是该操作符后面的实例。Scala不允许使用字母作为操作符的名称，除非使用下划线对该操作符增加前缀。

除了以：结尾的操作符之外，还有一组调用目标也是跟随它们之后的实例的操作符。这些都是一元操作符，分别是+、-、！和～。其中一元+操作符被映射为对unary_+()方法的调用，而一元-操作符被映射为对unary_-()方法的调用，以此类推。

**for表达式**

集合上的迭代器

如果在for表达式中提供了多个生成器，那么每个生成器都将形成一个内部循环。最右边的生成器控制最里面的循环。