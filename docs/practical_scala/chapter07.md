## 特质(Trait)

横切关注点(Crosscutting Concerns): 在多个程序中都会应用到的部分，例如日志，安全验证，过滤器，资源管理等；在单一的类层次结构上，很难实现这个功能；Scala使用特质解决了这个问题

什么是特质?

特质类似于带有部分实现的接口，实现了位于单继承和多继承中间的能力；一个类中可以混入多个特质。

特质中，已经被定义但未被初始化的val/var变量被认为是抽象的，混入特质的类需要实现它们。

可以混入任意多的特质，如果已经使用extends扩展了超类，可以使用with混入特质

特质的构造器不能接受任何参数

```scala
class Dog extends Animal with Friend {}
```

> 多重继承通常会带来方法冲突，而特质则不会。通过延迟绑定类中由特质混入的方法，就可以避免方法冲突。

选择性混入

在实例级别进行混入，而不是在类上，实现更加灵活的混入

```scala
val cat = new Cat("kitty") with Friend
val friend: Friend = cat
friend.listen()
```

**特质实现装饰器模式**

Scala中，特质可以是独立的，也可以扩展自某个类。

在特质中，使用super来调用方法将会触发延迟绑定（late binding）。这不是对基类方法的调用。相反，调用将会被转发到混入该特质的类中。如果混入了多个特质，那么调用将会被转发到混入链中的下一个特质中，更加靠近混入这些特质的类。

```scala
abstract class Check {
    def check: String = "Checked Application Details..."
}

trait CreditCheck extends Check {
    override def check: String = s"Checked Credit... ${super.check}"
}

trait EmploymentCheck extends Check {
    override def check: String = s"Checked Employment...${super.check}"
}

trait CriminalRecordCheck extends Check {
    override def check: String = s"Check Criminal Records...${super.check}"
}

val employmentApplication = new Check with CriminalRecordCheck with EmploymentCheck

println(employmentApplication.check)
```

**特质中方法的延迟绑定**

如果我们从这个抽象类扩展了一个特质，并用super调用该抽象方法，那么Scala将会要求我们将该方法声明为abstract override。组合使用这两个关键字是很奇怪的，但是传达了双重意图。通过使用关键字override，我们表明我们打算为某个在基类中声明的方法提供一个实现。同时，我们还表示，这个方法的最终实际实现将由混入了该特质的类提供。

Scala巧妙地避免了方法冲突的问题，并将方法调用按照从右到左的顺序进行链接，最后一个混入的特质具有拦截方法调用的最高优先级