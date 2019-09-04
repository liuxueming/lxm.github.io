## ES6
###let和const
* let 块级作用域
* const 声明一个只读的常量。一旦声明，常量的值不能修改.**只在声明所在的块级作用域内有效。**
* 顶层对象的属性：顶层对象，在浏览器环境指的是window对象，在 Node 指的是global对象。ES5 之中，顶层对象的属性与全局变量是等价的。ES6:**var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性**。

###变量的解析赋值
* 变量的解析赋值： 对象的解构与数组有一个重要的不同。**数组的元素是按次序排列**的，变量的取值由它的位置决定；而**对象的属性没有次序**，变量必须与属性同名，才能取到正确的值。
* 圆括号规则：只要有可能导致解构的歧义，就不得使用圆括号。
* 不能使用圆括号场景
	* 变量声明语句
	* 函数参数
	* 赋值语句的模式
* 可以使用圆括号（唯一一种情况）：**赋值语句的非模式部分，可以使用圆括号**<pre>[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确
* 用途
	1. 交换变量的值<pre>let x = 1;
let y = 2;
[x, y] = [y, x];</pre>

	2. 从函数返回多个值 	 <pre> // 返回一个数组
function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();
// 返回一个对象
function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
</pre>
	3. 函数参数的定义(解构赋值可以方便地将一组参数与变量名对应起来。)<pre>// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);
// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});</pre>
	4. 提取JSON数据<pre> let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};
let { id, status, data: number } = jsonData;
console.log(id, status, number);
// 42, "OK", [867, 5309]
</pre>
   5. 函数参数的默认值<pre>jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
} = {}) {
  // ... do stuff
};</pre>
	6. 遍历 Map 结构<pre>const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');
for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world</pre><pre>// 获取键名
for (let [key] of map) {
  // ...
}
// 获取键值
for (let [,value] of map) {
  // ...
}</pre>
	7. 输入模块的指定方法(加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰)<pre>const { SourceMapConsumer, SourceNode } = require("source-map");
</pre>

### 数值的扩展
* **Math**
	* Math.trunc()
		* :方法用于去除一个数的小数部分，返回整数部分<pre>Math.trunc(4.1) // 4
Math.trunc(4.9) // 4
Math.trunc(-4.1) // -4
Math.trunc(-4.9) // -4
Math.trunc(-0.1234) // -0
</pre>
	 * 对于非数值，Math.trunc内部使用Number方法将其先转为数值。<pre> Math.trunc('123.456') // 123
Math.trunc(true) //1
Math.trunc(false) // 0
Math.trunc(null) // 0
</pre>
	 * 对于空值和无法截取整数的值，返回NaN。<pre>Math.trunc(NaN);      // NaN
Math.trunc('foo');    // NaN
Math.trunc();         // NaN
Math.trunc(undefined) // NaN</pre>
	* Math.sign()
		* 参数为正数，返回+1；
		* 参数为负数，返回-1；
		* 参数为 0，返回0；
		* 参数为-0，返回-0;
		* 其他值，返回NaN。 





			   
