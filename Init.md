## 流程记录:

### 1. 

1.1. 开启`ReactDefaultBatchingStrategyTransaction`并加锁，当解锁前再次开启该事务时，则不执行事务，直接执行业务逻辑。

### 2.

1.2. 开启`ReactReconcileTransaction`：

获取当前input元素的range，锁住事件，清空回调池；

### 3.

3.1. 为*WrapperComponentInstance*的属性`_mountOrder`、`_nativeContainerInfo`设值;

3.2. 实例化`this._currentElement.type`，这里也就是*TopLevelWrapper*，实例后的值简称*classInstance*。

3.3. 设置classInstance

`props`为*WrapperComponentInstance*的`props`、
`updater`为`ReactUpdateQueue`
及`_reactInternalInstance`为*WrapperComponentInstance*

3.4. 另外再次设置*WrapperComponentInstance*的`_instance`值为*classInstance*

3.5. 如果*classInstance*有`componentWillMount`则执行。

3.6. 执行*classInstance*的`render`方法并返回，根据实例，返回的结果为*HelloElement*;

3.7. 设置*WrapperComponentInstance*的`_renderedNodeType`为`COMPOSITE`

### 4.

4.1. 与Structure中的第四步类似, 基于*HelloElement*创建*HelloComponentInstance*：
*HelloComponentInstance._currentElement = HelloELement*
设置*WrapperComponentInstance*的`_renderedComponent`为*HelloComponentInstance*
为*HelloComponentInstance*的属性`_mountOrder`、`_nativeContainerInfo`设值;


4.2. 实例化`HelloMessage`，并执行*getInit*方法，实例后的值简称*HelloMessageInstance*。


4.3. 设置*HelloMessageInstance*
`props`为*WrapperComponentInstance*的`props`、
`updater`为`ReactUpdateQueue`
及`_reactInternalInstance`为*WrapperComponentInstance*

4.4. 另外再次设置*HelloComponentInstance*的`_instance`值为*HelloMessageInstance*

4.5. 执行*HelloMessageInstance*的`render`方法，返回以div为type的Element，以下简称*DivElement*；

4.6. 设置*HelloComponentInstance*的`_renderedNodeType`为`NATIVE`

### 5.

5.1. 再次通过*DivElement*循环
只不过创建的是`ReactDOMComponentInstance`,以下简称*DivComponentInstance*
*DivComponentInstance._tag = "div"*
*DivComponentInstance._currentElement = DivElement*
设置*HelloComponentInstance*的`_renderedComponent`为*DivComponentInstance*

5.2. 创建divEl
DivComponentInstance._nativeNode = divEl;
divEl.internalInstanceKey = DivComponentInstance;
设置el的props;

5.3. 此时*DivComponentInstance*有2个child: "Hello ", "John"
将这2个child转换成对象
key: prefix + index
value: child转换成`ReactDOMTextComponent`实例(`ReactDOMTextComponentIn`)并将`_currentElement`与`_stringText`设置成字符串

5.4. 遍历转换后的child对象，创建*spanEl*
ReactDOMTextComponentInstance._nativeNode = spanEl;
spanEl.internalInstanceKey = ReactDOMTextComponentInstance;
设置spanEl的内容为相应字符串

5.5. 将*spanEl*插入*divEl*
将*divEl*转换成`DOMLazyTree`简称为`markup`

5.6. 如果*DivComponentInstance*有`ref`属性, 将设置`ref`的方法加入加入`ReactReconcileTransaction`的回调池
此时*DivElement*的逻辑做完，回到*HelloElement*的逻辑

### 6.

6.1. 
如果*HelloMessageInstance*有`componentDidMount`方法，将此方法加入`ReactReconcileTransaction`的回调池
如果*HelloMessageInstance*有`ref`属性, 将设置`ref`的方法加入`ReactReconcileTransaction`的回调池

此时*HelloElement*的逻辑做完，回到*WrapperElement*的逻辑

### 7.

7.1. 设置*WrapperComponentInstance._renderedComponent._topLevelWrapper = WrapperComponentInstance*;

也就是*HelloComponentInstance._topLevelWrapper = WrapperComponentInstance*;

7.2. 清空container内容并插入divEl


### 8.

8.1. 还原input元素的range

8.2. 启用Event

8.3. 执行`ReactReconcileTransaction`的回调池

8.4. 清空回调池
将callbackQueue加入CallbackQueue的instance池;
将reactReconcileTransaction加入`ReactReconcileTransaction`的instance池;

### 9.

解锁`ReactDefaultBatchingStrategyTransaction`，如有`setState`等操作，执行更新操作，具体操作详见[Update](./Update.md)。


## 总结: 

1. 开启事务
2. 不断将子Element包装成Component实例并实例化Class，执行getInit
3. 执行componentWillMount
4. 执行render返回孙子Element
...轮询2~4直到返回原生Elemenet
5. 返回原生Element渲染出来的DomNode
* 注： 如果原生Element中包含自定义Element，则以改自定义Element执行2~5, 并将最终渲染出来DomNode返回 *
6. 将DomNode插入页面
7. 按照自定义Element执行的顺序反向执行componentDidMount
