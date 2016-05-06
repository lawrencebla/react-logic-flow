# Transaction

在React中，`Transaction`相当一个通道，将需要执行的方法包装起来，执行该方法时，需要先从通道的入口中进入，执行完后，再从通道的出口出来；React使用`Transaction`来控制逻辑顺序执行。

具体实现
`Transaction`被实例化时，通过复写的`getTransactionWrappers`方法定义管道；
实例调用`perform`方法，接收真正要执行的业务逻辑`method`。
执行所有管道的`initialize`方法；
执行业务逻辑`method`；
执行所有管道的close方法 

3个主要的`Transaction`: `ReactDefaultBatchingStrategyTransaction`、 `ReactReconcileTransaction`、 `ReactUpdatesFlushTransaction`。

`ReactDefaultBatchingStrategyTransaction`:
用于处理初始化或更新时的整个流程，当这个流程中有多次state、props被改变时，合并成一次更新Update。

`ReactReconcileTransaction`:
主要用于处理组件的生命周期控制；
在被执行时，处理过程中控制住页面的事件，避免事件混乱，并且记录输入框的光标;
当渲染完成后还原光标，恢复事件机制，执行后续的生命周期方法(didMount)。

在初始化时，`ReactDefaultBatchingStrategyTransaction`与`ReactReconcileTransaction`关系如下：
图

`ReactUpdatesFlushTransaction`:
与`ReactDefaultBatchingStrategyTransaction`和`ReactReconcileTransaction`一起，实现更新流程。
关系如下：
图

当被`setState`或`forceUpdate`时，触发`ReactDefaultBatchingStrategyTransaction`中`close`的update逻辑，在update中，使用`ReactReconcileTransaction`实现真正的update，并将`didUpdate`回调放入`ReactUpdatesFlushTransaction`的`close`中;

#Pooler
为同一类对象添加不同的实例化参数。

`addPoolingTo`给该类对象的构造函数添加pool功能；
`getPooled`获取对象实例；
`release`调用实例的`destructor`方法，并在下次`getPooled`的时候返回实例并执行。

