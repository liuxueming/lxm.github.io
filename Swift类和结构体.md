## Swift类和结构体
### 概念
* 基本概念：类和结构体是人们构建代码所用的一种通用且灵活的构造体。
* 与其他编程语言所不同的是，**Swift 并不要求你为自定义类和结构去创建独立的接口和实现文件。** 你所要做的是在一个单一文件中定义一个类或者结构体，**系统将会自动生成面向其它代码的外部接口**。
* 对象：通常一个**类的实例**被称为对象。

### 比较
##### 相同点
* 定义属性用于存储值
* 定义方法用于提供功能
* 定义[下标操作](https://github.com/liuxueming/lxm..github.io/blob/d57151d077e0c92d0bc33f6ae448f44ea8215519/Swift%20%E4%B8%8B%E6%A0%87%E8%AF%AD%E6%B3%95.md)使得可以通过下标语法来访问实例所包含的值
* 定义构造器用于生成初始化值
* 通过扩展以增加默认实现的功能
* 实现协议以提供某种标准功能

##### 不同点(类的附加功能)
* [继承](http://www.swift51.com/swift4.0/chapter2/13_Inheritance.html) 允许一个类继承另一个类的特征
* [类型转换](http://www.swift51.com/swift4.0/chapter2/19_Type_Casting.html) 允许在运行时检查和解释一个类实例的类型
*  [析构器](http://www.swift51.com/swift4.0/chapter2/15_Deinitialization.html) 允许一个类实例释放任何其所被分配的资源
* [引用计数](http://www.swift51.com/swift4.0/chapter2/16_Automatic_Reference_Counting.html) 允许对一个类的多次引用

### 定义语法
* 语法：通过关键字**class**和**struct**来分别表示类和结构体。<pre> class SomeClass {
    // 在这里定义类
}
struct SomeStructure {
    // 在这里定义结构体
}</pre>
* 注意：请使用**UpperCamelCase**这种方式来命名（如SomeClass和SomeStructure等。相反的，请使用**lowerCamelCase**这种方式为属性和方法命名（如framerate和incrementCount），以便和类型名区分

* <pre>struct Resolution {
      var width = 0
      var height = 0
}
class VideoMode {
	  var resolution = Resolution()
      var interlaced = false
      var frameRate = 0.0
      var name: String?
}</pre>

### 实例
* 语法：结构体和类都使用构造器语法来生成新的实例。最简单形式是在结构体或者类的类型名称后跟随一对**空括号**。通过这种方式所创建的类或者结构体实例，其属性均会被初始化为默认值。<pre>let someResolution = Resolution()
let someVideoMode = VideoMode()</pre>

### 属性访问
* 语法:**点语法**，你可以访问实例的属性。其语法规则是，实例名后面紧跟属性名，两者通过点号(.)连接,也可以访问子属性。<pre>print("The width of someVideoMode is \(someVideoMode.resolution.width)")
// 打印 "The width of someVideoMode is 0"</pre>
* 注意:与 Objective-C 语言不同的是，Swift 允许直接设置结构体属性的子属性。<pre>someVideoMode.resolution.width = 1280
print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// 打印 "The width of someVideoMode is now 1280"</pre>

### 结构体类型的成员逐一构造器
* 所有结**构体都有一个自动生成的成员逐一构造器**，用于初始化新结构体实例中成员的属性。新实例中各个属性的初始值可以通过属性的名称传递到成员逐一构造器之中：
<pre>let vga = Resolution(width:640, height: 480)
</pre>
* 注意:类实例没有默认的成员逐一构造器

### 类和结构体的类型
#####  结构体
 * 结构体和枚举是**值类型**：值类型被赋予给一个变量、常量或者被传递给一个函数的时候，其值会被拷贝。
 * 在 Swift 中，所有的基本类型：整数（Integer）、浮点数（floating-point）、布尔值（Boolean）、字符串（string)、数组（array）和字典（dictionary），都是值类型，并且在底层都是以结构体的形式所实现。
 * Objective-c里的NSString、NSDictionary、NSArray都是引用类型)
 
#####  类

 * 类是**引用类型**：引用类型在被赋予到一个变量、常量或者被传递到一个函数时，其值不会被拷贝。因此，引用的是已存在的实例本身而不是其拷贝。
 *  等价于（**===**）：表示两个类类型（class type）的常量或者变量引用同一个类实例。
 *  不等价于（**!==**）：不是引用同一个实例
 *  等于（**==**）：表示两个实例的**值“相等”或“相同”**，判定时要遵照设计者定义的评判标准，因此相对于“相等”来说，这是一种更加合适的叫法。[等价运算符](http://www.swift51.com/swift4.0/chapter2/25_Advanced_Operators.html#equivalence_operators) 

### 类和结构体的选择
##### 按照通用的准则，当符合一条或多条以下条件时，请考虑构建结构体
1.  该数据结构的主要目的是用来**封装少量相关简单数据值**。
2. 有理由预计该数据结构的实例在被赋值或传递时，封装的数据将会被拷贝而不是被引用。
3. 该数据结构中储存的值类型属性，也应该被拷贝，而不是被引用。
4. 该数据结构不需要去继承另一个既有类型的属性或者行为

* 比如：
 1. 几何形状的大小，封装一个width属性和height属性，两者均为Double类型。
 2. 一定范围内的路径，封装一个start属性和length属性，两者均为Int类型。
 3. 三维坐标系内一点，封装x，y和z属性，三者均为Double类型。

##### 其他场景，请考虑使用类。
