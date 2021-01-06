### 一、react 出现的历史背景及特性
#### 1、react 出现的背景
1. 传统ui操作关注了太多的细节；（react解决了ui的细节问题）
2. 应用程序状态分散在各处，难以追踪和维护；

#### 2、react 很简单
1. 1个新概念；（组件）
2. 4个必须API；（）
3. 单向数据流；
4. 完善的错误提示；

#### 3、传统的 mvc 架构的问题
1、传统的mvc架构有非常多的model、view， 这些model和view之间的使用关系是错综复杂的，而且是双向绑定的；这会导致，当业务程序复杂之后，出现了问题很难去追踪究竟是model上出了问题还是页面操作上出的问题。

#### 小结：
1. 传统 web ui 开发的问题；
2. react 始终整体刷新页面；
3. flux 单向数据流；


### 二、将UI组织成组件树的形式
#### 1、理解react组件
1. react组件一般不提供方法，而是某种状态机；
2. react组件可以理解为一个纯函数；
3. 单向数据绑定；

#### 2、创建一个组件的基本思路
1. 创建静态ui；
2. 考虑组件的状态组成（组件的状态是由外部传入props，还是自己维护state）；
3. 考虑组件的交互方式；

#### 3、受控组件&非受控组件（表单元素）
1. 受控组件：表单状态由使用者维护（受控组件value、onchange）
2. 非受控组件：表单元素状态由DOM自身维护（ref）；

#### 4、创建组件原则：单一职责原则
1. 每个组件只做一件事；
2. 如果组件变的复杂，那么应该拆分成小组件；

#### 5、数据状态管理原则：DRY原则（Don't Repeat Yourself）
1. 能计算得到的状态就不要单独存储；
2. 组件尽量无状态，所需数据通过props获取；

#### 小结
1. 以组件方式思考UI的构建；
2. 单一职责原则；
3. DRY 原则；

### 三、JSX是语法糖而不是模板引擎
#### 1、 jsx使用方式

```
// 1. JSX本身也是表达式
const element = <h1>Hello, world!</h1>

// 2. 在属性中使用表达式；
<MyComponnet foo={1 + 2 +3}></MyComponnet>

// 3. 延展属性
const props = {firstName: 'Ben', lastName: 'Hector'};
const greeting = <Greeting {...props} />;

// 4. 表达式作为子元素
const element = <li>{props.message}</li>


```
#### 2、JSX优点
1. 直观：声明式创建界面；
2. 灵活：代码动态创建界面；
3. 无需学习新的模板引擎；

#### 3、约定
1. React认为小写的tag是原生DOM节点；
2. 大写字母开头为自定义组件；
3. JSX标记可以直接使用属性语法，
```
例如：<menu.Item />
```

#### 4、小结
1. jsx的本质；
2. 如何使用jsx
3. jsx的优点

### 四、React 生命周期及其使用场景
#### 1、constructor
1. 用于初始化内部状态，很少使用；
2. 唯一可以直接修改state的地方；

#### 2、getDerivedStateFromProps
1. 当state需要从props初始化时使用；
2. 尽量不要使用：维护两者状态一致性会增加复杂度；
3. 每次render都会调用；
4. 典型场景：表单控件获取默认值；

#### 3、componentDidMount
1. UI渲染完成后调用；
2. 只执行一次；
3. 典型应用场景：获取外部资源，调用api；

#### 4、componentWillUnmount
1. 组件移除时被调用；
2. 典型场景：资源释放；

#### 5、getSnapshotBeforeUpdate
1. 在页面render之前调用，state已更新；
2. 典型场景：获取render之前的DOM状态；

#### 6、componentDidUpdate
1. 每次UI更新后被调用；
2. 典型场景：页面需要根据props变化重新获取数据；

#### 7、shouldComponentUpdate
1. 决定Virtual DOM 是否需要重绘；
2. 一般可以由 PureComponent 自动实现；
3. 典型场景：性能优化；

