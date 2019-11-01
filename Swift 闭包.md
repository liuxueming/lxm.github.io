## Swift 闭包
### 概念
* **是自包含的函数代码块，可以在代码中被传递和使用**。Swift 中的闭包与 C 和 Objective-C 中的代码块（blocks）以及其他一些编程语言中的匿名函数比较相似。

* 相对于Objective-C 有如下优化点
	* 利用上下文推断参数和返回值类型
	* 隐式返回单表达式闭包，即单表达式闭包可以**省略 return** 关键字 
	* 参数名称缩写
	* 尾随闭包语法
	
### 闭包表达式
* 语法
<pre>{ (parameters) -> returnType in
    	statements
}</pre>
* 注意点：
 * 可以是 in-out 参数（Swift参数默认不能修改，但是使用关键字**inout**修饰之后，就可以修改，这样的参数，叫做in-out参数），但不能设定默认值 
 * 也可以使用具名的可变参数（用...修饰）（注：但是如果可变参数不放在参数列表的最后一位的话，调用闭包的时时编译器将报错。）
 * <pre>func add(a:Int, b:Int ,others:Int ...) -> Int {
var result = a + b
for num in others {
    result += num
}
    return result
}
let number = add(2, b: 5, others: 2, 50, 4)
print(number)</pre>
	* 元组也可以作为参数和返回值
	* Swift 自动为内联闭包提供了参数名称缩写功能，你可以直接通过 **$0，$1，$2** 来顺序调用闭包的参数，以此类推。<pre>reversedNames = names.sorted(by: >)
</pre>

### 尾随闭包
* 概念：尾随闭包是一个书写在函数括号之后的闭包表达式，函数支持将其作为最后一个参数调用。在使用尾随闭包时，你不用写出它的参数标签。
* <pre>func someFunctionThatTakesAClosure(closure: () -> Void) {
    // 函数体部分
}
// 以下是不使用尾随闭包进行函数调用
someFunctionThatTakesAClosure(closure: {
    // 闭包主体部分
})
// 以下是使用尾随闭包进行函数调用
someFunctionThatTakesAClosure() {
    // 闭包主体部分
}</pre>
* 注意：如果闭包表达式是**函数或方法的唯一参数**，则当你使用尾随闭包时，你甚至可以把 () 省略掉：<pre>reversedNames = names.sorted { $0 > $1 }
</pre>

### 值捕获
* 概念：闭包可以在其被定义的**上下文中捕获常量或变量**，即使定义这些常量和变量的原作用域已经不存在，闭包仍然可以在闭包函数体内引用和修改这些值。
* 原因:函数和闭包都是**引用类型**。无论你将函数或闭包赋值给一个常量还是变量，你实际上都是将常量或变量的值设置为对应函数或闭包的引用。

### 逃逸闭包
* 概念：当一个闭包作为参数传到一个函数中，但是**这个闭包在函数返回之后才被执行**，我们称该闭包从函数中逃逸。你可以在参数名之前标注 **@escaping**，用来指明这个闭包是允许“逃逸”出这个函数的。
**逃逸闭包就是把这个传入的block可以保存在外部.等待某一时刻你可以再次调用这个block.这一个时刻是在函数返回之后.**

<pre>//一种能使闭包“逃逸”出函数的方法是，将这个闭包保存在一个函数外部定义的变量中。
//这里是一个闭包数组
typealias completionHandlerBlock = () -> Void
var completionHandlers: [completionHandlerBlock] = []
class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWith_Escaping_Closure { self.x = 100 }  //将一个闭包标记为 @escaping 意味着你必须在闭包中显式地引用 self。
        someFunctionWith_Non_Escaping_Closure { x = 200 }   //不带self
    }
    //逃逸闭包
    func someFunctionWith_Escaping_Closure(completionHandler: @escaping completionHandlerBlock) {
        //函数接受一个闭包作为参数，该闭包被添加到一个函数外定义的数组中。如果你不将这个参数标记为 @escaping，就会得到一个编译错误。
        completionHandlers.append(completionHandler)
    }
    
    //非逃逸闭包
    func someFunctionWith_Non_Escaping_Closure(closure: () -> Void) {
        //该闭包是一个非逃逸闭包，这意味着它可以隐式引用 self。同时无法保存在外部
        completionHandlers.append(closure)  //报错 : Passing non-escaping parameter 'closure' to function expecting an @escaping closure
        closure()
    }
}
let instance = SomeClass()
instance.doSomething()
print(instance.x)
// 打印出 "200"

completionHandlers.first?()
print(instance.x)
// 打印出 "100"</pre>

### 自动闭包
* 概念：autoclosure 做的事情就是**把⼀句表达式⾃动地封装成⼀个闭包 **(closure)，使用 **@autoclosure**来进行修饰。
<pre>
func logIfTrue(_ predicate:@autoclosure () -> Bool) {
        if predicate() {
            print("true")
        }
    }
    //调用的时候可以直接写:
    logIfTrue(2>1)</pre>
