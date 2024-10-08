---
title: React快速入门
img: https://images.pexels.com/photos/11990061/pexels-photo-11990061.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500
excerpt: React的语法
top: false
toc: true
tocOpen: true
onlyTitle: false
comments: true
share: true
copyright: true
donate: false
categories: 计算机
tags: [前端,React,JSX,TSX,JavaScript,TypeScript]
mathjax: true
date: 2022-05-16 17:02:22
---

# React快速入门

## 一、React

### 1.JSX

> JSX全称JavaScript XML，与HTML结构相似，大大降低学习成本。

#### 1.1 前言

```bash
#初始化react项目
npx create-react-app 项目名称
# 启动项目
cd 项目名称
npm start
```

注意：

1. React元素的属性名使用驼峰命名法
2. 特殊属性名：class->className，for->htmlFor、tabindex->tabIndex
3. 没有子节点的React元素可以用`/>`结束。
4. 小括号包裹JSX语法

JSX嵌入JS表达式

* 数据存储在JS中
* 语法：{JS表达式}
* JSX也是合法的JS表达式
* 不能出现对象和语句

#### 1.2 JSX条件渲染

```jsx
const isLoading = false
const loadData=()=>{
    if(isLoading) return <div>loading...</div>
    return <div>数据加载完成</div>
}
const title =(
    <h1>
        {loadData()}
    </h1>
)
```

#### 1.3 JSX列表渲染

* 要渲染一组数据，应该使用数组的map()方法
* 每一个列表都应该添加key属性且不可变保持唯一

```jsx
const songs=[
    {id: 1,name: '我'},
    {id: 2,name: '号'}
]

const list = (
    <ul>
        {songs.map(item=> <li key={item.id}>{item.name}</li>)}
    </ul>
)
```

#### 1.4 JSX样式处理

```jsx
//导入css
import './index.css';

const list=(
    <h1 className="title">
        JSX样式处理
    </h1>
)
```

### 2.TSX

#### 2.1 使用CRA创建支持TS的项目

```bash
# 创建一个支持ts的react项目
npx create-react-app my-app --template typescript
# 在现有react项目添加TS支持
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```

* 项目新增了`tsconfig.json`用来配置TS的编译选项
* 文件扩展名为 `*.tsx`
* 项目中`src`文件夹下增加了`react-app-env.d.ts`，用来声明React的默认类型
  * 三斜线指令：指定依赖其他的类型声明文件

#### 2.2 函数组件类型

```ts
//可以舍弃JS的PropTypes进行类型检查，利用Ts的语法来实现其功能
type MyProps = {
  name: string;
  age?: number;
}

const Hello = ({ name, age = 18 }: MyProps) => {
  return <div>My Name is {name} ,my age is {age}</div>;
}

const App = () => {
  return (
    <div>
      <Hello name="John" age={30} />
    </div>
  );
}

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

#### 2.3 class组件类型

```ts
type State = { count: number };
type Props = {message?: string};

//无props无state
class App extends React.Component {}

//无props有state
class App2 extends React.Component<{}, State> {}

//有props无state
class App3 extends React.Component<Props> {}

//有props有state
class App4 extends React.Component<Props, State> {}

```

```ts
// props属性与state属性的类型
type State = { count: number };
type Props = {name: string, age?: number};

class App extends React.Component<Props> {
  state: State = {
    count: 0
  };

  handleClick = () => {
    this.setState({
      count: this.state.count + 1
    });
  }

  render() {
    const { name, age = 18 } = this.props;
    return (
      <div>
        <h1>Hello, {name}</h1>
        <h2>You are {age} years old</h2>
        <h3>{this.state.count}</h3>
        <button onClick={this.handleClick}>Click me</button>
      </div>
    );
  }
}
```

### 3.组件

> 组件是React核心
> 组件特点：可复用性、独立、可组合性

#### 3.1创建组件

* 函数创建组件

注意：

1. 函数名必须大写字母开头
2. 必须有返回值，表示该组件的结构
3. 如果返回值为null表示不渲染任何内容

```jsx
//普通函数创建组件
function Hello(){
    return <div>这个是我的函数组件</div>   
}

