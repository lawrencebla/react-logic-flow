# ReactClass

ReactClass模块包含2个方法，`createClass`和`injection`。

## createClass
createClass是React中最常见的方法之一，该方法接收自定义组件的一些配置属性参数(spec)，如render, getDefaultProps等。

### createClass具体做了如下操作:

1. 生成一个构造函数。

2. 将混入的spec和当前接受的spec生成一个新的spec。

	1. 如果spec中有mixins属性，则优先混入子属性的mixins属性，直到子属性不再有mixins属性。
    ```js
    if (spec.hasOwnProperty(MIXINS_KEY)) {
    RESERVED_SPEC_KEYS.mixins(Constructor, spec.mixins);
    }
    ```

	2. 混入spec中的displayName、childContextTypes、contextTypes、getDefaultProps、propTypes、statics、autobind方法到构造函数属性中。
    statics加入处理后的构造函数属性中。

    3. 收集需要自动bind this的方法。

	4. 将spec中的的非标准属性放入构造函数原型中。

3. 调用getDefaultProps方法。
    
    调用`getDefaultProps`方法，并将结果赋值给构造函数的`defaultProps`属性。

4. 返回上述构造函数。

## injection

在react被启动的时候，会优先调用该方法注入一些需要的内容，如`getDOMNode`**(已弃用)**。
