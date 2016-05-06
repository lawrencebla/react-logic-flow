# ReactElement

ReactElement中主要是构造函数和createElement函数，里面封装了type、key、ref、props等属性，其中type在大部分情况下为ReactClass的createClass方法返回的构造函数，也就是我们自定义的组件。

## 构造函数
```js
var ReactElement = function(type, key, ref, self, source, owner, props) {
	...
}
```
将type, key, ref, owner,props加入Element对象中，并返回。

## createElement
```js
ReactElement.createElement = function(type, config, children) {
	...
}
```
将config中的ref, key, self, source提取出，并与children, type.defaultProps结合，生成新的props；
将提取出的ref, key, self, source与新的props, 参数type传入构造函数

## cloneElement
```js
ReactElement.cloneElement = function(element, config, children) {
	...
}
```
从element中提取出props, key, ref, self, source, owner, type或对象拷贝
后面操作与createElement一致。