//箭头函数创建组件
const Hello = ()=> <div>这个是我的函数组件</div>

//渲染组件
ReactDom.render(<Hello />,document.getElementById('root'))
```

* 类创建组件

注意：

1. 类名首字母大写
2. 类组件应该继承React.Component父类
3. 类组件必须提供render()方法
4. render()方法必须有返回值

```jsx
//创建类组件
class Hello extends React.Componet{
    render(){
        return <div>这个是我的类组件</div>
    }
}

//渲染组件
ReactDom.render(<Hello />,document.getElementById('root'))
```

**每个组件都应该放在单独的文件中**:

步骤：

1. 创建组件文件
2. 在文件中导入React
3. 创建组件
4. 导出组件
5. 在需要使用的文件中导入该组件

```jsx
import React from 'react'

//创建组件
class Hello extends React.Componet{
    render(){
        return <div>这个是我的JS组件</div>
    }
}

//导出组件
export default Hello
```

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import Hello from './Hello.js'

//渲染组件
ReactDom.render(<Hello />,document.getElementById('root'))
```

#### 3.2 事件处理

* 事件绑定语法：
  * `on+事件名称={事件处理程序}`
  * 事件的名称采用驼峰命名法

```jsx
class App extends React.Component {
  //事件处理函数
  handleClick = () => {
    console.log('click');
  }
  render() {
    return <button onClick={this.handleClick}>按钮</button>;
  }
}

//渲染组件
const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(
  <App />
);
```

* 事件处理对象
  * 可以通过事件处理程序的参数获得事件对象
  * 该合成事件对象兼容所有浏览器

```jsx
class App extends React.Component {
  //事件处理函数
  handleClick(e) {
    e.preventDefault(); //利用事件对象阻止浏览器默认行为
    console.log('click');
   }
  render() {
    return <button onClick={this.handleClick}>按钮</button>;
  }
}
```

#### 3.3 有状态组件和无状态组件

* 函数组件叫做无状态组件，类组件叫做有状态组件
* 状态（state）即数据
* 函数组件没有自己的状态，只负责数据的展示（静）
* 类组件有自己的状态，负责更新UI，让组件动起来
* 状态（state）值是对象，是组件的私有数据
* 只能用`setState()`来修改`state`数据
* `setState()`用来修改状态和更新UI
* 思想：数据驱动视图

```jsx
class App extends React.Component {
  // 在这里初始化状态
  state = {
    count: 0
  }
  render() {
    return (
      <div>
        <h1>计数器：{this.state.count}</h1>
        <button onClick={() => this.setState({count: this.state.count + 1})}>+1</button>
      </div>
    );
  }
}
```

> 逻辑代码独立出去后this指向问题处理

* 箭头函数

```js
class App extends React.Component {
  // 在这里初始化状态
  state = {
    count: 0
  }
  // 在这里更新状态
  onAddOne = () => {
    this.setState({  //this必须指向实例
      count: this.state.count + 1
    })
  }
  render() {
    return (
      <div>
        <h1>计数器：{this.state.count}</h1>
        <button onClick={this.onAddOne}>+1</button>
      </div>
    );
  }
}
```

* ES5的bind()函数

```jsx
constructor(){
    super()
    this.onAddOne = this.onAddOne.bind(this)
}
```

#### 3.4 表单处理

* 受控组件：受到React控制的html

步骤：

1. 在state中添加一个状态，作为表单元素额value值
2. 给表单元素绑定change事件，将表单元素的值 设置为state的值 (双向绑定)

```jsx
class App extends React.Component {
  // 在这里初始化状态
  state = {
    txt: '',
  }
  // 在这里更新状态
  onChange = (e) => {
    this.setState({
      txt: e.target.value,
    })
  }
  render() {
    return (
      <div>
        <input type="text" value={this.state.txt} onChange={this.onChange} />
      </div>
    );
  }
}
```

