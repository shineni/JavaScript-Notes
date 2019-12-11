
* 作用域
* 变量
* 运算符
* 闭包
***
## 1. 相关概念
### 变量的生命周期
![](https://user-gold-cdn.xitu.io/2019/3/28/169c324a80a8d565?w=187&h=221&f=png&s=8667)
### 作用域
#### 概念
    JavaScript在ES6之前只存在函数作用域，在ES6中引入了let,const等变量声明关键字与块级作用域。
    作用域（Scope）即代码执行过程中的变量、函数或者对象的可访问区域，作用域决定了变量或者其他资源的可见性；计算机安全中一条基本原则即是用户只应该访问他们需要的资源，而作用域就是在编程中遵循该原则来保证代码的安全性。除此之外，作用域还能够帮助我们提升代码性能、追踪错误并且修复它们。JavaScript 中的作用域主要分为全局作用域（Global Scope）与局部作用域（Local Scope）两大类。
#### 用途
    在编译的时候会用到作用域。
    编译的过程：
    (1)词法分析（这个过程会将由字符组成的字符串分解成（对编程语言来说）有意义的代码块）
    (2)语法分析（这个过程是将词法单元流（数组）转换成一个由元素逐级嵌套所组成的代表了程序语法结构的树。这个树被称为“抽象语法树”）
    (3)代码生成（将这棵“树” 转换为可执行代码，将我们写的代码变成机器指令并执行）
函数对象的[[scope]]属性(内部属性,只有JS引擎可以访问, 但FireFox的几个引擎(SpiderMonkey和Rhino)提供了私有属性__parent__来访问它)
    

### 变量提升


# 为什么出现？（var的不足）
## var的两个问题
 1. 可以重复声明  
 2.  全局作用域
### 为了加强对变量生命周期的控制引入了块级作用域
## let和const的区别
const 声明不可以修改绑定，但是可以修改值
`
const name = "shine"

name
"shine"
name="aladdin"
VM218:1 Uncaught TypeError: Assignment to constant variable.
    at <anonymous>:1:5
(anonymous) @ VM218:1
const user = {name:"shine", age:16}

user.name="aladdin"

user //{name: "aladdin", age: 16}

`
## let 和const出现后的问题
临时死区（Temporal Dead Zone）TDZ
这是因为 JavaScript 引擎在扫描代码发现变量声明时，要么将它们提升到作用域顶部(遇到 var 声明)，要么将声明放在 TDZ 中(遇到 let 和 const 声明)。访问 TDZ 中的变量会触发运行时错误。只有执行过变量声明语句后，变量才会从 TDZ 中移出，然后方可访问。

作者：冴羽
链接：https://juejin.im/post/5b0238f66fb9a07aca7a74ba
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```
var value="global";
(function(){
	console.log(value);
	let value = "local"
}())
VM488:3 Uncaught ReferenceError: value is not defined
```
参考链接：

    https://zhuanlan.zhihu.com/p/28494566
    https://segmentfault.com/a/1190000016514414
    http://www.laruence.com/2009/05/28/863.html
    https://scotch.io/tutorials/understanding-scope-in-javascript