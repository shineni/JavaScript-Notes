# JavaScript-Notes
## 1. 变量提升
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
### 思考题： 代码中出现重名变量
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
1:如果是同名的函数，JavaScript编译阶段会选择最后声明的那个。
2:如果变量和函数同名，那么在编译阶段，变量的声明会被忽略
## 2.调用栈
> 执行上下文创建的条件
- **JavaScript执行全局代码**，会编译全局代码并创建全局执行上下文，而且在整个页面的生存周期内，全局执行上下文只有一份
- **调用一个函数**， 函数体内的代码会被编译，并创建执行上下文， 函数执行结束以后，创建的执行上下文会被销毁
- **使用eval函数的时候**，eval的代码也会被编译，并创建执行上下文
> 调用栈
> 栈溢出：调用栈是有大小的，当入栈的执行上下文超过一定的数据，JS引擎就会报错，这就是栈溢出。调用栈有两个指标，最大栈容量和最大调用深度，只要有一个不满足就会溢出。
- 函数调用：运行一个函数，函数名加括号，如showName()
-  调用栈：管理执行上下文的栈称为执行上下文栈，又称调用栈，栈中的元素满足先进先出的特点。
```

var a = 2
function add(b,c){
  return b+c
}
function addAll(b,c){
var d = 10
result = add(b,c)
return  a+result+d
}
addAll(3,6)
```
*  第一步 :   
1. 编译阶段： 创建全局执行上下文，并压入栈底
    - 变量环境
      ```
      var a = undefined
      function add(b,c){
        return b+c
      }
      function addAll(b,c){
      var d = 10
      result = add(b,c)
      return  a+result+d
      }
      ```
    - 语法环境
 2. 执行阶段： 调用函数addAll()进入第二步
 
* 第二步： 调用函数时创建函数的执行上下文
 1.编译阶段
   - 变量环境
   ```
   函数参数列表 b, c
   var d = undefined
   result= undefined
   var d = undefined
   ```

   - 语法环境
 2.执行阶段:给变量赋值，执行函数add(b,c)进入第三步
 
 * 第三步： 调用函数时创建函数执行上下文
 1.编译阶段
   - 变量环境
   ```
   函数参数列表 b, c

   ```
   - 语法环境
 2.执行阶段:给变量赋值，返回result = 9 后从此时的执行上下文从栈顶弹出
 
 第四步：将addAll的执行结果返回，并将其执行上下文从栈顶弹出，最终只剩下全局执行上下文
 
 ### 思考题：如何优化代码，解决栈溢出的问题(斐波拉契，runStack要执行50000次)
 ```
 
function runStack (n) {
  if (n === 0) return 1;
  if (n===1) return 1;
  return runStack( n- 2);
}
runStack(50000)
 ```
 > 尾递归：就是一个函数执行的最后一步是将另外一个函数调用并返回。[Link]https://juejin.im/entry/592e8a2d0ce463006b510b34
 ```
   // 尾调用正确示范1.0
  function f(x){
    return g(x);
  }

  // 尾调用正确示范2.0
  function f(x) {
    if (x > 0) {
      return m(x)
    }
    return n(x);
  }
 ```
```
/**
 * 优化思路：调用栈中只保留一个调用记录，就是让上一个函数不需要有啥保留的内容
 * 1.尾递归优化
 * 2.循环
 */
 
function runStack(n, result) {
if (n === 0) return result;
return runStack(n - 2, result);
}

function main(n, result) {
console.log(runStack(n, result));
}
main(5000, 100);

// es6
function runStack(n, result = 100) {
if (n === 0) return result;
return runStack(n - 2, result);
}

console.log(runStack(5000));
```