> 多表单元素统一处理

```jsx
class App extends React.Component {
  // 在这里初始化状态
  state = {
    txt: '',
    content: '',
    city: 'SZ',
    isChecked: false,
  }
  // 在这里更新状态
  onChange = (e) => {
    const target = e.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }
  render() {
    return (
      <div>
        <input type="text" name='txt' value={this.state.txt} onChange={this.onChange} /><br />
        <textarea name='content' value={this.state.content} onChange={this.onChange} /><br />
        <select name='city' value={this.state.city} onChange={this.onChange}>
          <option value="BJ">北京</option>
          <option value="SH">上海</option>
          <option value="SZ">深圳</option>
        </select><br />
        <input type="checkbox" name='isChecked' checked={this.state.isChecked} onChange={this.onChange} /><br />
      </div>
    );
  }
}
```

* 非控组件：操作DOM获取元素数据

1. 调用React.createRef()创建一个ref对象
2. 将创建好的ref对象添加到文本框中
3. 通过ref对象获取到文本框的值

#### 3.5 小结案例--评论

```jsx
class App extends React.Component {
  // 在这里初始化状态
  state = {
    comments: [
      { id: 1, name: 'Pete Hunt', text: 'This is one comment' },
      { id: 2, name: 'Jordan Walke', text: 'This is *another* comment' },
    ],
    username: '',
    content: '',
  }
  // 在这里更新状态

  commentsList = () => {
    return (this.state.comments.length === 0 
      ?<div className='noComment'><strong>暂无评论，快去评论吧~</strong></div>
      :(
        <ul>
          {this.state.comments.map((comment) => {
            return (
              <li key={comment.id}>
                <h3>{comment.name}:</h3>
                <p>{comment.text}</p>
              </li>
            );
          })}
        </ul>
      )
    );
  }

  onChange = (e) => {
    const { name, value } = e.target;
    this.setState({
      [name]: value
    });
  }

  addComment = () => {
    const comment = {
      id: this.state.comments.length + 1,
      name: this.state.username,
      text: this.state.content,
    };
    this.setState({
      comments: [...this.state.comments, comment],
    });
  }

  render() {
    return (
      <div className='app'>
        <div>
          <input type="text" name='username' value={this.state.username} className='user' placeholder='输入姓名' onChange={this.onChange}/><br/>
          <textarea name='content' className='text' value={this.state.content} placeholder='输入评论' onChange={this.onChange}/><br/>
          <button className='btn' onClick={this.addComment}>提交</button>
        </div>
        <div><h2>评论列表</h2></div>
        {this.commentsList()}
      </div>
    );
  }
}

//渲染组件
const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(
  <App />
);
```

### 4.组件通讯

#### 4.1 组件接收数据

组件是独立且封闭的单元，默认情况下只能使用自己的数据，而对于一个复杂的项目通常需要拆分多个组件，而多个组件需要数据传递就是组件通讯。

props的作用：接收传递给组件的数据

* 传递数据：给组件标签添加属性
* 接收数据：函数组件通过参数props接收数据，类组件通过this.props接收数据

```js
class App extends React.Component {
  render() {
    return (
      <div>
        <h1>{this.props.name}</h1>
      </div>
    );
  }
}

//渲染组件
const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(
  //传递字符串、数值、数组、函数
  <App name='jack'  age={19} colors={['read','green']} fn={()=> console.log('Hello')}/>  
);
```

注意：

* props无法对属性赋值，这是个只读属性
* 类组件在使用构造函数时，需要把props作为构造函数和super()的参数

#### 4.2 父组件传递给子组件

1. 父组件要提供传递的state数据
2. 给子组件标签添加属性，值为state中的数据
3. 子组件通过props接收父组件中传递的数据

```jsx
class Father extends React.Component {
  state = {
    name: '王五',
  }
  render() {
    return (
      <div>
        <Child name={this.state.name} />
      </div>
    );
  }
}

const Child = (props) => {
  return (
      <h1>{props.name}</h1>
  );
}
```

