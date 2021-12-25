## 函数值和闭包

函数如果只能接受其他类型的参数作为输入，那么它和普通的方法没有什么区别；高阶函数让函数的能力有了质的飞跃。

在Scala中，函数值实际上就是对象。

局部函数，定义在另一个函数内部的函数

在最简单的情况下，函数可以没有参数，只返回对象，类似于工厂模式；可以对比js里的构造函数

**柯里化(currying)**

将接受多个参数的函数转化为接收多个参数列表的函数；如何将一个普通函数柯里化?

部分应用函数

参数占位符: Scala用下划线（_）这个记号来表示一个函数值的参数。

> 调整Scala的简洁度到一个折中点，以达到你对可读性的要求。在利用Scala代码简洁性的同时，不要让代码变得含义模糊，一定要尽力保持在一个平衡点上。

没有必要把函数值赋值给变量，直接传递函数名就可以了，这是复用函数值的一种方式

**部分应用函数**

如果传递的参数比所要求的参数少，就会得到另外一个函数。这个函数被称为部分应用函数。部分应用函数使绑定部分参数并将剩下的参数留到以后填写变得很方便

如何得到部分应用函数?

1. 使用占位符`_`， `val f1 = sum _`，会得到一个函数值f1，执行时调用原函数sum，相当于得到了一个别名
2. 绑定部分参数，`val f1 = sum(1, _: Int, 3)`，得到一个 `f: Int => Int`，传入后调用原函数sum

好处是可先绑定部分不变的参数，得到一个函数值，接收变化的参数，例如日志程序

```scala
def log(level: String, message: String): Unit = {
    level match {
        case "debug" => println(s"debug: $message")
        case "warning" => println(s"warning: $message")
        case _ => println(s"common info: $message")
    }
}
val debug = log("debug", _: String)
```

**Execute Around Mehtod**

类似于Python里的with关键字，可以提供给使用者需要的资源，在结束后可以自动清理，例如关闭文件

```scala
class Resource private () {
  println("Starting to allocate resource...")
  def cleanUp(): Unit = println("Do some clean up actions...")
  def op1(): Unit = println("operation 1")
  def op2(): Unit = println("operation 2")
  def op3(): Unit = println("operation 3")
}

object Resource {
  def use(fn: Resource => Unit): Unit = {
    val resource = new Resource
    try {
      fn(resource)
    } finally {
      resource.cleanUp()
    }
  }
}
```