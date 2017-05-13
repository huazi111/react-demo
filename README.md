
## 技术栈：

react + react-router + redux + immutable + less + ES6/7 + webpack + fetch

###redux

	1.store 是保存数据的地方，整个应用只有一个Store,createStore函数用来生成store
	
	2.store 对象包含所有数据，如果想得到某时点的数据，就要对Store生成快照，这种时点数据集合叫做State。 store.getState()得到

	3.Action 用来接收state的变化，它是一个对象，type属性作为他的标识，改变State,就需要使用Action

	4.store.dispatch 发出Action 
		```javascript
		store.dispatch({
		  type: 'ADD_TODO',
		  payload: 'Learn Redux'
		});
		```
	5.store 收到Action后，必须计算state 需要变化，这个过程就交到Reducer
		```javascript
			const reducer = function (state, action) {
			  // ...
			  return new_state;
			};
		```
	6.当第一次渲染页面时，store里的初始state是怎么获得的呢？
	代码一开始一般就是 
	createStore(reducers,defaultParams)的调用，其中reducers可以使一个reducer，也可是redux.combineReducers过的reducer的集合。
	createStore方法会对每个reducer去dispatch一个action.type=@@redux/INIT类型的action，而这个action一般在reducer的代码里不会被handle，直接掉入default块，于是就返回了state的初始状态。
	然后一般就会ReactDom.render(将应用渲染出来，每个子组件的容器组件通过传入this.context.store.getState()方法获得的state对象, 以及容器组件上自带的ownProps给mapStatesToProperties方法，来构建props，最后将props应用到子组件的UI组件上。
	当在子组件上发生交互行为，如click时，mapDispatchToProps会定义click触发时应该dispatch哪一个action的映射。
	然后store接收到这个action后会进行reduce，得到最新的state，然后再调用所有的子组件的mapStatesToProps方法生成新的props。
	最后对Provider进行重新渲染，当然上面的事件计算出来的很多state可能都不会发生变化，所以diff算法不会去修改这些没有发生变化的组件，因此性能也比较好。