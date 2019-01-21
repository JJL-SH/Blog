# react16生命周期方法

## 所有生命周期方法：

- constructor
- getDerivedStateFromProps *new*
- shouldComponentUpdate
- render
- getSnapshotBeforeUpdate *new*
- componentDidUpdate
- componentDidMount
- componentWillUnmount
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