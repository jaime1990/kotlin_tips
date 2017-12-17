# 怎么用Kotlin去提高生产力：Kotlin Tips

汇总Kotlin相对于Java的优势以及怎么用Kotlin去简洁、务实、高效、安全的开发，每个总结的小点tip都有详细的说明和案例代码。


## Tip1-更简洁的字符串

详见案例代码[KotlinTip1](https://github.com/heimashi/kotlin_tips/blob/master/app/src/main/java/com/sw/kotlin/tip1/KotlinTip1.kt)

Kotlin中的字符串基本Java中的类似，有一点区别是加入了三个引号"""来方便长篇字符的编写。
而在Java中，这些都需要转义，先看看java中的式例
```java
    public void testString1() {
        String str1 = "abc";
        String str2 = "line1\n" +
                "line2\n" +
                "line3";
        String js = "function myFunction()\n" +
                "{\n" +
                "    document.getElementById(\"demo\").innerHTML=\"My First JavaScript Function\";\n" +
                "}";
        System.out.println(str1);
        System.out.println(str2);
        System.out.println(js);
    }
```
kotlin除了有单个双引号的字符串，还对字符串的加强，引入了**三个引号**，"""中可以包含换行、反斜杠等等特殊字符：
```kotlin
/*
* kotlin对字符串的加强，三个引号"""中可以包含换行、反斜杠等等特殊字符
* */
fun testString() {
    val str1 = "abc"
    val str2 = """line1\n
        line2
        line3
        """
    val js = """
        function myFunction()
        {
            document.getElementById("demo").innerHTML="My First JavaScript Function";
        }
        """.trimIndent()
    println(str1)
    println(str2)
    println(js)
}
```
同时，Kotlin中引入了**字符串模版**，方便字符串的拼接，可以用$符号拼接变量和表达式
```kotlin
/*
* kotlin字符串模版，可以用$符号拼接变量和表达式
* */
fun testString2() {
    val strings = arrayListOf("abc", "efd", "gfg")
    println("First content is $strings")
    println("First content is ${strings[0]}")
    println("First content is ${if (strings.size > 0) strings[0] else "null"}")
}
```

## Tip2-Kotlin中大多数控制结构都是表达式

首先，需要弄清楚一个概念**语句和表达式**，然后会介绍控制结构表达式的优点：**简洁**
#### 语句和表达式是什么？
- 表达式有值，并且能作为另一个表达式的一部分使用
- 语句总是包围着它的代码块中的顶层元素，并且没有自己的值
#### Kotlin与Java的区别
- Java中，所有的控制结构都是语句，也就是控制结构都没有值
- Kotlin中，除了循环（for、do和do/while）以外，大多数控制结构都是表达式(if/when等)


详见案例代码[tip2](https://github.com/heimashi/kotlin_tips/blob/master/app/src/main/java/com/sw/kotlin/tip2)
#### Example1：if语句
java中，if 是语句，没有值，必须显示的return
```java
/*
* java中的if语句
* */
public int max(int a, int b) {
    if (a > b) {
        return a;
    } else {
        return b;
    }
}
```
kotlin中，if 是表达式，不是语句，因为表达式有值，可以作为值return出去
```kotlin
/*
* kotlin中，if 是表达式，不是语句，类似于java中的三木运算符a > b ? a : b
* */
fun max(a: Int, b: Int): Int {
    return if (a > b) a else b
}
```
上面的if中的分支最后一行语句就是该分支的值，会作为函数的返回值。这其实跟java中的三元运算符类似，
```java
/*
* java的三元运算符
* */
public int max2(int a, int b) {
    return a > b ? a : b;
}
```
上面是java中的三元运算符，kotlin中if是表达式有值，完全可以替代，**故kotlin中已没有三元运算符了**，用if来替代。
上面的max函数还可以简化成下面的形式
```kotlin
/*
* kotlin简化版本
* */
fun max2(a: Int, b: Int) = if (a > b) a else b
```

#### Example2：when语句
Kotlin中的when非常强大，完全可以取代Java中的switch和if/else，同时，**when也是表达式**，when的每个分支的最后一行为当前分支的值
先看一下java中的switch
```java
    /*
    * java中的switch
    * */
    public String getPoint(char grade) {
        switch (grade) {
            case 'A':
                return "GOOD";
            case 'B':
            case 'C':
                return "OK";
            case 'D':
                return "BAD";
            default:
                return "UN_KNOW";
        }
    }
```
java中的switch有太多限制，我们再看看Kotlin怎样去简化的
```kotlin
/*
* kotlin中，when是表达式，可以取代Java 中的switch，when的每个分支的最后一行为当前分支的值
* */
fun getPoint(grade: Char) = when (grade) {
    'A' -> "GOOD"
    'B', 'C' -> {
        println("test when")
        "OK"
    }
    'D' -> "BAD"
    else -> "UN_KNOW"
}
```
同样的，when语句还可以取代java中的if/else if，其是表达式有值，并且更佳简洁
```java
    /*
    * java中的if else
    * */
    public String getPoint2(Integer point) {
        if (point > 100) {
            return "GOOD";
        } else if (point > 60) {
            return "OK";
        } else if (point.hashCode() == 0x100) {
            //...
            return "STH";
        } else {a
            return "UN_KNOW";
        }
    }
```
再看看kotlin的版本，使用**不带参数的when**，只需要6行代码
```kotlin
/*
* kotlin中，when是表达式，可以取代java的if/else，when的每个分支的最后一行为当前分支的值
* */
fun getPoint2(grade: Int) = when {
    grade > 90 -> "GOOD"
    grade > 60 -> "OK"
    grade.hashCode() == 0x100 -> "STH"
    else -> "UN_KNOW"
}
```

## Tip3-更好调用的函数：显示参数名/默认参数值
Kotlin的函数更加好调用，主要是表现在两个方面：1，显示的**标示参数名**，可以方便代码阅读；2，函数可以有**默认参数值**，可以大大**减少Java中的函数重载**。
例如现在需要实现一个工具函数，打印列表的内容：
```kotlin
/*
* 打印列表的内容
* */
fun <T> joinToString(collection: Collection<T>,
                     separator: String,
                     prefix: String,
                     postfix: String): String {
    val result = StringBuilder(prefix)
    for ((index, element) in collection.withIndex()) {
        if (index > 0) result.append(separator)
        result.append(element)
    }
    result.append(postfix)
    return result.toString()
}
/*
* 测试
* */
fun printList() {
    val list = listOf(2, 4, 0)
    /*不标明参数名*/
    println(joinToString(list, " - ", "[", "]"))
    /*显示的标明参数名称*/
    println(joinToString(list, separator = " - ", prefix = "[", postfix = "]"))
}
```
如上面的代码所示，函数joinToString想要打印列表的内容，需要传人四个参数：列表、分隔符、前缀和后缀。
由于参数很多，在后续使用该函数的时候不是很直观的知道每个参数是干什么用的，这时候可以显示的标明参数名称，增加代码可读性。
同时，定义函数的时候还可以给函数默认的参数，如下所示：
```kotlin
/*
* 打印列表的内容，带有默认的参数，可以避免java的函数重载
* */
fun <T> joinToString2(collection: Collection<T>,
                      separator: String = ", ",
                      prefix: String = "",
                      postfix: String = ""): String {
    val result = StringBuilder(prefix)
    for ((index, element) in collection.withIndex()) {
        if (index > 0) result.append(separator)
        result.append(element)
    }
    result.append(postfix)
    return result.toString()
}
/*
* 测试
* */
fun printList3() {
    val list = listOf(2, 4, 0)
    println(joinToString2(list, " - "))
    println(joinToString2(list, " , ", "["))
}
```
这样有了默认参数后，在使用函数时，如果不传入该参数，默认会使用默认的值，这样可以避免Java中大量的函数重载。

## Tip4-扩展函数和属性
扩展函数和属性是Kotlin非常方便实用的一个功能，它可以让我们随意的扩展第三方的库，你如果觉得别人给的SDK的api不好用，或者不能满足你的需求，这时候你可以用扩展函数完全去自定义。
例如String类中，我们想获取最后一个字符，String中没有这样的直接函数，你可以用.后声明这样一个扩展函数：
```kotlin
/*
* 扩展函数
* */
fun String.lastChar(): Char = this.get(this.length - 1)
/*
*测试
* */
fun testFunExtension() {
    val str = "test extension fun";
    println(str.lastChar())
}
```
这样定义好lastChar()函数后，之后只需要import进来后，就可以用String类直接调用该函数了，跟调用它自己的方法没有区别。这样可以避免重复代码和一些静态工具类，而且代码更加简洁明了。
例如我们可以改造上面tip3中的打印列表内容的函数：
```kotlin
/*
* 用扩展函数改造Tip3中的列表打印内容函数
* */
fun <T> Collection<T>.joinToString3(separator: String = ", ",
                                    prefix: String = "",
                                    postfix: String = ""): String {
    val result = StringBuilder(prefix)
    for ((index, element) in withIndex()) {
        if (index > 0) result.append(separator)
        result.append(element)
    }
    result.append(postfix)
    return result.toString()
}

fun printList4() {
    val list = listOf(2, 4, 0)
    println(list.joinToString3("/"))
}
```
除了扩展函数，还可以扩展属性，例如我想实现String和StringBuilder通过属性去直接获得最后字符：
```kotlin
/*
* 扩展属性 lastChar获取String的最后一个字符
* */
val String.lastChar: Char
    get() = get(length - 1)
/*
* 扩展属性 lastChar获取StringBuilder的最后一个字符
* */
var StringBuilder.lastChar: Char
    get() = get(length - 1)
    set(value: Char) {
        setCharAt(length - 1, value)
    }
/*
* 测试
* */
fun testExtension(){
    val s = "abc"
    println(s.lastChar)
    val sb = StringBuilder("abc")
    println(sb.lastChar)
}
```
定义好扩展属性后，之后只需import完了就跟使用自己的属性一样方便了。

#### Why？Kotlin为什么能实现扩展函数和属性这样的特性？
在Kotlin中要理解一些语法，只要认识到**Kotlin语言最后需要编译为class字节码，Java也是编译为class执行，也就是可以大致理解为Kotlin需要转成Java一样的语法结构**，
Kotlin就是一种**强大的语法糖**而已，Java不具备的功能Kotlin也不能越界的。
- 那Kotlin的扩展函数怎么实现的呢？介绍一种万能的办法去理解Kotlin的语法：**将Kotlin代码转化成Java语言**去理解，步骤如下：
    - 在Android Studio中选择Tools ---> Kotlin ---> Show Kotlin Bytecode 这样就把Kotlin转化为class字节码了
    - class码阅读不太友好，点击左上角的Decompile就转化为Java
- 再介绍一个小窍门，在前期对Kotlin语法不熟悉的时候，可以先用Java写好代码，再利用AndroidStudio工具**将Java代码转化为Kotlin代码**，步骤如下：
    - 在Android Studio中选中要转换的Java代码 ---> 选择Code ---> Convert Java File to Kotlin File

我们看看将上面的扩展函数转成Java后的代码
```java
/*
* 扩展函数会转化为一个静态的函数，同时这个静态函数的第一个参数就是该类的实例对象
* */
public static final char lastChar(@NotNull String $receiver) {
    Intrinsics.checkParameterIsNotNull($receiver, "$receiver");
    return $receiver.charAt($receiver.length() - 1);
}
/*
* 获取的扩展属性会转化为一个静态的get函数，同时这个静态函数的第一个参数就是该类的实例对象
* */
public static final char getLastChar(@NotNull StringBuilder $receiver) {
    Intrinsics.checkParameterIsNotNull($receiver, "$receiver");
    return $receiver.charAt($receiver.length() - 1);
}
/*
* 设置的扩展属性会转化为一个静态的set函数，同时这个静态函数的第一个参数就是该类的实例对象
* */
public static final void setLastChar(@NotNull StringBuilder $receiver, char value) {
    Intrinsics.checkParameterIsNotNull($receiver, "$receiver");
    $receiver.setCharAt($receiver.length() - 1, value);
}
```
查看上面的代码可知：对于扩展函数，转化为Java的时候其实就是一个静态的函数，同时这个静态函数的第一个参数就是该类的实例对象，这样把类的实例传人函数以后，函数内部就可以访问到类的公有方法。
对于扩展属性也类似，获取的扩展属性会转化为一个静态的get函数，同时这个静态函数的第一个参数就是该类的实例对象，设置的扩展属性会转化为一个静态的set函数，同时这个静态函数的第一个参数就是该类的实例对象。
函数内部可以访问公有的方法和属性。
- 从上面转换的源码其实可以看到**扩展函数和扩展属性适用的地方和缺陷**，有两点：
    - 扩展函数和扩展属性内**只能访问到类的公有方法和属性**，私有的和protected是访问不了的
    - 扩展函数**不能被override**，因为Java中它是静态的函数
- 下面再举几个扩展函数的例子，让大家感受一下扩展函数的方便：
```kotlin
/*
* show toast in activity
* */
fun Activity.toast(msg: String){
    Toast.makeText(this, msg, Toast.LENGTH_SHORT).show()
}

val Context.inputMethodManager: InputMethodManager?
    get() = getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager

/*
* hide soft input
* */
fun Context.hideSoftInput(view: View) {
    inputMethodManager?.hideSoftInputFromWindow(view.windowToken, 0)
}


/**
 * screen width in pixels
 */
val Context.screenWidth
    get() = resources.displayMetrics.widthPixels

/**
 * screen height in pixels
 */
val Context.screenHeight
    get() = resources.displayMetrics.heightPixels

/**
 * returns dip(dp) dimension value in pixels
 * @param value dp
 */
fun Context.dip2px(value: Int): Int = (value * resources.displayMetrics.density).toInt()
```

## Tip5-懒初始化by lazy 和 延迟初始化lateinit
#### 懒初始化by lazy
懒初始化是指推迟一个变量的初始化时机，变量在使用的时候才去实例化，这样会更加的高效。因为我们通常会遇到这样的情况，一个变量直到使用时才需要被初始化，或者仅仅是它的初始化依赖于某些无法立即获得的上下文。
```kotlin
/*
* 懒初始化api实例
* */
val purchasingApi: PurchasingApi by lazy {
    val retrofit: Retrofit = Retrofit.Builder()
            .baseUrl(API_URL)
            .addConverterFactory(MoshiConverterFactory.create())
            .build()
    retrofit.create(PurchasingApi::class.java)
}
```
像上面的代码，retrofit生成的api实例会在首次使用到的时候才去实例化。需要注意的是by lazy一般只能修饰val不变的对象，不能修饰var可变对象。
```kotlin
class User(var name: String, var age: Int)

/*
* 懒初始化by lazy
* */
val user1: User by lazy {
    User("jack", 15)
}
```

#### 延迟初始化lateinit
另外，对于var的变量，如果类型是非空的，是必须初始化的，不然编译不通过，这时候需要用到lateinit延迟初始化，使用的时候再去实例化。
```kotlin
/*
* 延迟初始化lateinit
* */
lateinit var user2: User

fun testLateInit() {
    user2 = User("Lily", 14)
}
```
#### by lazy 和 lateinit 的区别
- by lazy 修饰val的变量
- lateinit修饰var的变量，且变量是非空的类型

## Tip-不用再手写findViewById



## Tip-



## Tip-


## Tip-



### 参考文档
* 《Kotlin in Action》
* https://savvyapps.com/blog/kotlin-tips-android-development
