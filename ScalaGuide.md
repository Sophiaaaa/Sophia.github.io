# 1 基本语法
## 1.1 函数式编程体验 Spark-Shell 之 WordCount
- 数据准备
![数据准备](./picture/wordCount.png)
- wordCount
![wordCount](./picture/wordCount2.png)
- 排序
![排序](./picture/wordCount3.png)
## 1.2 Scala 交互模式(cmd 窗口介绍)
## 1.3 数据类型
- Scala 和 Java 一样，有 7 种数值类型 Byte、Char、Short、Int、Long、Float 和 Double（无包 装类型）和 Boolean、Unit 类型.
- 注意: Unit表示无值，和其他语言中void 等同。用作不返回任何结果的方法的结果类型。Unit 只有一个实例值，写成()。
## 1.4 变量的定义
```scala
object 变量定义 extends App { 
    /** 
      * 定义变量使用var或者val关 键 字 
      *
      * 语法: 
      *  var | val 变量名称(: 数据类型) =变量值
      */
    // 使用val修饰的变量, 值不能为修改,相当于java中final修饰的变量 
    val name = "tom"
 
    // 使用var修饰的变量,值可以修改 
    var age = 18
 
    // 定义变量时,可以指定数据类型,也可以不指定,不指定时编译器会自动推测变量的数据类型 
    val name2 : String = "jack"
}
```