#### 4.3  子组件传递数据给父组件

1. 父组件提供回调函数
2. 将该函数作为属性值传递给子组件
3. 子组件通过props调用回调函数
4. 将子组件的数据作为参数传递给回调函数

```jsx
class Father extends React.Component {
  state = {
    msg: ''
  }
  say=(data)=>{
    console.log('father say:', data);
    this.setState({
      msg: data
    })
  }
  render() {
    return (
      <div>
        <Child say={this.say} />
      </div>
    );
  }
}
class Child extends React.Component {
  state = {
    data: 'hello'
  }
  handleClick=()=>{
    this.props.say(this.state.data);
  }
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>click</button>
      </div>
    );
  }
}
```

#### 4.4 兄弟组件通信

1. 状态提升
2. 将多个组件需要共享的状态提升到他们最近的父组件中
3. 公共父组件职责：提供共享状态、提供操作共享状态的方法
4. 要通讯的子组件只需通过props接收状态或操作状态的方法

```jsx
class Father extends React.Component {
  state = {
    count: 0,
  }
  add = () => {
    this.setState({
      count: this.state.count + 1,
    })
  }
  render() {
    return (
      <div>
        <Child1 count={this.state.count} />
        <Child2 add={this.add} />
      </div>
    );
  }
}
const Child1 = (props) => {
  return (
    <div>
      <h1>计数器：{props.count}</h1>
    </div>
  )
}
const Child2 = (props) => {
  return (
    <div>
      <button onClick={()=> props.add()}>+1</button>
    </div>
  )
}
```

#### 4.5 跨多个组件通信

1. 生产者与消费者
2. 调用React.createContext()创建Provider和Consumer两个组件
3. 使用Provider在通信组件的最近父组件
4. 设置value属性，表示要传递的数据
5. 使用Consumer组件接收数据

```jsx
const {Provider, Consumer} = React.createContext();

class App extends React.Component {
  state = {
    count: 0,
  };
  
  add = () => {
    this.setState({
      count: this.state.count + 1,
    });
  };

  render() {
    return (
      <Provider value={{count: this.state.count,add: this.add}}>
        <div>
          <App1 />
          <App2 />
        </div>
      </Provider>
    );
  }
}

const App1 = () => (
  <div>
    <Consumer>
      { value => <h1>{value.count}</h1> }
    </Consumer>
  </div>
);

const App2 = () => (
  <Consumer>
    {value => {
      return (
        <button onClick={ ()=>value.add() }>Click</button>
      );
    }}
  </Consumer>
)
```

### 5.组件进阶

#### 5.1 静态类型检查

prop-types库提供props属性的类型校验

```jsx
App.propTypes={
  a: PropTypes.number,                 //属性a ，数值类型
  fn: PropTypes.func.isRequired,  //属性fn，函数且为必填项
  tag: PropTypes.element,            //React元素
  filter: PropTypes.shape({          //对象，area属性为字符串，price为数值
    area: PropTypes.string,
    price: PropTypes.number,
  })
}
```

prop-types库提供props属性默认值

```jsx
App.defaultProps={
  a: 10,
}
```

#### 5.2 组件的生命周期

* 生命周期：组件从创建到挂载到页面中运行，再到组件不用时卸载的过程。
* 生命周期的每个阶段总是伴随着一些方法的调用，这些方法就是生命周期的钩子函数
* 钩子函数：为开发人员在不同阶段提供操作时机
* 只有类组件才有生命周期

**创建时**:

* 执行时间：组件创建时
* 执行顺序：`constructor()--->render()--->componentDidMount()`

| 钩子函数          | 触发时间                      | 作用                                     |
| ----------------- | ----------------------------- | ---------------------------------------- |
| constructor       | 组件创建时，最先执行          | 1.初始化state   2.为事件处理程序绑定this |
| render            | 每次渲染都会触发              | 渲染UI（不能调用setState）               |
| componentDidMount | 组件挂载（DOM渲染完成）后触发 | 1.发送网络请求   2.DOM操作               |

