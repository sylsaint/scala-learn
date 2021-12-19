## 从Java到Scala

隐式类型转换，让更多的方法可以定义在对象上

元组和多重赋值

Scala支持元组定义，和多重赋值，但是在赋值时需要保证左侧变量数量与右侧对应

使用数组传递变长参数:

```scala
val numbers = Array(1,2,3,4,5)
max(numbers: _*)
```

参数默认值

```scala
def maxOfTwo(first: Int = 10, second: Int = 20)
```

> 为参数补充默认值的行为发生在编译时

命名参数

```scala
def power(base: Int = 2, exponent: Int = 3): Int = {
    Math.pow(base, exponent)
}
```

> 注意: 
> 没有默认值的参数，必须提供值
> 有默认值的参数，可以选择提供参数
> 一个参数最多只能传一次
> 重载基类方法时，最好保持参数名一致；否则会优先使用基类方法
> 重载方法里，参数名一致但类型不同，会导致歧义，引起编译错误

隐式参数

> 可以让调用者绝对应该传递什么样的默认参数，具备了使用者修改默认参数的能力

隐式参数的定义需要在单独的参数列表里
在调用时，也可以显示地指定隐式参数

字符串和多行字符串

多行字符串使用 `"""..."""`

字符串插值

`s插值器`: 在字符串前面添加字符`s`
`f插值器`: 可以格式化输出字符串
`raw插值器`: 变量替换成之，但是会保留不可打印的字符

合理的约定

> 为了能够让scala本身更加简洁，表达能力更好，有一些约定

1. 支持脚本
2. 分号可选
3. 括号可选
4. return可选
5. 类和方法默认是public
6. 默认载入的包: `java.lang` `scala` `scala.Predef`

> `scala.Predef` 里主要定义了打印函数和集合类
> `scala` 里定义了基本的类型，方法，函数类型和trait类型

什么是部分函数(Partial Function)?

>  A partial function of type `PartialFunction[A, B]` is a unary function where the domain does not necessarily include all values of type `A`. The function `isDefinedAt` allows to test dynamically if a value is in the domain of the function.

就是说，我们定义了一个函数，但是这个函数的定义域只是输入的值域的一部分

操作符重载

技术上说，Scala没有操作符重载，因为语法的关系，让它看起来像是操作符重载，实际是方法调用

因为操作符存在优先级，所以Scala在方法上定义了优先级，模拟操作符优先级的效果

结合律: 左结合

**赋值**

Scala中不能多重赋值，每个赋值的结果是`Unit`

`==`的比较，Scala中的比较是基于值的，基于引用的比较使用`.eq`

**分号处理**

默认可以省略，在某行情况下需要

例如：在某些上下文中，`{`前需要添加`;`，因此，如果是想在创建一个实例之后新建一个代码块，就要写上分号。

**return**

避免显式return，scala会自动选择最后一个表达式的值作为返回值

**访问权限**

1. 默认方法是public
2. Scala对内部方法和变量的控制是细粒度的
3. Scala中的protected方法只有派生类可以访问，并且还有很灵活的用法
4. Scala中封装是类级别的，但是还可以控制只能是当前的实例方法访问

protected: 修饰的成员只能对自己和派生类可见；在访问一个实例的protected方法时，需要保证访问者和被访问者的类型一样

针对private，可以定制其可见性范围，语法为 `private[AccessQualifier]`

1. AccessQualifier未指定，当前类和伴生对象可见
2. AccessQualifier是类名，当前类和伴生对象以及对应的封闭类和伴生对象
3. AccessQualifier是封闭的包名，当前类和伴生对象和对应包下的所有类
4. AccessQualifier是this，那么只有当前实例可以访问

```scala
private[Abc] def getName = name
```