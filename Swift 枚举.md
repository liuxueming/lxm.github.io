### Swift 枚举
* 概念：枚举为一组相关的值定义了一个共同的类型，使你可以在你的代码中以类型安全的方式来使用这些值。
* 语法：<pre>enum SomeEnumeration {
    // 枚举定义放在这里
	}
* 类型：
	* 关联值：
		* 概念：关联值是将额外信息附加到enum case中的一种方式。如:<pre>enum Barcode { 
    	case upc(Int, Int, Int, Int)
    case qrCode(String)
}
		* 注意：**关联类型的常量和变量可以存储case选项的中的一个（连同它们的关联值），但是同时只能存储两个值中的一个值**。例如: <pre>var productBarcode = Barcode.upc(8, 85909, 51226, 3)</pre><pre>productBarcode = .qrCode("ABCDEFGHIJKLMNOP")</pre>
这时，原始的 Barcode.upc 和其整数关联值被新的 Barcode.qrCode 和其字符串关联值所替代。类型的常量和变量可以存储一个 .upc 或者一个 .qrCode（连同它们的关联值），但是在同一时间只能存储这两个值中的一个。
	* 原始值
		* 概念：作为关联值的替代选择，枚举成员可以被默认值（称为原始值）预填充，这些原始值的**类型必须相同。**	
		* 注意：原始值可以是**字符串**、**字符**，或者**任意整型值**或**浮点型值**。每个原始值在枚举声明中必须是**唯一**的
		* 原始值的隐式赋值：在使用原始值为**整数**或者**字符串类型**的枚举时，不需要显式地为每一个枚举成员设置原始值，Swift 将会自动为你赋值。
			1. 当使用整数作为原始值时，隐式赋值的值依次递增 1。如果第一个枚举成员没有设置原始值，其原始值将为 0。	
			2. 当使用字符串作为枚举类型的原始值时，每个枚举成员的隐式原始值为该枚举成员的名称
			3. 使用枚举成员的 rawValue 属性可以访问该枚举成员的原始值.例如：<pre> let earthsOrder = Planet.earth.rawValue
// earthsOrder 值为 3
let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection 值为 "west"	</pre>
		* 使用原始值初始化枚举实例
			* 概念：如果在定义枚举类型的时候使用了原始值，那么将会自动获得一个初始化方法，这个方法接收一个叫做 rawValue 的参数，参数类型即为原始值类型，返回值则是枚举成员或 nil。你可以使用这个初始化方法来创建一个新的枚举实例.例如<pre>enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}<br>let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet 类型为 Planet? 值为 Planet.uranus：</pre>
	* 递归枚举
		* 概念：递归枚举是一种枚举类型，它有一个或多个枚举成员使用该枚举类型的实例作为关联值。使用递归枚举时，编译器会插入一个间接层。你可以在枚举成员前加上 **indirect** 来表示该成员可递归。 
		* 例如计算表达式  (5 + 4) * 2 ：<pre>indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}</br>let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))<br>func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    case let .number(value):
        return value
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
    }
}
print(evaluate(product))
// 打印“18”</pre>

* 参考链接：<br>**Swift 中枚举高级用法及实践:**<https://swift.gg/2015/11/20/advanced-practical-enum-examples/><br>**官方文档:**<https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html>

	 