## 当setState或其他操作跟新组件时：

将需要更新的callback, state, 
加入
_pendingStateQueue(setState)
_pendingReplaceState(replaceState)
_pendingForceUpdate(forceUpdate)
_pendingCallbacks
_pendingElement(set/replaceProps, 通过cloneAndReplaceProps)

1. 开启ReactUpdatesFlushTransaction事务

2. 开始ReactReconcileTransaction事务，获取当前input元素的range，锁住事件，清空回调池；

3. 如果prevParentElement ！== nextParentElement
执行componentWillReceiveProps
通过_pendingStateQueue设置新的state

4. 通过_pendingForceUpdate或者执行classInst.shouldComponentUpdate判断是否更新

5. 执行classInst.componentWillUpdate方法

6. 执行classInst.render方法生成新的Element

7. 如果新旧Element类型一样，执行子Inst的receiveComponent
如果子集不是Dom
将新的Element和老的Element传入更新
如果是Dom
通过props更新attr
更新props中的child的_currentElement、_stringText并更新dom

8. 将componentDidUpdate添加到ReactReconcileTransaction CallbackQueue

9. 检查是否有运行时添加的更新

10. 执行ReactReconcileTransaction CallbackQueue
11. 清空ReactUpdatesFlushTransaction