**更新时**:

* 执行时间：调用函数 1.`setState()` 2.`forceUpdate()` 3.组件接收到新的`props`
* 执行顺序：`render()--->componentDidUpdate()`

| 钩子函数           | 触发时间                      | 作用                      |
| ------------------ | ----------------------------- | ------------------------- |
| render             | 每次渲染都会触发              | 渲染UI                    |
| componentDidUpdate | 组件更新（DOM渲染完成）后触发 | 1.发送网络请求  2.DOM操作 |

**卸载时**:

* 执行时间：组件消失

| 钩子函数             | 触发时间       | 作用     |
| -------------------- | -------------- | -------- |
| componentWillUnmount | 组件卸载时触发 | 清理工作 |

**新的生命周期函数**:

| 钩子函数                 | 触发时间                         | 用法                                                                          |
| ------------------------ | -------------------------------- | ----------------------------------------------------------------------------- |
| getDerivedStateFromProps | 第一次初始化组件以及后续的更新前 | 静态函数，需要加static。 返回一个对象作为新的state，返回null则不需要更新state |
| getSnapshotBeforeUpdate  | 组件更新前                       | 在更新前获得过去的快照                                                        |

#### 5.3 组件复用

props的children属性

* 表示了组件的子节点，当组件标签有子节点时，props就会有该属性
* 与普通props一样，值可以是任意值。

利用复用来处理相似功能的方法
两种方式：

  1. render props模式
  2. 高阶组件

步骤：

1. 创建复用的组件，在复用组件中提供状态和操作状态的方法
2. 将复用组件的状态作为props.render(state)方法的参数，暴露到组件外
3. 外部使用render()的属性接收参数并使用

```jsx
class Mouse extends React.Component {
  state = {
    x: 0,
    y: 0
  }
  handleMouseMove = (event) => {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }
  componentDidMount() {
    document.addEventListener('mousemove', this.handleMouseMove);
  }
  render() {
    return this.props.render(this.state);
  }
}
class App extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={(mouse) => (
          <p>The mouse position is ({mouse.x}, {mouse.y})</p>
        )} />
      </div>
    );
  }
}
```

children属性 代替 render属性

```jsx
class Mouse extends React.Component {
  state = {
    x: 0,
    y: 0
  }
  handleMouseMove = (event) => {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }
  componentDidMount() {
    document.addEventListener('mousemove', this.handleMouseMove);
  }
  render() {
    return this.props.children(this.state);
  }
}
class App extends React.Component {
  render() {
    return (
      <Mouse>
        {(mouse) => (
          <div>
            <h1>Move the mouse around!</h1>
            <p>The current mouse position is ({mouse.x}, {mouse.y})</p>
          </div>
        )}
      </Mouse>
    );
  }
}
```

代码优化：

```jsx
//卸载时，解除事件绑定
componentWillUnmount() {
  document.removeEventListener('mousemove', this.handleMouseMove);
}
//添加类型限制
Mouse.propTypes = {
  children: PropTypes.func.isRequired
}
```

高阶组件采用包装（装饰）模式来实现状态逻辑复用

步骤：

1. 创建一个函数，名称为with开头
2. 指定函数参数，为大写字母开头
3. 在函数内部创建一个类组件，提供复用的状态逻辑代码
4. 在该组件中，渲染参数组件，同时将状态通过prop传递给参数组件
5. 调用该高阶组件，传入要增强的组件，通过返回值拿到增强后的组件，并将其渲染到页面中

