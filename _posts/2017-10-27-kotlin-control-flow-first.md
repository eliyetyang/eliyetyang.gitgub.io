
if表达式

普通的if我们普通的摇，基本用法与java相同

kotlin中木有“boolean？then:else” if 就够了，比如酱紫
val max = if (a > b) a else b
这里也可以更换成方法块如
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}

when表达式

kotlin中使用when替换原有的switch，基本用方法如下
when (x) {
    1 -> print("x == 1")//这里像case
    2 -> print("x == 2")
    else -> { //这里像default
        print("x is neither 1 nor 2")
    }
}
条件还可以和起来写，如下
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
