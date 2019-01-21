# react16生命周期方法

## 所有生命周期方法：

- constructor
- static getDerivedStateFromProps *new*
- shouldComponentUpdate
- render
- getSnapshotBeforeUpdate *new*
- componentDidUpdate
- componentDidMount
- componentWillUnmount
- static getDerivedStateFromError *new*
- componentDidCatch *new*

## constructor(props)

constructor 一般用来初始化 state，声明 ref，绑定方法。

```
class MyComponent extends Component{
  constructor(props){
    super(props)
    this.state = {
      counter: 0
    }
    this.ref = React.createRef()
    this.handleClick = this.handleClick.bind(this)
  }
}
```

## static getDerivedStateFromProps(props, state)

组件每次被 render 的时候，包括在组件构建之后（虚拟 DOM 之后，实际 DOM 挂载之前），每次获取新的 props 或 state 之后；每次接收新的 props 之后都会返回一个对象作为新的 state，返回 null 则说明不需要更新 state；配合 componentDidUpdate，可以覆盖 componentWillReceiveProps 的所有用法

在 getDerivedStateFromProps 中还禁止了访问类上的属性（this...），强制让开发者去比较 props 与 state 中的值，以确保当开发者用到 getDerivedStateFromProps 这个生命周期函数时，就是在根据当前的 props 来更新组件的 state，而不是去做其他一些让组件自身状态变得更加不可预测的事情。也不能在这个方法里调用当前实例的其他方法，比如 this.fetchData，想调用的话得在 componentDidUpdate 里调用。

```
// 以前的用法
componentWillReceiveProps(nextProps) {
  if (nextProps.translateX !== this.props.translareX) {
    this.setState({
      translateX: nextProps.translateX  
    })
  }
}

// 现在的用法
static getDerivedStateFromProps(props, state) {
  if (props.translateX !== state.translateX) {
    return {
      translateX: props.translateX
    }
  }
  return null;
}
```

## render

render 主要作用就是返回一段 JSX，表示想要渲染的内容。如果返回值是 null 或 false 则不会渲染任何内容。

```
render() {
  return (
    <div>
      <h1>title</h1>
      <p>description</p>
    </div>    
  )
}
```

## getSnapshotBeforeUpdate(prevProps, prevState)

这个方法会在组件更新时，render 方法之后，但是 DOM 还没有真正发生更新前执行。我们可以根据更新前的 props 和 state 返回一个值，这个值将会作为下一个生命周期方法 componentDidUpdate 的第三个参数传入。可以用来与 componentDidUpdate 协作完成一些事情。这里就可以不在 state 上面存数据直接把变量扔给 componentDidUpdate 使用

```
getSnapshotBeforeUpdate(prevProps, prevState) {
  if (prevState.blocks.length < this.state.blocks.length) {
    const grid = this.grid.current;
    const isAtBottomOfGrid = window.innerHeight + window.pageYOffset === grid.scrollHeight;

    return { isAtBottomOfGrid };
  }
  return null;
}
```

## componentDidUpdate(prevProps, prevState, snapshot)

该生命周期方法会在组件渲染更新完成后执行，一般在这里做的事情是：根据props的改变做一些操作比如请求数据，根据snapshot的值做一些操作，或者是做一些dom操作等等。

1、根据 props、state 的改变做一些处理

在旧的生命周期方法componentWillReceiveProps中，经常会比较this.props和nextProps，来进行其他操作比如请求数据，或调用this.someFunc等，在新的生命周期方法中这部分操作可以在componentDidUpdate中完成。

```
componentDidUpdate(prevProps, prevState, snapshot) {
  if (this.props.count !== prevProps.count) {
    this.fetchData();
  }
}
```

2、根据第三个参数snapshot做一些处理

比如以下例子就是根据上面 getSnapshotBeforeUpdate 里返回的 isAtBottomOfGrid 来判断当前页面的滚动条是不是在底部，如果是的话，更新后还需要手动将滚动条滚到底部。

```
componentDidUpdate(prevProps, prevState, snapshot) {
  if (snapshot.isAtBottomOfGrid) {
    window.scrollTo({
      top: this.grid.current.scrollHeight,
      behavior: 'smooth',
    });
  }
}
```

## componentDidMount

一般在 componentDidMount 里处理组件装载好之后才可以进行的操作，比如绑定事件、发送数据请求、或者进行一些dom相关的操作。

```
componentDidMount () => {
  window.addEventListener('scroll', this.handleScroll)
  this.timer = setTimeout(() => {
    console.log(new Date())
  }, 500)
}
```

## componentWillUnmount

组件在卸载之前执行，这个方法与 componentDidMount 是相对应的，在 componentDidMount 中绑定的事件、创建的定时器都可以在这个方法里清除。如果不清除这些事件、定时器的话这些任务会常驻在内存中不会自动销毁，知道手动刷新页面

```
componentWillUnmount() {
  window.removeEventLisrener('scroll', this.handleScroll)
  clearTimeout(this.timer)
}
```

## static getDerivedStateFromError(error)

这个方法用来获取子组件抛出的错误，根据错误信息返回一个对象，为新的state的值。只能获取到子组件生命周期方法中抛出的错误，像this.handleClick里抛出的错误，不会触发该方法。这个方法只能用来返回错误，如果想打印错误或做其他处理，需要在componentDidCatch里写。

```
static getDerivedStateFromError(error) {
  return { hasError: true };
}
```

## componentDidCatch(error, info)

当子组件中抛出错误后，componentDidCatch就会触发，可以在这个方法里捕获错误、打印错误信息或上报错误信息等操作。

```
componentDidCatch(error, info) {
  console.log(error, info);
}
```