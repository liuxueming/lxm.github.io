## Swift 下标语法
#### 概念
* 基本概念：下标可以定义在**类、结构体和枚举**中，是访问集合，列表或序列中元素的快捷方式。举例来说，用下标访问一个Array实例中的元素可以写作someArray[index]，访问Dictionary实例中的元素可以写作someDictionary[key]。
* 注意：一个类型可以定义多个下标，通过不同索引类型进行重载。**下标不限于一维，你可以定义具有多个入参的下标满足自定义类型的需求。**

#### 下标语法
* 基本语法：与定义实例方法类似，定义下标使用**subscript**关键字，指定一个或多个输入参数和返回类型；与实例方法不同的是，下标可以设定为读写或只读。
<pre>subscript(index: Int) -> Int {
    get {
      // 返回一个适当的 Int 类型的值
    }
    set(newValue) {
      // 执行适当的赋值操作
    }
}</pre>
* 注意：newValue的类型和下标的返回类型相同。如同计算型属性，可以不指定 setter 的参数（newValue）。如果不指定参数，setter 会提供一个名为newValue的默认参数。
<pre>struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}
let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[7])")
// 打印 "six times three is 21"</pre>

#### 下标选项
* 概念：下标可以接受**任意类型、任意数量**的入参。下标的**返回值也可以是任意类型**。下标可以使用变量参数和可变参数，但**不能使用输入输出参数，也不能给参数设置默认值**

* 下标的重载： 一个类或结构体可以根据自身需要提供**多个下标实现**，使用下标时将通过入参的数量和类型进行区分，自动匹配合适的下标。

<pre>struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(count: rows * columns, repeatedValue: 0.0)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    //重载： 断言在下标越界时触发
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}</pre>