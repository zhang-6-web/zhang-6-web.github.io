+++
date = '2025-09-08T13:17:13+08:00'
draft = false
title = 'react类组件和函数组件的渲染机制以及钩子函数'

+++

React的渲染机制基于虚拟dom的概念。虚拟dom是真实dom的js对象的表示，React通过比较虚拟DOM的变化来高效的真实更新DOM,从而减少不必要的DOM操作，用于提高性能。

类组件的生命周期：

1. 构造函数(constructor)：初始化组件的状态和绑定事件处理函数（组件实例化时调用一次）

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
}
```

2. 组件挂载(componentDidMount):组件挂载后调用一次：

```jsx
componentDidMount() {
  fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => this.setState({ data }));
}
```

3. 组件更新:

+ shouldComponentUpdate:决定组件是否需要重新渲染，可以避免不必要的渲染；组件接受到新的props和state时调用。

```jsx
shouldComponentUpdate(nextProps, nextState) {
  return nextState.count !== this.state.count;
}
```

+ getSnapshotBeforeUpdate:在组件更新之前捕获DOM的状态，例如滚动位置等。返回一个值，该值将作为componentDidUpdate的第三个参数。

```jsx
getSnapshotBeforeUpdate(prevProps, prevState) {
  if (prevProps.data !== this.props.data) {
    return this.someRef.current.scrollTop;
  }
  return null;
}
```

+ componentDidUpdate:组件更新完成后调用（主要用于更新DOM）

```jsx
componentDidUpdate(prevProps, prevState, snapshot) {
  if (snapshot !== null) {
    this.someRef.current.scrollTop = snapshot;
  }
}
```

4. 组件卸载(componentWillUnmount)：组件卸载时调用（清理资源、取消订阅、清理定时器等）

```jsx
componentWillUnmount() {
  clearInterval(this.timer);
}
```

函数组件生命周期：

1. useState:
2. useEffect:
3. useLayoutEffect:会在所有DOM变更之后同步调用，可以用于读取DOM布局并同步重新渲染
4. useMemo:用于缓存计算值，避免不必要的计算。
5. useCallback:用于缓存函数，避免不必要的函数重新创建
6. useRef:创建一个可变的引用，其值在组件的整个生命周期内保持不变
7. useImperativeHandle:用于自定义暴露给父组件的实例值
8. useReducer:
9. useContext:
