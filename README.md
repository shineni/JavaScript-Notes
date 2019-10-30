# JavaScript-Notes
## 变量提升
### 变量的声明和赋值
var myname = 'shine ni'
> 可以看成有声明和赋值两部分组成
1. 变量声明： var myname= undefined
2. 变量赋值： myname= "shine ni"
### 函数的声明和赋值
```
function foo(){
  console.log('foo')
}

var bar = function(){
  console.log('bar')
}
```
> 第一个函数 foo 是一个完整的函数声明，也就是说没有涉及到赋值操作；
> 第二个函数涉及了变量的声明和赋值，是先声明变量 bar，再把function(){console.log('bar')}赋值给 bar
### Javascript执行经历两个阶段
```
showName()
console.log(myname)
var myname = 'shine ni'
function showName() {
    console.log('函数showName被执行');
}
```
1. 编译阶段
* 生成执行上下文（执行上下文是JavaScript 执行一段代码时的运行环境）
  - 变量环境
   ```
   var myname = undefined
   function showName() {
    console.log('函数showName被执行');
   }
   ```
  - 语法环境
* 生成可执行的代码
```
showName()
console.log(myname)
myname = 'shine ni'
```
2. 执行阶段
### 代码中出现重名变量
```

function showName() {
    console.log('Shine');
}
showName();
function showName() {
    console.log('Shine Ni');
}
showName(); 
```
执行过程分析：
1.编译阶段。遇到第一个ShowName()会将第一个函数的函数体放到变量环境中，当遇到第二个ShowName()，继续将其放于变量环境中，但是变量环境中已经有了ShowName函数，此时第二个ShowName将会覆盖第一个ShowName,因此，这时候变量环境中实际上只存了第二个ShowName函数。
2.执行阶段。由于变量环境中只存在了第二个ShowName函数，因此，两次的执行结果都时Shine Ni.
所以，当一段代码定义了两个相同名字的函数，那么最终生效的时最后一个函数。
