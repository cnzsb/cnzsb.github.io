title: '从一个 CRUD 上手 React 和 AntD'
date: 2017-03-09 23:36:05
tags: [React, AntD]

---

换了新公司后，技术栈使用的是 react。虽然一路从 vue 1 到 vue 2，但是我还是对 react 有好感的，当初学习 vue 1 文档少的可怜的时候也是借鉴了不少 react 的文章才得以理解，因此在接下来的文章中会小部分的对比下 react 和 vue 的不同。本次的文章主要是关于初次接触 react，并开发出一个具有 CRUD 功能页面的过程。提前了解本项目的详情请点击 [GitHub 地址](https://github.com/cnzsb/react-antd-crud)和[线上预览地址](http://www.zhaoshibo.net/react-antd-crud/)。

<!-- more -->

## 需求背景

本项目其实是一个入职的 training project，项目需求就是做一个 CRUD 的管理页面。要求基础框架仅使用 react （不含 redux 之类的）和 antd，AJAX 库使用的是公司基于 axios 封装的库。项目提供了数据库和一个不完整的接口（好吧，其实只有其中一张表的 get），鉴于此这个项目使用了前端的方法仅做展示使用。

## 学习 React

先把项目需求放一边，对于没接触过 react 自然应该先把基本的语法学习一下。我是先看了官方的 [tutorial](https://facebook.github.io/react/tutorial/tutorial.html) 之后去看[文档](https://facebook.github.io/react/docs/installation.html)，因为时间比较紧，所以只计划了 2 天的时间，这点时间我只看到了 “Optimizing Performance” 这一章。

### 与 Vue 的异同

因为接触过 vue，所以在读文档时自然而然就会在心中做一个对比。

#### 1. 语法区别

虽然 vue 2 已经支持 JSX 的写法了，但是多数情况下都是用模板语法进行开发的。而 react 的 “all in js” 的理念一上来会让代码读起来有点痛苦，不过适应了之后其实和模板语法也没什么区别，简单理解在 vue 中习惯把 `template` 放在最上面，而 react 的 `render` 是在最下面的。另外在 react 中的作用域 `this` 需要特别注意。

vue 提供了 `v-for`、`v-if` 等的语法方便渲染 DOM。而在 react 中推荐抽离组件的思维，也就是一个可以复用的 `li` 完全可以自成一个组件，随后我们可以利用 JS 的 `map`、`if` 等方法直接进行操作。

#### 2. 单项数据流和双向绑定

react 推崇单向数据流，而 vue 是双向绑定的。vue 从 1 升级到 2 之后也开始推荐单向传递、父子组件独立的思维，如移除了 `props` 的 `twoway`等。

这里不得不提的就是 `props`，vue 2 版本中为了排除父子组件的耦合，移除了旧的 `$dispatch` 和 `$broadcast`，假设不使用 eventbus 和 vuex 的情况下，我的开发思路一般都是子组件仅用来接受上层处理好的数据而不独立处理数据，因此方法一般都存在于父组件之中。

而 react 在不使用其他状态管理的情况下，一般需要通过父组件提供的方法来操作从父组件传进子组件的数据。假设存在父组件 A 和 子组件 B，处理方法如下：

```js
// 子组件 B
class B extends React.Component {
    render() {
        return(
            <div>
                <label>
                子组件的输入框：
                    <input value={this.props.value} onChange={this.props.onChange} />
                </label>
            </div>
        )
    }
}

// 父组件 A
class A extends React.Component {
    constructor() {
      super()       // 一般情况下传递参数 props 其实没有用，而如果在 constructor 中则使用了 this.props 则必须写入参数
      this.state = {
        value: ''
      }
      
      this.onChange = this.onChange.bind(this)
    }
    
    onChange(event) {
        this.setState({
            value: event.target.value
        })
    }

    render() {
        return (
            <div>
                <B value={this.state.value} onChange={this.onChange} />
                <p>父组件的内容：{this.state.value}</p>
            </div>
        )
    }
}
```

在嵌套过深的父子组件中，这种单向的方式非常方便追踪数据问题等，而开发过程会相对麻烦一些。这在接下来的 CRUD 项目中把我绕的非常晕，而等到表单出问题的时候又让我立马体会到了其中的便利。

#### 3. 生命周期

react 的生命周期提供了更多的钩子，方便我们决定在哪里进行操作和定位等。这里通过一张不知来源的图作为参考。

![React 生命周期](http://7xlivs.com1.z0.glb.clouddn.com/2017/03/09/%E4%BB%8E%E4%B8%80%E4%B8%AA%20CRUD%20%E4%B8%8A%E6%89%8B%20React%20%E5%92%8C%20AntD/React%20%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

## 准备开发

通过与已知知识的对比来学习新技术会帮助更好的上手，时间问题了解了大概核心的问题后基本上就可以开始项目了。在开始项目之前还需要大概了解下要使用的 UI 库，了解一下开发中可能用到的部分即可：布局、表单、表格、分页、按钮和通知等。

### 1. 搭建环境

本文的环境以 [github](https://github.com/cnzsb/react-antd-crud) 上的仓库来介绍。实际开发时使用的是公司脚手架生成的包含 java 的环境，并在 tomacat 的服务器上进行的开发。

本次项目的重点是实践 react，因此环境部分的配置不过多介绍。我利用了官方脚手架 [creat-react-app](https://github.com/facebookincubator/create-react-app) 生成文件目录。之后修改了生成目录为 docs，为了配合 github-pages 来显示；修改了 webpack 中 babel-loader 的配置来实现 antd 组件的按需加载。

### 2. 开发思路

开发之前需要理清楚页面的层次结构和组件结构。 这里先把最后的项目目录放下来，然后来讲设计思路。

```bash
|-- api
|   |-- sqlData.js      # 操作 sqlData 的 ajax 方法
|-- components
|   |-- App.jsx         # 页面
|   |-- Content.jsx     # 页面主体内容区
|   |-- Factory.jsx     # 表单组件工厂
|   |-- FormModal.jsx   # 弹出的增加或编辑的表单组件
|   |-- Search.jsx      # 搜索组件
|-- libs
|   |-- ajax.js         # ajax 实例及公共方法
|   |-- util.js         # 工具方法
|-- store
|   |-- sqlConfig.js    # 数据库表单配置项
|-- index.js            # 入口文件
|-- style.css           # 样式文件
```

首先项目需求中本来是存在多张表的，虽然因为种种原因仅只提供了一张表的获取接口，但是为了能够在有第 n 张表来的时候可以随意配置，因此我单独抽离了数据库配置文件，为了就是以后再有新需求可以“偷懒”少写代码。

在 `sqlConfig.js` 中我定义了每一个需要展示的 table header 的 `type`，从而按需渲染不同的表单类型，如 `input` 的 `text` 类型，或者仅做展示的 `display` 类型，以后还可以扩展 `date` 类型等。另外根据 antd 中的配置需要，增加了对应的比较重要的配置项，如 `width`。另外我也定义了 `validators` 来对每一项进行校验，实际项目中因为时间问题并没有完善此功能。

在有了思路后，就要去布局组件的位置，从而决定哪里最适合负责总的 state 管理，哪里又适合 props。

```bash
|-- App
|   |-- Menu                # 导航菜单
|   |-- Content             # 主要内容区
|       |-- Search          # 搜索组件
|           |-- Factory     # 表单工厂
|       |-- ButtonGroup     # 操作按钮群
|       |-- Table           # 表格组件
|           |-- Pagination  # 分页组件
|       |-- FormModal       # 弹出的编辑表单
|           |-- Factory     # 表单工厂
```

在最终的 react 组件中，按照重要程度，结构大致如上。我们通过下图来了解组件间具体如何通信。

![react-antd-crud](http://7xlivs.com1.z0.glb.clouddn.com/2017/03/09/%E4%BB%8E%E4%B8%80%E4%B8%AA%20CRUD%20%E4%B8%8A%E6%89%8B%20React%20%E5%92%8C%20AntD/react-antd-crud.png)

基本的结构有了之后，接下来就是 ajax 具体如何使用了，这里我们使用了 axios，并对 ajax 进行一个全局的默认设置，把所有的错误都进行了同样的提示，这些设置保存在 `libs/ajax.js` 文件中。

## 开发总结

整个开发过程使用了三天时间。最难的第一个地方就是在接受 `props` 之后选择哪一个生命周期的钩子函数进行 `setState` 更新；第二个地方是组件之间各种状态传递的时候实在把我绕晕了，主要还是没有适应 react 的模式；第三个地方是在使用 antd 的 `table` 组件的 `rowSelection` 时，为了每一次都能让 `selectedRowKeys` 与组件对应 `state` 中的该值同步，需要把 `rowSelection` 的赋值操作放在 `render` 中，从而触发每一次组件更新的 `render` 方法。

除了缺点之外，本项目做到了抽离及复用组件，项目中的 `Factory` 组件可以根据配置文件生成对应类型的表单元素，进一步开发的话可以把 `validators` 的功能增加进去。对于编辑和新增表单这些验证规则可能和搜索组件 `Search` 中的不太一样，如果有要求的话，可以再定制维护一份对应搜索组件的 `Factory` 也是可以的。综上而言本项目在之后引入新的数据库表和对应的接口的话，配置相关的 `sqlConfig` 文件即能轻松实现复用。另外项目中简单的配置了 axios 的响应数据类型后的规则，做到了总的处理，具体的相关配置还需要参考[官方文档](https://github.com/mzabriskie/axios)。

综上，本篇文章侧重记录了项目的主要流程及思考方式，并没有详细解释 react 或者 antd 的相关知识，权当抛砖引玉，深入的内容还需要不断学习才能掌握，多看文档多实践。