```jsx
function withMouse(WrappedComponent){
  class Mouse extends React.Component {
    state = {
      x: 0,
      y: 0
    }
    handleMouseMove = (event) => {
      this.setState({
        x: event.clientX,
        y: event.clientY
      });
    }
    componentDidMount() {
      document.addEventListener('mousemove', this.handleMouseMove);
    }
    componentWillUnmount() {
      document.removeEventListener('mousemove', this.handleMouseMove);
    }
    render() {
      return <WrappedComponent {...this.state}/>
    }
  }
  //设置装饰不同组件的名字，给调试使用
  Mouse.displayName = `withMouse(${getDisplayName(WrappedComponent)})`;
  return Mouse;
}

function getDisplayName(WrappedComponent) {
  return WrappedComponent.displayName || WrappedComponent.name || 'Component';
}

const Position = (props) => {
  return (
      <p>The current mouse position is ({props.x}, {props.y})</p>
  );
}

const MousePosition = withMouse(Position);

class App extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <MousePosition/>
      </div>
    );
  }
}
```

高阶组件的props的传递丢失问题：

```jsx
//把props继续往下传
render() {
  return <WrappedComponent {...this.state} {...this.props}/>
}
```

#### 5.4 setState函数

* setState()函数是异步更新的（有延迟）

```jsx
//原来语法
this.setState({
  x: event.clientX,
  y: event.clientY
});
```

```jsx
//推荐语法，表示最新的state和props
this.setState((prevState,props) => {
  return {
    x: event.clientX,
    y: event.clientY
  };
},()=>{
  //第二个参数，回调函数，在状态更新完成后立即执行

});
```

#### 5.5 组件性能优化

```jsx
//在render()之前运行，用来判断组件是否渲染，避免重复渲染
shouldComponentUpdate(nextProps,nextSate){
  //状态不是自己的用nextProps进行比较，状态是自己的用nextSate进行比较
  //需要重新渲染
  return true
  //不需要重新渲染
  return false
}
```

类组件继承`React.PureComponent`，自己已经实现`shouldComponentUpdate`钩子函数，来对前后props和state进行比较，来决定是否进行本次渲染。

当要对比引用类型的值时，需要重新创建对象，才能更新组件。

#### 5.6 虚拟DOM和Diff算法

执行过程：

1. React初始state，创建虚拟DOM对象树
2. 根据虚拟DOM生成真正的DOM，渲染到页面中
3. 当数据变化后，重新根据新的数据，创建新的虚拟DOM对象树
4. 与上一次得到的虚拟DOM，使用Diff算法进行对比，得到需要更新的内容
5. 最终，React只将变化的内容更新到DOM中，重新渲染到页面。

### 6.Hooks

1. 高阶组件复用代码复杂
2. 生命周期复杂
3. 函数组件是无状态组件

#### 6.1 useState

>接收一个初始值，返回一个状态值和一个更新函数

```jsx
const [state, setState] = useState(initialState);
```

> 更新函数通过接收一个新的值来更新状态值

```jsx
setState(newState);
```

>Example

```jsx
import React,{useState} from 'react';

const {Provider, Consumer} = React.createContext();

const App = () => {
  const [count, setCount] = useState(0);
  const add = () => setCount(count + 1);
  return (
    <Provider value={{count, add}}>
      <App1 />
      <App2 />
    </Provider>
  );
}

const App1 = () => (
  <div>
    <Consumer>
      { value => <h1>{ value.count }</h1> }
    </Consumer>
  </div>
);

const App2 = () => (
  <Consumer>
    {value => {
      return (
        <button onClick={ value.add }>Click</button>
      );
    }}
  </Consumer>
)
```

#### 6.2 useEffect

函数组件没有生命周期

useEffect作用：通过使用这个 Hook，你告诉 React 你的组件需要在渲染后做一些事情。React 会记住你传递的函数（我们将它称为我们的“效果”），并在执行 DOM 更新后调用它。

useEffect运行时间：默认情况下，它在第一次渲染后和每次更新后运行。

```jsx
// useEffect不加第二个参数，监测所有数据，一旦数据发生改变就会调用该Hook
useEffect(() => {
  document.title = `You clicked ${count} times`;
});
```

```jsx
// useEffect加第二个参数，监测指定数据，一旦指定数据发生改变就会调用该Hook
useEffect(() => {
  document.title = `You clicked ${count} times`;
},[count]);
```

