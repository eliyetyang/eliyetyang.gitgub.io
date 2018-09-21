---

title: kotlin阅读笔记

categroies:

    - note

tags:
    - kotlin

---

## 阅读 《Android Development with Kotlin》

### kotlin的优秀之处

### 1. 可变声明

&emsp;kotlin可以根据上下文自动识别变量的类型。

例子：

```kotlin
var name = "Igor" // 识别name变量的类型为String
name = "Marcin"
```

这样编译器会报错：

```kotilin
var name = "Igor"
name = 2 // name 的类型在前一句代码中已经识别为 String ，无法接受Int类型。
```

### 2. 字符串模板（我这种交法不知对不对）

之前使用java的时候，在进行字符串与变量拼接时基本都是使用**StringBuilder** **StringBuffer**
或是 **+** 号进行拼接（线程不安全，线程安全，静态拼接），以及**String.format**函数。

在kotlin中，你可以直接使用字符串模板来进行替换，个人理解就和**String.format**一样，只不过写法更简单更强大了。

例子：
```kotlin
val name = "Eliyet"
println("My name is $name")
// 打印结果：  My name is Eliyet
```

<p>
更强大的是，你可以直接在表达式里使用函数，如下例。
</p>

```kotlin
val name = "Eliyet"
println("My name is ${name.toUpperCase()}")
//打印结果： My name is ELIYET
```

字符串中 **$** 可以表示一个占位符， **{}** 大括号里是一个表达式，其返回值为前占位符所对应的实际值。

### 3. 空指针

在java中，你可以为任何一个引用型变量赋值为null，这经常会造成空指针异常或是繁杂的非空判断。
但在kotlin中，你需要为变量指定其值是否可赋为null。例子如下：

```kotlin
var a: String = "abc"
a = null // 编译报错
var b: String? = "abc"
b = null // 正确
```

在变量类型后添加 **?** 表示此变量可以储存null，如果没有，则我们无法将null赋值给该变量，换句话说就是此变量必定非空。
而对于可空变量的函数调用，可以使用更剪辑的非空检查，进行方法调用，来避免空指针异常。例子如下：

```kotlin
savedInstanceState?.doSomething
```

这时**doSomething**只有在**savedInstanceState**对象非空的情况下才会调用，如果该对象为空，则不会执行。

### 4. 新增数据类型

- Range
    一个封闭的区间，包含结束。

    ```kotlin
    for (i in 1..10) {
        print(i)
    } // 12345678910
    ```
- Pair
    使用 **to** 连接的成对数据
    ```kotlin
    val capitol = "England" to "London"
    println(capitol.first) // Prints: England
    println(capitol.second) // Prints: London
    ```
    我们还可以用一种特殊的声明 *(destructive declarations)* 方式来对分开使用两个值
    ```kotlin
    val (country, city) = capitol
    println(country) // Prints: England
    println(city) // Prints: London
    ```
    也可以对其列表进行循环遍历
    ```kotlin
    val capitols = listOf("England" to "London", "Poland" to "Warsaw")//这里是个不可变列表，下面会有介绍。
    for ((country, city) in capitols) {
        println("Capitol of $country is $city")
    }
    // Prints:
    // Capitol of England is London
    // Capitol of Poland is Warsaw
    ```
    当然也可以使用**FroEach**语句
    ```kotlin
    val capitols = listOf("England" to "London", "Poland" to "Warsaw")
    capitols.forEach { (country, city) ->
        println("Capitol of $country is $city")
    }
    ```
- 可变与不可变
    在kotlin中，对应的有可变与不可变集合列表等，可变则前缀为**Mutable**，例如：
    （**List** 与 **MutableList**, **Set** 与 **MutableSet**, **Map** 与 **MutableMap** 等），
    kotlin提供了一系列的方法，用于快速创建对应的列表。
    ```kotlin
    val list = listOf(1, 2, 3, 4, 5, 6) // type is List
    val mutableList = mutableListOf(1, 2, 3, 4, 5, 6)// type is MutableList
    ```
    不可变集合（无**Mutable**前缀）则意味着集合创建后，数量不可变，也就是无法 **add**
    或者 **remove** 元素，相反可变集合则和之前java中的集合类似。

### 5. lambda表达式
这个java1.8后应该也比较熟悉了，只不过kotlin提供了更强大更简洁的lambda表达式支持，保证那些1.8之前的设备也没问题。
（至于性能是否与stream这种一样我还暂时没有研究。等到学的差不多了看看原理在做评论）
例如Android中常见的点击回调监听。

```kotlin
view.setOnClickListener {
    println("Click")
}
```

再来个复杂点的

```kotlin
val text = capitols.map { (country, _) -> country.toUpperCase() }//把上面代码中所声明的capitols中每一项country大写后输出一个新的列表
                   .onEach { println(it) }//循环打印列表中的元素 也就是country  打印结果 ENGLAND POLAND
                   .filter { it.startsWith("P") }//根据筛选条件进行筛选
                   .joinToString (prefix = "Countries prefix P:")//将列表与prefix字符串拼接
println(text) // Prints: Countries prefix P: POLAND
```

我们不是必须将参数转化为lambda表达式，但我们可以定义自己的lambda表达式，这可以让我们用一种全新的方式进行编成，
下列示例演示了只在Android Marshmallow以上的系统版本才执行的代码块。
```kotlin
inline fun supportsMarshmallow(code: () -> Unit) {
    if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
        code()
}
//usage
supportsMarshmallow {
    println("This code will only run on Android Nougat and newer")
}
```


