#### 小结
1. react 生命周期方法；
2. react 生命周期方法使用场景；

### 五、jsx的运行基础：Virtual DOM
#### 1、diff 策略
1. 广度优先分层比较

#### 2、虚拟DOM的两个假设
1. 组件的DOM结构是相对稳定的（很少发生跨层移动）；
2. 类型相同的兄弟节点可以被唯一标识（key）；

#### 小结
1. 算法复杂度为O(n)；
2. 虚拟DOM如何计算diff；
3. key属性的作用（对于同类型的兄弟元素diff）；

### 七、组件设计模式：高阶组件&函数作为子组件
#### 1、高阶组件：函数参数为一个组件，返回一个新的组件；

#### 2、函数作为子组件：this.props.children(arg)

#### 小结
1. 高阶组件和函数子组件都是设计模式；
2. 可以实现更多场景的组件复用；

### 八、理解context API 及其使用场景
#### 1、context 是定义在组件之外的数据状态；
```
const defaultValue = {};
const MyContext = React.createContext(defaultValue);
```
#### 2、MyContext.Provider
```
// context 提供者
<MyContext.Provider>
	...
</MyContext.Provider>
```
#### 3、MyContext.Consumer
```
// context 消费者,这能让你在函数式组件中完成订阅 context,这需要函数作为子元素模式；
<MyContext.Consumer>
  {value => /* 基于 context 值进行渲染*/}
</MyContext.Consumer>
```
#### 4、Class.contextType 
```
// 挂载在 class 上的 contextType 属性会被重赋值为一个由 React.createContext() 创建的 Context 对象。这能让你使用 this.context 来消费最近 Context 上的那个值。你可以在任何生命周期中访问到它，包括 render 函数中
class MyClass extends React.Component {
  componentDidMount() {
    let value = this.context;
    /* 在组件挂载完成后，使用 MyContext 组件的值来执行一些有副作用的操作 */
  }
  componentDidUpdate() {
    let value = this.context;
    /* ... */
  }
  componentWillUnmount() {
    let value = this.context;
    /* ... */
  }
  render() {
    let value = this.context;
    /* 基于 MyContext 组件的值进行渲染 */
  }
}
MyClass.contextType = MyContext;
```

### 八、使用脚手架工具创建React项目
#### 1、create-react-app（基础的配置，适合小型项目）
#### 2、rekit（基于create-react-app，包含了redux、react-router等适合大型项目）
#### 3、sandbox（在线的react运行环境）

### 九、打包和部署
#### 1、为什么需要打包？
1. 编译：jsx、es6等编译成浏览器可执行的；
2. 资源整合；
3. 优化代码；

#### 2、打包注意事项
1. node环境是否设置为production
2. 禁用开发时专用代码，比如logger
3. 设置根路径

### 十、redux
#### 1、redux特性
1. single source of truth（单一数据源）
2. 可预测性 state + action = newState 产生新状态
3. 纯函数更新store（产生新的store）

#### 2、理解store、action、reducer
- store

```
const store = createStore(reducer);
// store的三个api
store.getState();
store.dispatch(action);
store.subscribe(listener);

```

- action

- reducer 

```
reducer 不是改变旧的state，而是产生新的state
(state, action) => newState
所有的reducer会接受所有的action，就是说dispatch的action会被所有的reducer接受。
```

- 常用的工具函数

```
// 把多个reducer合并成一个reducer
combinReducers({...reducers}); 
bindActionCreators(actionCreator, dispatch);
```
**注意：redux是一个通用的状态管理库，并非一定要和react一起使用**

#### 3、在react中使用redux

1. provider
2. connect

#### 4、理解异步action、中间件

2. 中间件作用

- 截获action
- 发出action

#### 小结
1. 异步action不是特殊action，而是多个同步action的组合使用；
2. 中间件是在dispatcher中截获action做特殊处理后再发送action；

#### 5、如何组织action和reducer

#### 6、理解不可变数据
- 为何需要不可变数据？