```jsx
const App = () => {
  const [count, setCount] = useState(0);

  const add = () => setCount(count + 1);
  const remove = () =>  ReactDOM.unmountComponentAtNode(document.getElementById('root'));

  // return后面的数据将会在组件卸载后执行
  useEffect(() => {
    const timer = setInterval(() => {
      setCount(count => count + 1);
    }, 1000);
    return () => {
      clearInterval(timer);
    };
  },[]);

  return (
    <Provider value={{count, add, remove}}>
      <App1 />
      <App2 />
    </Provider>
  );
}
```

常用操作：

1. 发起ajax请求
2. 设置订阅/启动定时器
3. 更改DOM

#### 6.3 useContext

函数组件中处理跨多个组件通信的问题

>单文件跨组件通信

```jsx
const Context = React.createContext();

const App = () => {
  const [count, setCount] = useState(0);

  const add = () => setCount(count + 1);

  return (
    <Context.Provider value={{count, add}}>
      <App1 />
      <App2 />
    </Context.Provider>
  );
}

const App1 = () => {
  const {count} = useContext(Context);
  return <h1>{count}</h1>;
}

const App2 = () => {
  const {add} = useContext(Context);
  return <button onClick={ add }>Add</button>
}
```

>多文件跨组件通信

```jsx
// ------------"./components/Add"--------------------
import React, { useContext } from 'react';

const App2 = (props) => {
    const { add, remove } = useContext(props.context);
    return (
        <>
            <button onClick={add}>Add</button>
            <button onClick={remove}>Remove</button>
        </>
    );
}

export default App2;

//-------------'./components/Show'---------------------
import React,{useContext} from 'react';

const App1 = (props) => {
    const {count} = useContext(props.context);
    return <h1>{count}</h1>;
};

export default App1;

//------------"./App.jsx"--------------------
import React, { useEffect, useState } from 'react';
import App1 from './components/Show';
import App2 from './components/Add';

const Context = React.createContext();

const App = (props) => {
  const [count, setCount] = useState(0);

  const add = () => setCount(count + 1);
  const remove = () => props.root.unmount();

  useEffect(() => {
    const interval = setInterval(() => {
      setCount(count => count + 1);
    }, 1000);
    return () => clearInterval(interval);
  }, []);

  return (
    <Context.Provider value={{ count, add, remove }}>
      <App1 context={Context} />
      <App2 context={Context} />
    </Context.Provider>
  );
}

export default App;

// --------"./index.js"-------------------
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App.jsx';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App root={root}/>
  </React.StrictMode>
);
```

### 7.外部常用库

#### 7.1 axios请求数据

axios是基于promise对ajax的一种封装

```shell
# 安装axios
npm i axios
```

* 发起GET请求
  `axios.get('url',{params:{（js对象）参数}}).then(callback)`
* 发起POST请求
  `axios.post('url',{（js对象）参数}).then(callback)`
* 其他方式

  ```jsx
  axios({
    method: '请求类型',
    url: '请求的URL地址',
    data: {数据},
    params: {参数}
    // then获得返回的数据，catch在请求错误时处理，可以无限叠加
  }).then(result => result).catch(err => callback).
  ```

#### 7.2 Router路由

现代前端应用大多都是SPA（单页应用程序），也就是只有一个HTML页面的应用程序，可以减轻服务器压力，用户体验好，为了有效使用单页面管理原来的多页面的功能，前端路由应运而生。

React Router官网：<https://reactrouterdotcom.fly.dev/docs/en/v6>

>安装

```bash
npm i react-router-dom
```

```js
import { BrowserRouter } from 'react-router-dom';
import 'bootstrap/dist/css/bootstrap.min.css';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

> 基础使用 Routes, Route, Link, Outlet

```jsx
import {Routes, Route, Link, Outlet} from 'react-router-dom';

