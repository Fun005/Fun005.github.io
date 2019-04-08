---
title: 谈谈Object.create()
date: 2019-04-08 19:22:16
tags: [javascript,object]
categories: javascript进阶
description: js对象大家创建的多了，但是有真正深入了解过Object.create()吗？今天本文带你好好见识Object.create()
---
# Object.create()

### 一、首先放MDN上的定义：

> `Object.create()`方法创建一个新对象，使用现有的对象来提供新建对象的`__proto__`

语法：

> ```
> Object.create(proto, [propertiesObject])
> ```

参数：

`proto` ：新创建对象的原型对象。

`propertiesObject` 可选。如果没有指定为`undefined` ，则是要添加到新创建对象的可枚举属性（即其自身定义的属性，而不是其原型链上的枚举属性）对象的属性描述符以及相应的属性名称。这些属性对应的`Object.defineProperties()`的第二个参数。

返回值：一个新对象，带着指定的原型对象和属性。

例外：如果`propertiesObject`参数不是`null`或一个对象，则抛出一个`TypeError`异常。



### 二、个人理解

`Object.create(null)`创建的是一个空对象，并且该对象上没有继承`Object.prototype`原型链上的属性或者方法，例如：`toString()`,`hasOwnProperty()`等方法。

![图一](https://upload-images.jianshu.io/upload_images/7513201-c1d1184fa8f020d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/655) 

![图二](https://upload-images.jianshu.io/upload_images/7513201-77470a9f630648b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/637) 



### 三、MDN代码实践

> var o;
>
> // 创建一个原型为null的空对象
>
> o = Object.create(null);
>
> 
>
> o = {}
>
> // 以字面量方式创建的空对象就相当于：
>
> o = Object.create(Object.prototype);
>
> 
>
> o = Object.create(Object.prototype,{
>
> ​    // foo会成为所创建对象的数据属性
>
> ​    foo: {
>
> ​         writeable: true,
>
> ​	 configurable: true,
>
>  	 value: 'hello'
>
> ​    },
>
> ​    // bar 会成为所创建对象的访问器属性
>
> ​    bar: {
>
> ​        configurable: false,
>
> ​        get: function() { return 10 },
>
> ​	set: function(value) {
>    ​          console.log("Setting `o.bar` to ", value);
>    ​     }
>
> ​    }
>
> });
>
> 
>
> function Constructor() {}
>
> o = new Constructor();
>
> // 上面的一句就相当于：
>
> o = Object.create(Constructor.prototype);
>
> // 当然，如果在Constructor函数中有一些初始化代码，Object.create不能执行那些代码。
>
> 
>
> // 创建一个以另一个空对象为原型，且拥有一个属性p的对象
>
> o = Object.create({}, { p: { value: 42 } }) 
>
>  
>
> // 省略了的属性特性默认为false，所以属性p是不可写、不可枚举、不可配置的：
>
> o.p = 24
>
> o.p
>
> // 24
>
>  
>
> o.q = 12
>
> for(var prop in o) {
>
> ​	console.log(prop)
>
> }
>
> // "q"
>
>  
>
> delete o.p
>
> // false 
>
>  
>
> // 创建一个可写的、可枚举的、可配置的属性p
>
> o2 = Object.create({}, {
>
> ​    p: {
>
> ​        value: 42,
>
> ​        writeable: true,
>
> ​	configurable: true 
>
> ​    }
>
> })

### 四、Object.create(null)的使用场景

为什么很多源码作者会使用`Object.create(null)`来初始化一个新对象呢？这是作者的习惯，还是一个最佳实践？ 其实都不是，这并不是作者不经思考随便用的，也不是javascript编程中的最佳实践，而是需要因地制宜，具体问题具体分析。

我们进一步比较一下`Object.create(null)`和`{}`创建控对象的区别：

在chrome打印如下：

![1549424515384](C:\Users\TomKa\AppData\Local\Temp\1549424515384.png)

从上图可以看到，使用`create`创建的对象，没有任何属性，显示`No properties`，我们可以把它当作一个非常**纯净**的map来使用，我们可以自己定义`hasOwnProperty`、`toString`方法，不管是有意还是不小心，我们完全不必担心会将原型链上的同名方法覆盖掉。举个例子：

> //Demo1:
>
> var a= {...省略很多属性和方法...}; 
>
> //如果想要检查a是否存在一个名为toString的属性，你必须像下面这样进行检查： if(Object.prototype.hasOwnProperty.call(a,'toString')){     
>
> ​	... 
>
> } 
>
> //为什么不能直接用a.hasOwnProperty('toString')?
>
> //因为你可能给a添加了一个自定义的hasOwnProperty 
>
> //你无法使用下面这种方式来进行判断,因为原型上的toString方法是存在的：
>
>  if(a.toString){}  
>
> 
>
> //Demo2:
>
> var a=Object.create(null)
>
> //你可以直接使用下面这种方式判断，因为存在的属性，都将定义在a上面，除非手动指定原型： if(a.toString){}

 另一个使用`create(null)`的理由是，在我们使用`for..in`循环的时候会遍历对象原型链上的属性，使用`create(null)`就不必再对属性进行检查了，当然，我们也可以直接使用`Object.keys[]`。

 ### 五、总结

1. 你需要一个非常干净且高度可定制的对象当作数据字典的时候；
2. 想节省`hasOwnProperty`带来的一丢丢性能损失并且可以偷懒少些一点代码的时候;

用`Object.create(null)`吧！其他时候，请用`{}`。 

 

 