---

title: kotlin Demo学习笔记

categories:

  - learn
  
tags:

  - kotlin
  
---

#####
>几个月前跟着 [Kotlin Koans](https://github.com/Kotlin/kotlin-koans) 了解了下 [Kotlin](https://kotlinlang.org/)
但是并没有做笔记，今天再想象几乎忘光了，看来学习还是要留下点什么，今天决定再重文以便，留下此博文，也用于与大家交流。
还没有下载demo的大家可以根据连接[下载](https://github.com/Kotlin/kotlin-koans)，也可以以直接通过[官方网站](https://kotlinlang.org/)进行学习与查阅。
做Android的朋友，我建议demo使用[intelliJ](https://www.jetbrains.com/idea/download/#section=linux)打开。

###开始正题 下面开始逐个讲解

#####0.HelloWorld

这个项目的todo中，说明了整个项目的要点。

>1. 从项目李的[README.md](https://github.com/Kotlin/kotlin-koans/blob/master/README.md)文件了解如何使用此项目及各种解决办法。
>2. 通过任务描述后的 **documentation=** 所跟方法，可以跳转到该问题对应的在线文档。
>3. 通过 **references =** 所跟方法，可以导航到描述中对应的方法。

好了，下面完成第一个任务，将task0()方法，返回 "OK".

```kotlin
fun task0(): String {
    return "OK"
}
```
#####1.JavaToKotlinConverter

这个是ide自动转换的练习，从.java文件中复制方法，然后粘贴到.kt中，编译器会提示是否要转换，
这样ide就可以自动将java翻译为kotlin了，这个功能对java转kotlin是不是很有好～。下面让我们使用
自动转换完成任务。

```kotlin
fun task1(collection: Collection<Int>): String {
    val sb = StringBuilder()
    sb.append("{")
    val iterator = collection.iterator()
    while (iterator.hasNext()) {
        val element = iterator.next()
        sb.append(element)
        if (iterator.hasNext()) {
            sb.append(", ")
        }
    }
    sb.append("}")
    return sb.toString()
}
```

#####2.NamedArguments

```kotlin
// default values for arguments
fun bar(i: Int, s: String = "", b: Boolean = true) {}

fun usage() {
    // named arguments
    bar(1, b = false)
}
```
这次是函数行参的讲解，从范例可以看出函数的行参可以在声明函数时直接对其赋值作为默认值。
此时就产生了一个疑问，是否拥有默认值时可选的，那么没有默认值的参数是否在使用时为必须参数？
一般来讲应该是必传的，int之类再java中不进行初始化可能会有默认值，但引用型就会是null。基于严谨，验证一下，
发现不传方法会报错。猜想正确，无默认值的行参在使用该函数时必须由外接传入进行初始化。

另一点，使用方法时，参数的传入是以类似赋值的形式进行的，就像java中 **变量名 = 值** 这种形式，
那么传入参数的位置也就不再受限制，有默认值的行参传入也成为可选项，对于没有传入但函数声明时存在默认值的行参，函数将使用其声明时的默认值。

这种声明及使用方式让我想起了重构中的以函数代替临时变量，他将临时变量的声明放在了函数的行参中，
在需要传入行参时可以直接调用其他方法，传入该函数，这样可能会让代码的可读性大大提高，例如如下代码。

```kotlin
fun usage() {
    getMedialNumberAndCauculate(
            max = getMax(),
            min = getMin(),
            weight = cauculateWeight(
                    cityCode = 10,
                    countryCode = 2
            )
    )
    
    //or
    
    getMedialNumberAndCauculate(
                getMax(),
                getMin(),
                cauculateWeight(
                    cityCode = 10,
                    countryCode = 2
                )
        )
}
```

把这里想象成好多参数和方法，但读起来却并不费尽，层次分明，每一个参数的由来功能都简单易懂。
下面让我们回到demo，任务中让我们使用 **collection** 的 **joinToString**方法，并且只使用
**prefix** 与 **arguments** 两个行参。看一下测试用例输出结果为集合的两头带大括号,所以前后两端插入。
代码如下：

```kotlin
fun task2(collection: Collection<Int>): String {
    return collection.joinToString(prefix = "{", postfix = "}")
}
```