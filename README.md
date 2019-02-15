# 《React设计模式与最佳实践》阅读笔记



[TOC]

## React基础

### 声明式编程

- [ ] 这里的概念需要整理

### React元素

#### 定义

和界面相关，只包含展示界面所必须的信息。描述了屏幕上需要显示的内容，是构成组件的基本要素。

#### 本质

实际上是一个对象，组件和DOM元素可以通过`props/children`可以不断向下嵌套。React会递归的取回整个__DOM节点树__然后渲染，过程被称为一致性比较，React DOM 和 React Native 均是通过这种方式在各自的平台上创建 UI。

* type：如果是字符串，元素就表示DOM节点；如果是函数，那么就是组件

```javascript
// DOM 元素
const element = <div>元素</div>

// element
{
        $$typeof: Symbol(react.element),
        key: null,
        props:{
            children: "元素",
            __proto__: Object
        }
        ref: null,
        type: "div",
        _owner: null,
        _store: {validated: false},
        _self: null,
        _source: null,
        __proto__: Object
    }

// 组件
const component = function (props) {
        return <div>组件</div>   
    }
// component
ƒ component(props) {
    return React.createElement(
        'div',
        null,
        '\u7EC4\u4EF6'
    );
}
```

### 与传统模板的区别

Mustache

```javascript
{{#items}}
    {{#first}}
    	<li><strong>{{name}}</strong></li>
    {{/first}}
    {{#link}}
    	<li><a href="{{url}}">{{name}}</a></li>
    {{/link}}
{{/items}}
```

React

```js
render() {
    return (
        <button style={{ color: 'red' }} onClick={this.handleClick}>
        	Click me!
        </button>
    )
}
```

#### 结论

模板高度依赖从逻辑层接收到的数据模型来显示信息，样式同样也严重依赖模板的结构。所以传统的关注点分离原则更像是技术分离。

React则是将逻辑，数据，样式均放到一个组件内。最终目标是将创建组件所用到的每项技术都封装起来，并根据它们的领域和功能进行关注点分离。



## 整理代码

### JSX

#### JSX是什么

React.createElement()`的语法糖。

#### 特性

- 通过开头字母的大小写来区分创建HTML元素或者React元素。
- 属性的设置通过XML的方式，和html元素的属性设置方式相同（属性名不能为JavaScript的保留字，样式期望传入对象，样式属性为驼峰写法）
- 子元素同样是直接嵌套在标签内
- 通过`{{}}`来调用变量或者表达式
- 只能有一个根元素（因为每个元素是一个createElement和函数，不能同时返回两个函数）
- JSX会过滤空格，需要显示的插入`{' '}`

### Babel是什么

JavasScript的转译器，将代码转译成目标环境支持的代码，React中用来处理JSX语法。

### 最佳实践

元素传递属性时通过展开属性来传递，避免传递整个引用对象。

书写JSX时换行更加易读，并且换行后需要用`（）`包括。

属性同样在换行后更加易读。

元素的渲染判断用行内的方式，`if...else`可以用三元条件运算。

将判断条件封装到函数内或者设置 `get`属性

直接将数组进行map并返回一个数组，可以清晰的创建列表

`jsx-control-statements` 库可以将条件语句包装正逻辑标签。

### 代码风格管理

#### ESLint

规则分三个等级：`off/0`，`wran/1`，`error/2`

### 函数式编程

#### 高阶函数

接受一个函数作为参数，返回另一个函数，通常会做一些增强操作。

#### 纯粹函数

不产生副作用，不会改变自身作用域以外的任何东西。

#### 柯里化

将多参数函数转换成在不同阶段传入单个参数的函数。

```js
const add = (x, y) => x + y
```

to

```
const add = x => y => x + y

const add1 = add(1)
add1(2) // 3
add1(3) // 4
```

#### 组合

将不同的函数组合成为新函数，这样可以编写更小而简单，易于测试的纯粹函数



#### 函数式编程与UI

这主要是描述一种视角，即将UI看作传入状态的函数，通过运用函数式编程的一些概念来组织这些UI，达到两者相结合的目的。