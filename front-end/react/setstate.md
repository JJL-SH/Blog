# 懒惰的'setState'

setState 不会立即改变数据

```
// name is ""
this.setState({
    name: "name"
})
console.log(`name is ${this.state.name}`)
```

这段代码没有按我的预料输出name.

所以如果需要获取,就需要在回调函数里去做.

```
this.setState({
    name: "name"
}, () => {
  console.log(`name is ${this.state.name}`)
})
```

这样才会如你所预料那般的输出.

对于上面的这个问题倒是还好.因为setState是放在一个队列里异步去处理的, 所以在没有成功改变之前,输出的就是之前的值. 在回调里显示正确的数据,那是因为callback的原因.

setState多次,re-render一次

这个是我始料未及的了,我一直以为每次setState都会造成一次re-render.其实并不是这样.

```
componentDidMount(){
  this.setState((prevState, props) => ({count: this.state.count + 1})) // 1
  this.setState((prevState, props) => ({count: this.state.count + 1})) // 2
  this.setState((prevState, props) => ({count: this.state.count + 1})) // 3
  this.setState({name: "xiaohesong"}) // 4
}

render(){
  console.log("render")
  return(<h1>SetState</h1>)
}
```

可以发现,这里只会出现render两次,而不是想象中的4+1(初始化的render). WTF! Why?

好吧,还是得寻找原因不是?

我们之前说他是塞进一个队列里去做异步的处理的,就是说他是把我们这4个setState操作放到了一个队列里,然后batch操作.啥啥啥?恩,还是来代码阐述下比较好.

```
  this.setState((prevState, props) => ({count: this.state.count + 1})) // 1
  this.setState((prevState, props) => ({count: this.state.count + 1})) // 2
  this.setState((prevState, props) => ({count: this.state.count + 1})) // 3
  this.setState({name: "xiaohesong"}) // 4
```

上面的这段代码里的这四个会被塞进队列里进行批量操作.批量操作?

```
{count: this.state.count + 1}, {count: this.state.count + 1}, ..., {name: "xiaohesong"})
```

如果把上面的代码换成异步的呢?

```
componentDidMount(){
    setTimeout(() => {
        this.setState(count: this.state.count + 1)
        this.setState(count: this.state.count + 1)
    })
}
```

可以发现,如果改成这样,也会触发re-render. 可是这是为啥setTimeout里的两次this.state.count会成功,而不是像上面一样,浅合并成一个呢? 这个还得继续探索.

这个是因为react在他自身的事件内去给你做批量处理，但是非react自身事件，无法进行批量处理，比如在一些异步事件中。

react17中会有unstable_batchedUpdates api去处理。

```
ReactDOM.unstable_batchedUpdates(() => {
  this.setState({a: 1}); // 不重新渲染
  this.setState({a: 2}); // 不重新渲染
});
```

当结束unstable_batchedUpdates之后，会进行渲染。

## 总结

- setState操作,默认情况下是每次调用, 都会re-render一次,除非你手动shouldComponentUpdate为false. react为了减少rerender的次数,会进行一个浅合并.将多次re-render减少到一次re-render.
- setState之后,无法立即获取到this.state的值,是因为在setState的时候,他只会把操作放到队列里.