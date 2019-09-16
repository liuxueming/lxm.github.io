## Redux 笔记
### 设计思想
* Web 应用是一个状态机，视图与状态是一一对应的。
* 所有的状态，保存在一个对象里面。

### 三原则：
* **单一数据源**：整个应用的State被存储在一个状态树中，且只存在于唯一的Store中。
* **State是只读的**
* **应用状态的改变通过纯函数（reducer）来完成**

### 适用场景
 场景：**多交互、多数据源。**
 
* 某个组件的状态，需要共享
* 某个状态需要在任何地方都可以拿到
* 一个组件需要改变全局状态
* 一个组件需要改变另一个组件的状态

###基本概念和API
1. **Store**</br>保存数据的地方，整个应用只能有一个Store。<pre>import { createStore } from 'redux';
const store = createStore(fn);</pre>
2. **State**</br>Store对象包含所有数据。**如果想得到某个时点的数据，就要对 Store 生成快照。这种时点的数据集合，就叫做 State。**<pre>import { createStore } from 'redux';
const store = createStore(fn);
const state = store.getState();</pre>
Redux 规定， 一个 State 对应一个 View。只要 State 相同，View 就相同。你知道 State，就知道 View 是什么样，反之亦然。
3. **Action**</br>State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。所以，State 的变化必须是 View 导致的。**Action 就是 View 发出的通知，表示 State 应该要发生变化了**</br>Action 是一个对象。其**中的type属性是必须的**，表示 Action 的名称。其他属性可以自由设置[参考链接](https://github.com/redux-utilities/flux-standard-action)<pre>const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};</pre>
4. **Action Creator**</br>定义一个函数来生成Action,这个函数就叫做Action Creator。<pre>const ADD_TODO = '添加 TODO';
function addTodo(text) {
	return {
   type: ADD_TODO,
   text
  }
}
const action = addTodo('Learn Redux');</pre>
5. **store.dispatch()**</br>`store.dispatch()`是 View 发出 Action 的唯一方法。<pre>import { createStore } from 'redux';
const store = createStore(fn);
store.dispatch({
  type: 'ADD_TODO',
  payload: 'Learn Redux'
});</pre>它接受一个Action对象作为参数，将它发送出去。<pre>store.dispatch(addTodo('Learn Redux'));
</pre>
6. **Reducer**</br>Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。**Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State**<pre>const defaultState = 0;
const reducer = (state = defaultState, action) => {
  switch (action.type) {
    case 'ADD':
      return state + action.payload;
    default: 
      return state;
  }
};
const state = reducer(1, {
  type: 'ADD',
  payload: 2
});</pre>
实际应用中，Reducer 函数不用像上面这样手动调用，store.dispatch方法会触发 Reducer 的自动执行。为此，Store 需要知道 Reducer 函数，做法就是在生成 Store 的时候，将 Reducer 传入createStore方法。<pre>import { createStore } from 'redux';
const store = createStore(reducer);</pre>
7. **纯函数**</br>只要是同样的输入，必定得到同样的输出。
8. **store.subscribe()**</br>Store 允许使用`store.subscribe`方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。<pre>import { createStore } from 'redux';
const store = createStore(reducer);
store.subscribe(listener);
</pre>显然，只要把 View 的更新函数（对于 React 项目，就是组件的render方法或setState方法）放入listen，就会实现 View 的自动渲染。</br>**解除监听** store.subscribe方法返回一个函数，调用这个函数就可以解除监听。<pre>let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);
unsubscribe();</pre>
![流程图](https://img.alicdn.com/tps/TB1kYfaNVXXXXcLaXXXXXXXXXXX-604-352.png)

###Reducer 的拆分
不同的函数负责处理不同属性，最终把它们合并成一个大的 Reducer 即可。Redux 提供了一个`combineReducers`方法，用于 Reducer 的拆分。你只要定义各个子 Reducer 函数，然后用这个方法，将它们合成一个大的 Reducer。总之，`combineReducers()`做的就是产生一个整体的 Reducer 函数。**该函数根据 State 的 key 去执行相应的子 Reducer**，并将返回结果合并成一个大的 State 对象。

### 中间件
概念：**中间件就是一个函数，对store.dispatch方法进行了改造，在发出 Action 和执行 Reducer 这两步之间，添加了其他功能**



参考链接：
</br>[图解Redux数据流](https://alisec-ued.github.io/2016/11/23/%E5%9B%BE%E8%A7%A3Redux%E6%95%B0%E6%8D%AE%E6%B5%81(%E4%B8%80)/)</br>[Redux 入门教程（一）：基本用法
](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)</br>[Redux 入门教程（二）：中间件与异步操作
](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html)</br>[redux office](https://redux.js.org/basics/basic-tutorial)

