### JavaScript预编译

#### JS执行过程

1. 语法分析，检测代码中语法错误；
2. 预编译，本质是创建AO和GO对象，对其属性操作；
3. 解释执行，js将会逐行读取代码执行；

#### JS预编译

预编译简单理解就是在内存中开辟一块空间，用来存放变量和函数，在javascript中，会在内存中使用\<code>var\<code>关键字声明的变量和使用\<code>function\<code>关键字声明的函数开辟空间，用来存放这两者的变量和函数，其中编译时 \<code>function\<code>的优先级比\<code>var\<code>高;

##### 预编译什么时候执行

预编译分为全局编译和局部编译，全局编译发生在页面加载完成时执行，而局部编译发生在函数执行前一刻；

> Tip：预编译阶段发生变量声明和函数声明，没有初始化行为（赋值）；匿名函数不参与预编译，只有在执行阶段才会进行变量初始化。

特点：**任何变量，如果未声明就赋值，那么该变量为全局变量，即暗示全局变量（imply global）**，并且所有的全局全局变量都是window的属性。

```JS
// 暗示全局变量：在一个变量未声明，直接赋值，该变量为全局变量，即window所有
function fn(){ 
  b = 10
  var a = b = 10
}
fn()
// console.log(a); // error
console.log(window.b); // 10
```

#### 预编译步骤

##### 局部预编译的4个步骤

1. 创建AO对象（Activation Object），执行期上下文；
2. 找形参和变量声明，将变量和形参名作为AO属性名，值为undefined；
3. 将实参值和形参统一；
4. 在函数里面找函数声明，值赋予函数体；

###### 示例：

```js
function test(a) {
  console.log(a);
  var a = 2;
  console.log(a);
  function a () {}
  console.log(a);
  var b = function () {}; // 函数表达式
  console.log(b);
  function d(){}
}
test(1);
```

第一步 ：创建AO对象

```json
AO{
  //此时空 对象
}
```

第二步：确定AO对象属性

```json
AO{
  a: undefined, // 函数参数
  b: undefined //声明的变量
}
```

第三步：将实参赋值给形参

```json
AO{
  a: 1 // 函数参数
  b: undefined // 声明的变量
}
```

第四步：处理函数里面声明函数

```json
AO{
  a: function a () {} // 函数参数
  b: undefined // 声明的变量
  d: function d(){}
}
```

此时函数预编译已经完成的，预编译后执行代码：
第一条执行的是控制台打印出a的值，所以输出function a () {}；
第二条语句赋值给a，则AO对象中a的值被覆盖为2；
第三条语句控制台打印a的值为2；
第四条为声明，预编译处理过所以直接跳过；
第五条打印出a的值，一样为2；
第六条为赋值，赋值b的值为function () {}；
第七条打印出b的值function () {}；
第八条声明，预编译处理过所以直接跳过；

##### 全局预编译的3个步骤

1. 创建GO对象（Global Object），全局对象;
2. 找变量声明，将变量名作为GO的属性名，值为undefined；
3. 查找函数声明，作为GO属性，值赋予函数体；

```JS
// GO 示例 
console.log(a); // undefined
a = 100;
function test() {
  console.log(a); // undefined
  a = 200;
  console.log(a); // 200
  var a = b = 300; // a 声明提升；b未声明，作为window对象
}   
test();
console.log(b); // 300
var a; // 声明变量，提升 
console.log(a); //100
```

##### js中函数声明先提升还是变量先提升

同一个标识符的情况下，变量声明与函数声明都会提升；函数声明会覆盖变量声明，但不会覆盖变量赋值，即：如果声明变量的同时初始化或赋值那么变量优先级高于函数。

```js
console.log(a);    // f a() {console.log(10)}
console.log(a());    //  10
var a = 3;
function a() {
  console.log(10)
}
console.log(a)   // 3
a = 6;
console.log(a());  //  a:6,  输出 a is not a function;
```

###### 其他示例

```JS
function fn(a, c) {
  console.log(a); // function a
  // 变量声明+变量赋值，（只提升变量声明，不提升变量赋值）
  var a = 123
  console.log(a); // 123
  console.log(c); // function c
  // 函数声明
  function a(){}
  if(false){
    var d = 578
   }
  console.log(d); // undefined
  console.log(b); // undefined
  // 函数表达式
  var b = function(){}
  console.log(b); // function(){}
  function c(){}
  console.log(c); // function c
}
fn(1, 2)

// 预编译
function fn(a = undefined, c = undefined){ // 形参声明
  function a(){}; // 函数声明
  var d = undefined;
  var b = undefined; // b为函数表达式
  function c(){}

  // 按顺序执行
  console.log(a); // function a
  a = 123
  console.log(a); // 123
  console.log(c); // function c
  if(false){ 
    d = 578 // 未执行
  }
  console.log(d); // undefined
  console.log(b); // undefined
  b = function(){}
  console.log(b); // function(){}
  console.log(c); // function c
}
fn(1,2)

// AO过程
// AO: {
//   a: undefined  1  f a(){} 123
//   c: undefined  2  f c(){}
//   b: undefined f(){}
//   d: undefined
// }
```

