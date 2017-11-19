---
title: react学习笔记之入门
date: 2017-11-05 20:22:10
tags: [React]
categories:
 - 学习笔记
---
## 前言
之前居然一直没有学习过react，还是趁着周末学习一下这个流行的框架。虽然fb随时又可能不给我们用了？！= =但是学习一下这个框架的思想也是好的。
React是2014年f8大会上被提出的，能用来解决一个大型应用的数据变更的问题。它不是完整的MVC、MVVM框架，只负责view层。它实现的原理是虚拟DOM的机制，数据单向绑定，这使它能快速响应复杂的数据变更和交互。它推崇组件化开发，这也满足了复杂场景下的高性能要求。
<!-- more -->
## 虚拟DOM与JSX
这是[官网](https://reactjs.org/)上的一个例子
``` jsx
ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('root')
)
```
`ReactDOM.render`用于将模版转为HTML语言，并插入指定的DOM节点。上面的`<h1>`实质上不是真的HTML标签，而是一个虚拟DOM。上面的代码就是把渲染结果`<h1>`标签插入到id为root的节点中。
这种HTML与Javascrpt混写的语法叫JSX。它实质上是一个语法糖，每一个XML标签都会被JSX转换工具转换成纯Javascript代码。上面的代码不使用JSX可以这样写：
``` js
// 不使用JSX
ReactDOM.render(
    React.createElement('h1', null, 'Hello, world!'),
    document.getElementById('root')
)
```
我们可以看到，使用JSX，组件的结构和组件之间的关系看上去更加清晰。
它的的基本语法规则是遇到HTML标签（以`<`开头），就用HTML规则解析；遇到代码块（以`{`开头），就用JavaScript规则解析。
## 组件
接下来我们来尝试封装组件
``` jsx
class App extends Component {
    render() {
        return <h1 className="title">Hello {this.props.name}</h1>
    }
}
```
``` jsx
ReactDOM.render(
    <App name="semine" />,
    document.getElementById('root')
)
```
``` css
.title {
    color: #ff0000;
    font-size: 44px;
}
```
`React.createClass`方法用于生成一个组件类。这里用了ES6的`class`关键字更为清晰。`App`就是一个组件类，模版插入`<App />`时，会自动生成`App`的一个实例。所有的组件类都必须有自己的`render`方法，用于输出组件。
样式方面，也可以用内嵌样式的写法。
``` jsx
class App extends Component {
    render() {
        const styleObj = {
            color: '#ff0000',
            fontSize: '44px'
        }
        return <h1 style={styleObj}>Hello {this.props.name}</h1>
    }
}
```
## 组件的生命周期
 - Mounted 被render解析生成对应的DOM节点，并被插入浏览器的DOM结构的一个过程
 - Update 当组件的state（状态）改变，一个mounted的组件被重新render的过程
 - Unmounted 一个mounted的组件对应的DOM节点在DOM结构中被移除的过程

每一个状态React都封装了对应的hook（钩子）函数
 - mounting：getInitialState(ES6不支持这个方法，改为在构造函数中声明)->componentWillMount->render->componentDidMount

``` jsx
class App extends Component {
    constructor(props) {
        alert('init')
        super(props)
        this.state = {
            color: 'blue'
        }
    }
    componentWillMount() {
        alert('will')
    }
    render() {
        return (
            <div className="App">
                <div style={this.state}>state</div>
            </div>
        );
    }
    componentDidMount() {
        alert('did')

        window.setTimeout(() => {
            this.setState({
                color: 'red'
            })
        }, 1000)
    }
}
```
这里的state可以这样理解：就是将组件看成是一个状态机，一开始有一个初始状态，然后用户互动，导致状态变化，从而触发重新渲染UI
 - Updating：componentWillReceiveProps(需要接受新的props时)->shouldComponentUpdate(对比新旧的props和state，判断是否需要update)->componentWillUpdate->render->componentDidUpdate
 - Unmounting：componentWillUnmount

## 监听事件
``` jsx
class TestButton extends Component {
    constructor(){
        super()
        this.clickHandler = this.clickHandler.bind(this)
    }
    // 监听点击事件
    clickHandler(e) {
        e.stopPropagation()
        e.preventDefault()

        const tip = this.refs.tip
        if (tip.style.display === 'none') {
            tip.style.display = 'inline'
        } else {
            tip.style.display = 'none'
        }
    }
    render() {
        return (
            <div>
                <button onClick={this.clickHandler}>显示|隐藏</button><span ref="tip">点击按钮</span>
            </div>
        )
    }
}

class Input extends Component {
    constructor(props) {
        super(props)
        this.changeHandler = this.changeHandler.bind(this)
        this.state = {
            inputContent: ''
        }
    }
    // 监听改变事件
    changeHandler(e) {
        e.stopPropagation()
        e.preventDefault()

        this.setState({
            inputContent: e.target.value
        })
    }
    render() {
        return (
            <div>
                <input type="text" onChange={this.changeHandler}/><span>{this.state.inputContent}</span>
            </div>
        )
    }
}

class App extends Component {
    render() {
        return (
            <div className="App">
                <Input />
                <TestButton />
            </div>
        );
    }
}

export default App
```
其中，从组件获取真实DOM的节点，我们用到了`ref`属性
另外，之前`React.createClass`中事件是默认绑定到当前类中，但是使用es6语法的话，this指向div的支撑实例（backing instance），需要手动绑定this才会指向当前类

## 后记
React写的时候感觉语法限制没有那么多，跟vue也有一些相似之处，比如`ref`。但我更喜欢它更清晰地传达了虚拟DOM的思想。
找回努力学习的感觉吧少女！期待下次更新> <

## 参考
 - [React 入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html)
 - [React 入门](http://www.imooc.com/learn/504)