1. 性能优化(只需要比较两个对象的引用是否变化；不需要深层次的遍历比较对象中具体某个属性是否变化；redux中的store里的每个节点都是不可变数据)
2. 易于调试和跟踪（可以记录变化之前和之后的状态）
3. 易于推测（在任何时刻都可以推测store是由于什么引起的变化，store可定是由action引起的变化， 可以对比变化前后的action推测出action是否被正确处理）

- 如何操作不可变数据

```
// 1. 原生写法
{...}, Object.assign()

// 2. immutability-helper
// 3. immer
```
### 十一、react-router
#### 1、react-router 特性
1. 声明式路由定义；
2. 动态路由；

#### 2、基于路由配置进行资源组织
1. 实现业务逻辑的松耦合；
2. 易于扩展，重构和维护；
3. 路由层面实现lazy load；

#### 3、react-router API
```
1. <Link>: 普通链接不会触发浏览器刷新；
2. <NavLink>: 类似Link，但是会添加当前选中状态；
3. <Prompt>: 满足条件时提示用户是否离开当前页面；
4. <Redirect>: 重定向当前页面，例如登录判断
5. <Route>: 路由配置的核心标记，路径匹配时显示相应组件；
6. <Switch>: 只显示第一个匹配的路由；
```

#### 4、通过url传递参数

### 十二、Next.js
### 十三、测试
#### 1、react让前端单元测试变得容易
1. react应用很少需要直接访问浏览器API；
2. 虚拟DOM可以在nodejs环境运行和测试；
3. redux隔离了状态管理，纯数据层单元测试；

#### 2、单元测试涉及的工具和技术
1. Jest: Facebook 开源的js测试框架；
2. js DOM：浏览器环境的NodeJs模拟；
3. Enzyme：react组件渲染和测试；
4. nock：模拟HTTP请求；
5. sinon：函数模拟和调用跟踪；
6. istanbul：单元测试覆盖率；

### 十四、react开发常用开发&调试工具
#### 1、eslint
1. 使用.eslintrc 进行规则的配置；
2. 使用airbnb的js代码风格；

#### 2、Prettier
1. 代码格式化神器；
2. 更容易保证写出风格一致的代码；vscode中可以安装相应插件， 在保存时会根据.prettierc配置文件中设置的规则格式化代码；

#### 3、react dev tools（chrome plugin）
#### 4、redux dev tools （chrome plugin）

### 十五、前端项目的理想架构：可维护、可扩展、可测试、易开发、易构建
#### 1、易于测试
1. 功能的分层是否明晰；
2. 副作用少；
3. 尽量使用纯函数；

### 十六、拆分复杂度
#### 1、按领域模型（feature）组织代码、降低耦合度——将业务逻辑拆分成高内聚低耦合的模块
#### 2、如何组织component、action、reducer
1. 文件夹结构
	- 按feature组织源文件
	- 组件和样式文件同一级
	- redux单独文件夹
	- 单元测试保持同样目录结构放在tests文件夹

#### 小结：
1. 按feature组织组件，action和reducer；
2. 使用root loader加载feature下的各个资源；
3. 做到高内聚松耦合；

#### 3、在每个feature中，单独定义自己的路由
1. 每个feature都有自己的专属路由配置l
2. 顶层路由使用JSON配置，更易维护和理解；
3. 如何解析JSON配置到react语法；

### 十七、Rekit
#### 1、Rekit是如何工作的
1. 定义了基于feature的可扩展文件夹结构；
2. 基于最佳实践生成代码合管理项目元素；
3. 提供工具和IDE确保代码和文件夹结构遵循最佳实践；

#### 2、通过代码自动生成保持一致性
1. 文件夹结构一致性；
2. 文件名一致性；
3. 变量名一致性；
4. 代码逻辑一致性；

### 十八、react-router管理路由授权
#### 1、
1. 实现基础：react-router 的动态路由机制；（）
2. 区分受保护路由和公开路由；（protected或角色权限admin、role等）
3. 访问未授权路由时重定向到登录页面；