const Header = () => (
  <>
    <h1>Header</h1>
    <p>
      Outlet 标签为子元素占位符，为子元素占位，布局页面
    </p>
    <ul>
      <li><Link to="/">Home</Link></li>
      <li><Link to="/First">First</Link></li>
      <li><Link to="/Second">Second</Link></li>
    </ul>
    <hr />
    <Outlet/>
  </>
);

const Home = () => (
  <>
    <h1>Home</h1>
    <p>
      This is the home page
    </p>
  </>
);

const First = () => (
  <>
    <h1>First</h1>
    <p>
      This is the first page
    </p>
  </>
);

const NotFound = () => (
  <>
    <h1>Not Found</h1>
    <p>
      This page is not found
    </p>
  </>
);

const App = () => (
  <Routes>
    <Route path="/" element={ <Header/> }>
      <Route index element={ <Home/> }/>
      <Route path="First" element={ <First/> }/>
      <Route path="*" element={ <NotFound/> }/>
    </Route>
  </Routes>
);
```

#### 7.3 Ant Design的UI库

```shell
# 安装UI样式
npm install antd --save
# 安装依赖修改webpack
npm install @craco/craco
npm install craco-less
npm install less-loader less
```

修改 `package.json` 里的 `scripts` 属性

```json
"scripts": {  
  "start": "craco start", 
  "build": "craco build", 
  "test": "craco test", 
}
```

然后在项目根目录创建一个 `craco.config.js` 用于修改默认配置

```js
// 修改为绿色主题
const CracoLessPlugin = require('craco-less');

module.exports = {
  plugins: [
    {
      plugin: CracoLessPlugin,
      options: {
        lessLoaderOptions: {
          lessOptions: {
            modifyVars: { '@primary-color': '#1DA57A' },
            javascriptEnabled: true,
          },
        },
      },
    },
  ],
};
```

#### 7.4 Redux

专门用于状态管理的JS库，集中管理React组件的共享状态
从 UI 层完全抽离出来，只负责管理数据，让 React 只专注于 View 层的绘制。

中文文档：<https://cn.redux.js.org/>

英文文档： <https://redux.js.org/>

```shell
# 安装 redux
npm install @reduxjs/toolkit
npm install react-redux
```

> 创建 Redux Store

```js
import { configureStore } from '@reduxjs/toolkit'

// 导入别的文件配置好的 reducer
import counterReducer from '../features/counter/counterSlice'

// 把reducer存到store中，store的属性可以随便改
export default configureStore({
  reducer: {
    counter: counterReducer
  }
})
```

> 创建 Slice Reducer

```js
import { createSlice } from '@reduxjs/toolkit'

export const counterSlice = createSlice({
  //name字符串用作每个action类型的第一部分, 每个reducer函数的键名用作第二部分
  name: 'counter',
  // 初始化state值
  initialState: {
    value: 0
  },
  // 定义操作属性的方法
  reducers: {
    increment: state => {
      state.value += 1
    },
    decrement: state => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})
//导出操作属性的方法给组件使用
export const { increment, decrement, incrementByAmount } = counterSlice.actions
//导出 reducer
export default counterSlice.reducer
```

> React Counter 组件

```js
import React, { useState } from 'react'
import { useSelector, useDispatch } from 'react-redux'
import {
  decrement,
  increment,
  incrementByAmount,
  incrementAsync,
  selectCount
} from './counterSlice'
import styles from './Counter.module.css'

export function Counter() {
  // 从store中导出属性值给组件使用
  const count = useSelector(selectCount)
  // 导出store的dispatch函数来给组件调用方法
  const dispatch = useDispatch()
  // 给组件显示属性
  const [incrementAmount, setIncrementAmount] = useState('2')

  return (
    <div>
      <div className={styles.row}>
        <button
          className={styles.button}
          aria-label="Increment value"
          onClick={() => dispatch(increment())} // 调用方法
        >
          +
        </button>
        <span className={styles.value}>{count}</span>
        <button
          className={styles.button}
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          -
        </button>
      </div>
    </div>
  )
}
```
