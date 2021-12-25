## 模式匹配

**匹配字面值和常量**

通配符`_`

```scala
name match {
    case "foo" => println(s"foo: $name")
    case "bar" => println(s"bar: $name")
    case _ => println("default")
}
```

**匹配元组和列表**

```scala
def processItems(items: List[String]): Unit = {
    items match {
    case List("apple", "ibm") => println("Apples and IBMs")
    case List("red", "blue", "white") => println("Stars and Stripes...")
    case List("red", "blue", _*) => println("colors red, blue, ... ")
    case List("apple", "orange", otherFruits @ _*) =>
        println("apples, oranges, and " + otherFruits)
    }
}

processItems(List("apple", "ibm"))
processItems(List("red", "blue", "green"))
processItems(List("red", "blue", "white"))
processItems(List("apple", "orange", "grapes", "dates"))
```

如果你使用大写字母的名称，Scala将会在作用域范围内查找常量。但是，如果使用的是非大写字母的名称，它将只假设其是一个模式变量——在作用域范围内任何具有相同非大写字母的名称都将会被忽略。

可以使用显式的作用域指定（如果ObjectName是一个单例对象或者伴生对象，那么使用ObjectName.filedName，如果obj是一个引用，则使用obj.fieldName），以便在case表达式中访问被隐藏的字段

也可以通过在变量名的两边加上反单引号（`）来给Scala提示

**使用case类进行模式匹配**

如果你有一个不带参数的case类，那么请在类名之后加上一个空括号，以表明它接受的是空的参数列表，否则Scala编译器会产生一个警告

当我们忘记了括号时，我们发送的将不是该case类的实例，而是其伴生对象。伴生对象混合了scala.Function0特质，意味着它可以被视为一个函数。因此，我们最终将会发送一个函数而不是case类的实例。


**提取器**

该提取器具有一个名为unapply()的方法，它接受我们想要匹配的值。当case Symbol()=>被执行的时候，match表达式将自动将input作为参数发送给unapply()方法

**正则表达式**

Scala的正则表达式买一赠一。当你创建了一个正则表达式时，将免费得到一个提取器。


无处不在的下划线(`_`)字符

1. 作为包引入的通配符
2. 作为元组索引的前缀, `tup._1`
3. 作为函数的隐式参数
4. 用于变量的默认值初始化
5. 用于在函数名中混合操作符
6. 模式匹配中作为通配符
7. 异常处理中，和case连用
8. 作为分解操作的一部分
9. 用于部分应用一个函数
