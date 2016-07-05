---
layout: default_post
---

`set`语句和`get`语句作为**函数**绑定在对象(`Object`)的属性上。当访问该属性时，`get`函数被调用。当对该属性赋值时，`set`函数被调用。

<br/>

### 创建一个getter

```javascript
var obj = {
  counter: 0,
  get prop() {
    this.counter++;
    console.log('now counter is '+this.counter);
  }
}
```

当访问对象`obj`的属性`prop`时，绑定在该属性上的`get`函数会被执行。

如：访问两次`obj.prop`，控制台会分别输出`now counter is 1`和`now counter is 2`。

<br/>

### 通过Object.defineProperty()定义对象属性

`Object.defineProperty()`方法可以在对象上定义一个新属性，或修改原有属性。以上创建`get`的语句也可以这样写：

```javascript
var obj = { counter: 0 };

// define obj.prop
Object.defineProperty(obj,'prop',{
  get : function() {
    this.counter++;
    console.log('now counter is '+this.counter);
  }
})
```
同样地，访问两次`obj.prop`，控制台会分别输出`now counter is 1`和`now counter is 2`。

<br/>

### 对象属性的两种描述符(Property descriptor)


对象里目前存在的属性描述符有两种主要形式：数据描述符(data descriptors)和存取描述符(accessor descriptors)。

数据描述符是一个拥有可写或不可写的值(`value`)的属性，而存取描述符是由`getter-setter`对描述的属性。**描述符必须是两者之一，不能同时拥有两种形式**。

上述两个例子中，如果在控制台输出`obj`，浏览器(Chrome)得到的未展开结果是`Object {counter:0}`。这是因为`obj.counter`是拥有`value`值的数据描述符，而`obj.prop`是由`getter-setter`定义的存取描述符，没有`value`值。

每个属性都有一个描述对象(Descriptor)，用来控制属性的行为。要查看属性具体的描述对象(Descriptor)，可以使用`Object.getOwnPropertyDescriptor`方法。

例如， `Object.getOwnPropertyDescriptor(obj, 'counter')`方法输出`obj.counter`的描述对象：

```javascript
Object {value: 0, writable: true, enumerable: true, configurable: true}
```

而`Object.getOwnPropertyDescriptor(obj, 'prop')`方法输出`obj.prop`的描述对象：

```javascript
Object {set: undefined, enumerable: false, configurable: false}
```

展开可以查看已经定义的`get`函数。

因此， 上述对`obj.counter`的赋值语句

```javascript
var obj = { counter:0 };
```

也可以通过`Object.defineProperty()`方法定义，方法中传入的第三个参数对象即为该属性的描述对象(Descriptor)：

```javascript
var obj = {};

Object.defineProperty(obj,'counter',{
  value: 0,
  writable:true,
  enumerable: true,
  configurable: true
});
```

<br/>

### 参考

* 关于描述对象(Descriptor)可以参考[这里](http://es6.ruanyifeng.com/#docs/object#属性的可枚举性)
* 关于getter可以参考[这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/get)
* setter文中没有详细描述用法，与getter类似，可以看[这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/set)
* Object.defineProperty()方法的用法看[这里](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)