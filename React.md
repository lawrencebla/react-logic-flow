# 概述

在开发人员可以接触到的React世界中，React由2个部分组成。

1. Element
2. Class

### Element:

我们所有的jsx的语法都可以视为Element
如实例中的`<HelloMessage name="John" />`和`<div>Hello {this.props.name}</div>`；

Element中有type属性，可以视为jsx语法中的标签；

* 当jsx标签是原生html标签时，type值为标签字符串；

* 当jsx标签是自定义组件(Class)时，type为该Class。

*** 主要相关文件: [ReactElement](./ReactElement.md) ***

### Class:

开发人员自定义的组件叫做Class，是一个构造函数，如`HelloMessage`。

*** 主要相关文件: [ReactClass](./ReactClass.md) ***

### Component:

在React内部，加入了新的成员`Component`, 它有多种展现方式，如`CompositeComponent`、`DOMComponent`等；
`Component`将`Element`作为属性，放入`Component`中。

在React内部，所以的操作都是基于`Component`。

### Transaction:

React中的事务管理，一般用于控制一大堆逻辑的开始和结束。

*** 详见: [Transaction](./Transaction.md) ***

### 工作流

1. 构造需要用到的组件([详情](./Structure.md))

2. 初始化组件及页面([详情](./Init.md))

2. 更新组件([详情](./Update.md))
