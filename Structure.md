已HelloMessage为例：
```js
"use strict";

var HelloMessage = React.createClass({
  displayName: "HelloMessage",

  render: function render() {
    return React.createElement(
      "div",
      null,
      "Hello ",
      this.props.name
    );
  }
});

ReactDOM.render(React.createElement(HelloMessage, { name: "John" }), mountNode);
```

1. 创建Class:
HelloMessage为构造函数, prototype中加入了`render`、`setState`、`forceUpdate`、`replaceState`、`isMounted`方法;
添加displayName属性;
如果有getDefaultProps方法，执行并将值放入`defaultProps`属性。

2. 创建基于HelloMessage的Element(以下简称HelloElement):
HelloElement中的type属性为HelloMessage构造函数,
props为`{ name: "John" }`。

3.创建WrapperElement:
跟随render方法，可以发现在HelloElement外又封装了一层WrappedElement的Element；
WrappedElement的type为`TopLevelWrapper`、props为HelloElement。

4.创建基于WrapperElement的Component实例(以下简称WrapperComponentInstance)：
WrapperComponentInstance为ReactCompositeComponentWrapper的实例化，prototype具有ReactCompositeComponent.Mixin的属性和_instantiateReactComponent属性。
将WrapperComponentInstance的属性_currentElement置为WrapperElement。


总结：
1. 创建自定义组件(Class)层级树。
2. 为自定义组件(Class)层级树添加顶级Component，以便创建统一入口。
3. 实例化顶级Component。