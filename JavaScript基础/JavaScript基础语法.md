JavaScript基础语法-dom-bom-js-es6新语法-jQuery-数据可视化echarts

### 第一部分：ECMAScript

####  1、初始JavaScript

##### 1.5 JS的组成

javascript：ECMAScript（JavaScript语法）、DOM（页面文档对象模型）、BOM（浏览器对象模型）

prompt()：浏览器弹出输入框

#### 2、变量

##### 2.1、声明变量特殊情况    

```javascript
// 1、只声明变量不赋值
var sex;
console.log(sex); // undefined

// 2、不声明、不赋值，直接输出
console.log(name); // 代码提示错误

// 3、不声明，直接赋值
age = 10;
console.log(age); // 10
```

#### 3、数据类型

JavaScript是弱语言，变量的类型会根据具体的值动态变化的。

JavaScript中数据类型主要分两类：

- 简单的数据类型：Number（int、float）、String、Boolean（true、false）、Undefined、Null
- 复杂数据类型：Object（array是特殊对象）

##### 3.1、数字类型Number

```javascript
// 1、八进制数字序列范围：0～7，数字前面加0
var num1 = 07; // 对应十进制7
var num2 = 012; // 对应十进制10
var num3 = 08; // 对应十进制8
var num4 = 019; // 对应十进制19

// 2、十六进制数字序列范围：0～9以及A～F，数字前面加0x
var num = 0xA;

// 3、数字的临界值
console.log('MAX_VALUE', Number.MAX_VALUE);// MAX_VALUE 1.7976931348623157e+308
console.log('MAX_SAFE_INTEGER', Number.MAX_SAFE_INTEGER); // MAX_SAFE_INTEGER 9007199254740991
console.log('MIN_VALUE', Number.MIN_VALUE); // MIN_VALUE 5e-324
console.log('MIN_SAFE_INTEGER', Number.MIN_SAFE_INTEGER); // MIN_SAFE_INTEGER -9007199254740991

// 4、数字类型三个特殊值
console.log(Number.MAX_VALUE * 2); // Infinity 无穷大
console.log(- Number.MAX_VALUE * 2); // -Infinity 无穷小
console.log('aaa' - 1); // NaN  not a number

// 5、进制转化
var num = 19;
console.log('转二进制：', num.toString(2)); // 10011
console.log('转8进制：', num.toString(8)); // 23
console.log('转16进制：', num.toString(16)); // 13

// 6、isNaN() 判断非数字， 返回true：不是数字，false: 是数字
console.log(isNaN(12)); // false
console.log(isNaN('sdfasf')); // true
```

##### 3.2、字符串类型 String

**任何类型的拼接一个字符串，将会生成的是新的字符串**

###### 字符串函数：

length：获取字符串的长度；

##### 3.3、Boolean、Undefined、Null

```javascript
console.log(true + 1); // 2
console.log(false + 0); // 1

var str;
console.log(str); // undefined
console.log(str + 'value'); // undefinedvalue
console.log(str + 1); // NaN undefined和数字相加返回NaN

var val = null;
console.log('value:' + val); // value:val
console.log(val + 1); // 1
```

##### 3.4、类型转换

 1、typeof检测变量类型: number、string、boolean、undefined、object，

- 注意：其中null为object

```javascript
var a = 10;
console.log(typeof a) // number
console.log(typeof null) // object
```

2、转化字符串 toString、String、隐式转换（拼接字符串）

```javascript
// 1、toSring
var num = 1;
var str = num.toString()
// 2、String
var num = 11
var str = String(num)
// 3、拼接字符串 隐式转换
var num = 1;
var str = num + ''
```

 3、字符串转数字：parseInt、parseFloat、Number、隐式转换（算数运算符+、-、*、/）

```javascript
// 1、转整型：parseInt
var str = '111'
var num = parseInt(str)
// 2、转浮点：parseFloat
var str = '3.14'
var num = parseFloat(str)
// 3、Number
console.log(Number('1212')); // 1212
// 4、利用算数运算符+、-、*、/ 隐式转换
console.log('123' - 0); // 123
// 5、字符串首部必须是数字，否则返回NaN
console.log(parseInt('123px')); // 123
console.log(parseInt('rem123px')); // NaN
```

4、转换为布尔型：Boolean

- 空、否定值会转换成false，如：0、NaN、Null、undefined；其他值都会转成true

```javascript
console.log(Boolean('')); // false
console.log(Boolean(0)); // false
console.log(Boolean(NaN)); // false
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false
```

#### 4、运算符

##### 4.1、算术运算符

- 算术运算符包括：+、-、*、/、%、幂（**）、
- 算术运算符有优先级，先乘除、后加减、小括号优先
- 浮点类型精度存在问题

##### 4.2、表达式

##### 4.3、前置递增、后置递增

- 前置递增（++num）：先自加，后返回值 ；
- 后置递增（num++）：先返回原值，后自加；
- 单独使用两者是一样效果；

```javascript
var num = 10;
console.log(++num); // 11

var num = 10;
console.log(num++); // 10

var num = 10;
++num;
console.log(num); // 11

var num = 10;
num++;
console.log(num); // 11

var num = 10;
console.log(++num + 10); // 21

var num = 10;
console.log(num++ + 10); // 20

var num = 10
console.log(num++ + ++num); // 22 num++返回10 ++num返回12
```

##### 4.5、比较运算符

注意：== 会进行隐式转换

##### 4.6、逻辑运算符 &&、||、！

##### 4.7、赋值运算符 -=，+=、*=、/=、%=

```javascript
var num = 2;
num += 2;// num = num + 2 , num=4
```

##### 4.8、运算符优先级

##### 4.9、位运算符

- ~ 非
- |  按位与
- & 按位非
- ^ 按位异或
- << 按位左移
- \>> 按位右移
- \>>> 无符号右移

#### 5、流程控制分支结构

##### 5.1、分支流程控制

if else、if else if、三元表达式、switch case

- 一元表达式：++num；二元表达式：3 + 5；三元表达式：true ?  true : false；表达式是有返回值；
- Swich case break，若是条件没有加break会出现case穿透问题；

#### 6、循环控制

##### 6.1、循环：for、while、do...while

- Countinue：结束当前循环
- break：退出整个循环
- do...while：先执行循环体，再进行判断，为真时继续循环体，为假时则终止当前循环;

#### 7、数组

数组：单一变量存储多个值

##### 7.1、数组创建、获取、遍历

```javascript
var arr1 = new Array();
var arr2 = [];
var arr3 = ['张三'， '里斯', 21312];
console.log(arr3[1]) // 获取中元素

// 通过for循环遍历数组
for(let i = 0; i < arr3.length; i++){
  console.log(arr3[i])
}

```

##### 7.2、数组函数

- length：获取数组的长度；
- toString()：将数组转化成字符串，以逗号分隔；
- join()：拼接数组元素，可以规定分隔符；
- pop()：删除数组最后一个元素，**返回“被弹出”的值**；
- push()：方法（在数组结尾处）向数组添加一个新的元素，**返回新数组的长度**；
- shift()：方法会删除首个数组元素，并把所有其他元素“位移”到更低的索引，**返回被“位移出”的字符串**。
- unshift()：（在开头）向数组添加新元素，并“反向位移”旧元素，**返回新数组的长度**；
- delete：使用 delete 会在数组留下未定义的空洞。请使用 pop() 或 shift() 取而代之；
- splice()：方法用于向数组添加新项，

```javascript
// 1、length 获取数组的长度
var arr = ['张三', '里斯', 'lilei', 'HanMeimei']
console.log(arr.length); // 4

// 2、toString
var arr = ['张三', '里斯', 'lilei', 'HanMeimei']
console.log(arr.toString()); // 张三,里斯,lilei,HanMeimei

// 3、join
var arr = ['张三', '里斯', 'lilei', 'HanMeimei']
console.log(arr.join('>')); // 张三>里斯>lilei>HanMeimei

// 4、pop
var arr = ['张三', '里斯', 'lilei', 'HanMeimei']
console.log(arr.pop(), arr); // HanMeimei，arr:['张三', '里斯', 'lilei']

// 5、push
var arr = ['张三', '里斯', 'lilei', 'HanMeimei']
console.log(arr.push('张三疯'), arr); // 4 arr:['张三', '里斯', 'lilei', 'HanMeimei', '张三疯']

// 6、shift
var arr = ['张三', '里斯', 'lilei', 'HanMeimei']
console.log(arr.shift(), arr); // 张三 arr:['里斯', 'lilei', 'HanMeimei']

// 7、unshift
var arr = ['张三', '里斯', 'lilei', 'HanMeimei']
console.log(arr.shift('Lily'), arr);// 4 arr:['张三', '里斯', 'lilei', 'HanMeimei']

//8、delete 删除元素
var arr = ['张三', '里斯', 'lilei', 'HanMeimei']
delete arr[0];
console.log(arr[0], arr);// undefined arr:[empty, '里斯', 'lilei', 'HanMeimei']

```

#### 8、函数

##### 8.1、函数的使用

> 函数参数（Function parameters）是在函数定义中所列的名称
>
> 函数参数（Function arguments）是当调用函数时由函数接收的真实的值。
>
> **注释：**与其他程序设计语言不同，ECMAScript 不会验证传递给函数的参数个数是否等于函数定义的参数个数。开发者定义的函数都可以接受任意个数的参数（根据 Netscape 的文档，最多可接受 255 个），而不会引发任何错误。任何遗漏的参数都会以 undefined 传递给函数，多余的函数将忽略。

- 形参：函数定义时接收的参数；
- 实参：函数调用时传递的参数；
- 形参和实参匹配问题：
  - 两者个数一致，正常执行；
  - 实参的个数大于形参的个数，会按照形参个数进行匹配；
  - 实参的个数小于形参的个数，未匹配的形参默认是undefined（**形参可以看成是不用声明的变量**）；
- **函数若是没有return，则返回的undefined;**
- 当 JavaScript 到达 return 语句，函数将停止执行。

##### 8.1、arguments的使用

在函数代码中，特殊对象 arguments，开发者无需明确指出参数名，就能访问它们

#### 9、作用域

- js的作用域（es6）之前：全局作用域、局部作用域
- 块级作用域：JS在ES6之前是没有

##### 9.2、变量的作用域

- 全局变量：
  - 在全局的作用域下的变量，全局都可以调用；
  - **如果在函数内部，没有声明的变量直接赋值，则该变量也是全局变量（不建议直接赋值）**
- 局部变量：
  - 局部作用域下声明的变量；
  - **函数的形参是局部变量**；
- 局部变量和全局变量效率对比：
  - 全局变量：只有在浏览器关闭时候才会销毁，比较占内存资源；
  - 局部变量：当程序执行完毕就会销毁，比较节约内存资源；

```javascript
// 全局作用域下声明的全局变量
var num = 10;
function fn(){
  var num1 = 20; // 在局部作用域下声明局部变量
  num2 = 20; // 注意:在函数内部，没有声明的变量直接赋值的变量属于全局变量
}
// console.log(num1); // 报错
// console,log(num2); // 20 全局变量
```

##### 9.3、作用域链

作用域链：内部函数访问外部函数的变量，采取的是链式查找的方式来决定取那个值，这种结构称为作用域链；

```javascript
var num = 10;
function fn(){
  var num = 20;
  function fun(){
    console.log(num); // 就近原则
  }
}
```

#### 10、JavaScript预解析

- 能够知道解析运行JS分哪些步骤？
- 能够说出变量提升的步骤和运行过程？
- 能够说数函数提升的步骤和运行过程？

##### 10.1、预解析

JavaScript解析器在运行JavaScript代码分为两步：**预解析和代码执行**

- 预解析：js引擎会把JS里面的var、function提升到当前的作用域的最前面;
- 代码执行：按照代码的书写的顺序从上往下执行；

##### 10.2、变量预解析和函数预解析

 预解析分为：变量预解析（变量提升）和函数预解析（函数提升）

- 变量提升：就是把所有的变量声明提升到当前作用域最前面，不提升赋值操作；
- 函数提升：就是把所有函数声明提升到单签作用域的最前面，不调用函数

```javascript
// 1、未声明变量
console.log(num); // 10_JS预解析.html:11 Uncaught ReferenceError: num is not defined

// 2、代码预解析，源代码:
console.log(num); // undefined 
var num = 10;
// 预解析，变量提升后，解析代码后如下：
var num;
console.log(num); // undefined
num = 10;


// 3、声明函数，函数将会提升
fn(); // 11
function fn(){
  console.log('11');
}

//4、函数表达式，源代码：
fun(); // 10_JS预解析.html:25 Uncaught TypeError: fun is not a function
var fun = function(){
  console.log('22')
}
// 预解析，变量提升，解析后代码如下：
var fun;
fun(); // 10_JS预解析.html:25 Uncaught TypeError: fun is not a function
fun = function(){
  console.log('22')
}
```

##### 10.3、预解析案例

Case1:

```javascript
var num = 10;
fun();
function fun(){
  console.log(num); // undefined
  var num = 20;
}

// 预解析如下：
var num;
function fun(){
  var num;
  console.log(num);
  num = 20;
}
num = 10;
fun();
```

Case2:

```javascript
var num = 10;
function fn2(){
  console.log(num); // undefined
  var num = 20;
  console.log(num); // 20
}
fn2();
// 预解析如下
var num;
function fn2(){
  var num;
  console.log(num); // undefined
  num = 20;
  console.log(num); // 20
}
num = 10;
fn2()
```

Case4:

```javascript
fn4();
console.log(c); // 9
console.log(b); // 9
console.log(a); // error a is undefined
function fn4(){
  var a = b = c = 9; 
  // 注意1：相当于 var a = 9, b = 9, c = 9; 
  // 注意2：b、c是函数内部直接赋值，局部变量未声明直接赋值将会变成全局变量
  // 区别于集体赋值：var a = 9, b = 9, c = 9;
  console.log(a); 9
  console.log(b); 9
  console.log(c); 9
}
// 预解析代码：
function fn4(){
  var a;
  a = b = c = 9;
  console.log(a); // 9
  console.log(b); // 9
  console.log(c); // 9
}
fn4();
console.log(c); // 9
console.log(b); // 9
console.log(a); // error a is undefined
```

#### 11、对象

- 为什么需要对象？
- 使用字面量创建对象？
- 使用构造函数创建对象；
- new的执行过程；
- 遍历对象；

##### 11.1、什么是对象

在Javascript中，对象是一组**无序**的相关**属性和方法**的集合，所有的事物都是对象，例如：字符串、数值、数组、函数；

##### 11.2、对象创建

1. 字面量创建：使用{}符号；
2. 利用new Object 创建对象（其实是实例化的过程）；
3. 使用构造函数创建；

###### 11.2.1、构造函数

**构造函数特点**：

- 一般首字母要大写，驼峰；
- 构造函数不需要return，就可以返回结果；
- 调用构造函数必须使用new；
- 构造函数实例（new ）后就创建一个对象；
- 属性和方法前面必须使用this；

```javascript
function 构造函数名(){
  this.属性 = 值；
  this.方法 = function(){
  }
}
new 构造函数名(); // 调用函数返回的是一个对象
```

##### 11.3、new关键字

new关键字执行过程：

1. new 构造函数可以在内存中创建一个空的对象；
2. this 就会指向创建的空对象；
3. 执行构造函数的里面的代码，给这个新对象添加属性和方法；
4. 返回这个新对象（所以构造函数里面不需要retrun）

##### 11.4、遍历对象属性

**for...in**遍历对象或是数组，一般是遍历数组

```javascript
for(let k in obj){ // k是属性名
  console.log(obj[k]); // 打印属性值
}
```

#### 12、Javascript内置对象

目标：

- 什么是内置对象?
- 如何查询API文档？
- Math对象常用方法
- Date对象常用方法
- Array对象常用的方法
- String对象常用的方法

##### 12.1、内置对象

- Javascript中的对象分为3种：自定义对象、内置对象、浏览器对象；
- 自定义对象、内置对象是JS基础内容，属于ECMAScript；浏览器对象属于JS独有；
- JavaScript提供内置对象主要有：Math、Date、Array、String 等，主要MDN提供；

##### 12.2、Math对象

- Math.PI  圆周率；

- Math.floor() 向下取整；

- Math.ceil() 向上取整；

- Math.round() 四舍五入取整；

  注意：-1.5 结果是-1；原因是：针对0.5是向上取整，所以-1；

- Math.abs() 绝对值；

- Math.max() 取最大值；

- Math.random() 获取随机小数

  - 取之范围是[0,1)，就是0、0-1之前

注意：函数内的隐式转换；

```javascript
// ceil 四舍五入 0.5向上取整
console.log(Math.ceil(1.5)); // 2
console.log(Math.ceil(-1.5)); // -1
console.log(Math.ceil(-1.6)); // -2

/**
 * 随机取整，得到一个两数之前的随机整数，包含两个数在内
 * @param {*} min
 * @param {*} max
 * @returns
 */
export function getRandomIntInclusive(min, max) {
  min = Math.ceil(min)
  max = Math.ceil(max)
  return Math.floor(Math.random() * (max - min + 1)) + min
}

```

##### 12.3、Date日期

-  是构造函数，使用时必须new Date();

```js
const weeks = ['日','一','二','三','四','五','六']

// 1、获取的当前的毫秒数，1970-01-01
var date = new Date()
// 1.1 通过valueof() getTime()
console.log('毫秒：' + date.valueOf());
console.log('毫秒：' + date.getTime());
// 1.2 常用写法
var mictime =  +new Date()
console.log('毫秒: ' + mictime);
// 1.3 H5新增
console.log(Date.now());

// 2、常用方法
var date = new Date()
// 2.1 获取年
console.log('Date '+ date);
console.log('Year ' + date.getFullYear());
console.log('Month ' + date.getMonth()); // 从 0（1月）到 11（12月)
console.log('Date ' + date.getDate());
console.log('Day ' + date.getDay()); // 返回0-6，0是周日
console.log('星期 ' + weeks[date.getDay()]);
console.log('Hour ' + date.getHours());
console.log('Minute ' + date.getMinutes());
console.log('Second ' + date.getSeconds());

// 3、赋值
console.log(new Date(2021, 10, 1));
```

##### 12.4、Array数组对象

###### 12.4.1、创建数组

- 直接声明：[]
- 通过实例Array对象，new Array()
  - 一个参数:numberlength，数组长度
  - 二个参数及以上，数组元素

```js
// 1、创建数组
var arr1 = [1,2,3];
console.log(arr1);
var arr1 = new Array(2); // 一个参数:numberlength，数组长度
console.log(arr1);
var arr1 = new Array(2,3); // 二个参数及以上，数组元素
console.log(arr1);
```

###### 12.4.2、检测数组

- instanceof：
- isArray()
  - 当检测Array实例时, `Array.isArray` 优于 `instanceof`,因为Array.isArray能检测`iframes`

```js
// 2、检测是否是数组
var arr2 = [1, 2, 3];
// 2.1 instanceof
console.log('[1, 2, 3] instanceof -->', arr2 instanceof Array);
// 2.2 isArray()，优先于instanceof，H5新增方法，IE9以上支持
console.log('[1, 2, 3] instanceof -->', Array.isArray(arr2));
```

###### 12.4.3、添加删除数组元素方法

- push(参数1.....)：数组末尾添加一个或多个元素，**改变原数组**，**返回新的长度**；
- prop()：删除最后一个元素，数组长度减一，**改变原数组**，**返回删除的元素**；
- unshift(参数1...)：向数组开头添加一个或个元素，**修改原数组，返回新的长度**；
- shift()：删除数组的一个元素，数组长度减1无参数，**修改原数组，返回一个元素的值**；
- reverse()：颠倒数组中元素的顺序，无参数，改变原数组，并返回新数组；
- sort()：对数组进行排序，改变原数组，返回新数组；
  - 排序规则：默认排序顺序是在将元素转换为字符串，然后比较它们的UTF-16代码单元值序列时构建的；
  - 参数compareFunction：可选，用来指定按某种顺序进行排列的函数，如果省略，元素按照转换为的字符串的各个字符的Unicode位；
- indexof()：获取元素的索引值，从首部查找；
  - 不存在则返回-1
  - 若是元素重复多次出现，只会返回元素第一次出现的索引值
- lastIndexof()：获取元素的索引值，是从尾部查找；
- toString()：是以逗号分隔
- join()：默认是逗号，可以传递分隔符

```js
console.log('%c4、数组排序 reverse() sort()-----------------------------', 'color: green');
// 4.1、反转数组
var arr = [5, 4, 3, 2, 1]
arr.reverse();
console.log(arr); // [1, 2, 3, 4, 5]

// 4.2、数组排序，
// 无参数，默认排序规则：元素会按照转换为的字符串的诸个字符的Unicode位点进行排序
// 如果 compareFunction(a, b) 小于 0 ，那么 a 会被排列到 b 之前；
// 如果 compareFunction(a, b) 等于 0 ， a 和 b 的相对位置不变。备注： ECMAScript 标准并不保证这一行为，而且也不是所有浏览器都会遵守（例如 Mozilla 在 2003 年之前的版本）；
// 如果 compareFunction(a, b) 大于 0 ， b 会被排列到 a 之前。
// compareFunction(a, b) 必须总是对相同的输入返回相同的比较结果，否则排序的结果将是不确定的。

// 4.2.1、无参数，相同位数
var arr = [1,8, 6, 2, 9]
arr.sort(); // 针对相同位数的数组，排序正常
console.log(arr); // [1, 2, 6, 8, 9]

// 4.2.2、无参数，不同位数
var arr = [1, 18, 6, 62, 611]
arr.sort(); // 会先比较第一位，依次进行
console.log(arr); // [1, 18, 6, 611, 62]

// 4.2.3 有参数
// 升序排列
var arr = [1, 18, 6, 62, 611]
arr.sort(function(curr, next){ 
  return (curr - next) 
}) // ES5
console.log(arr); // [1, 6, 18, 62, 611]

// 降序排列
var arr = [1, 18, 6, 62, 611]
arr.sort((curr, next) => ( next - curr) ) //ES6
console.log(arr); // [611, 62, 18, 6, 1]

console.log('%c5、数组索引值 indexof() lastIndexof()-----------------------------', 'color: green');
// 5、数组索引值
// 5.1、indexOf() 从数组首部查找
var arr = ['张', '王', '李', '赵', '李']
console.log(arr.indexOf('张')); // 0 
console.log(arr.indexOf('周')); // -1 不存在元素 -1
console.log(arr.indexOf('李')); // 2 返回元素第一次出现的索引值

// 5.2、lastIndexOf() 从数组的尾部查找
var arr = ['张', '王', '李', '赵', '李']
console.log(arr.lastIndexOf('张')); // 0 
console.log(arr.lastIndexOf('周')); // -1 不存在元素 -1
console.log(arr.lastIndexOf('李')); // 4 返回元素第一次出现的索引值
```

##### 12.5、String字符串

###### 12.5.1、基本包装类型

为了方便操作基本类型，JavaScript还提供三个特殊的引用类型：String、Number、Boolean。

> 基本包装类型：就是把简单的数据类型包装成复杂的数据类型，这样基本数据类型就有了属性和方法。

###### 12.5.2、字符串的不可变

指的是里面的值不可变，虽然看上去可以改变内容，但是其实是地址变化，内存中新开辟一个内存空间，所以大量拼接字符串时会有效率问题；

###### 12.5.3、根据字符返回位置

- indexOf()：返回字符第一次出现位置 从头部开始查找
- lastIndexOf 返回字符第一次出现位置 从尾部开始查找
- chatAt()：获取指定位置的字符；
- charCodeAt()：获得指定位置的ASCII码
- str[index]：H5支持的获取指定位置的字符， ie8+
- concat() 拼接字符串,相当于+
- substr(start, length) 从start位置开始，length:截取字符长度
- slice(start, end) 从start位置开始，截取到end位置（不包含end字符）
- substring(start, end) 从start位置开始，截取到end位置（不包含end字符）,end不接受负值
-  replace() 替换字符串，只替换第一个字符串
-  split() 字符串转数组

```JS
// 1、indexOf 返回字符第一次出现位置 从头部开始查找
var str = "hellow world!"
console.log(str.indexOf('l'));  // 2 从序号0开始

// 2、lastIndexOf 返回字符第一次出现位置 从尾部开始查找
var str = "hellow world!"
console.log(str.lastIndexOf('l'));  // 2 从序号0开始

// 3、charAt() 获取指定位置的字符
var str = "hellow world!"
console.log(str.charAt(8));  // o 

// 4、charCodeAt 获得指定位置的ASCII码
var str = "hellow world!"
console.log(str.charCodeAt(8));  // 111

// 5、str[index] H5支持的获取指定位置的字符， ie8+
var str = "hellow world!"
console.log(str[8]);  // o

// 6、concat() 拼接字符串,相当于+
var str = 'hellow world!'
console.log(str.concat('me', ' ', 'too!'));

// 6、substr(start, length) 从start位置开始，length:截取字符长度
var str = 'helloworld!'
console.log(str.substr(5, 5)); // world
var str = '你好，世界!'
console.log(str.substr(3, 2)); // 世界

// 7、slice(start, end) 从start位置开始，截取到end位置（不包含end字符）
var str = 'helloworld!'
console.log(str.slice(0, 1)); // h

// 8、substring(start, end) 从start位置开始，截取到end位置（不包含end字符）,end不接受负值
var str = 'helloworld!'
console.log(str.substring(0, 1)); // h
```

#### 13、数据类型讲解

##### 13.1、简单数据类型和复杂数据类型

- 简单类型：又叫做基本数据类型、值类型
  - 值类型：在存储变量中存储的是值本身，如：string、number、boolean、null、undefined
  - null：返回的是一个空对象
  - **存放在栈里面，直接开辟空间存放实际的值；**
- 复杂类型：又叫做引用类型
  - 复杂类型存储变量中存储的地址（引用），如：Object、Array、Date
  - **存放在堆里面；在栈里面存放索引地址，地址只想堆里面的数据**

##### 13.2、堆和栈

- 堆：由操作系统自动分配释放，存放函数的参数值、局部变量的值等，其操作方式类似数据结构中的栈；
- 堆：一般是程序员分配释放，若程序员不释放，程序结束时可能由OS回收，分配方式类似链表

##### 13.3、简单类型内存分配

##### 13.4、复杂类型内存分配

##### 13.5、简单类型传参

> ***函数的形参也可看成一个变量，当我们把一个值类型变量作为参数传给函数形参时，其实是把变量在栈空间的值复制了一份给形参，那么方法内部的对形参修改，都不会影响到外部变量。***

```JS
// 函数内部形参改变时，不会影响到外部实参的值；
// 原理：简单数据类型传参时，其实是把栈里面的值复制了一份给形参；
function fn(a){
  a++;
  console.log(a);  // 11
}
var x = 10;
fn(x);
console.log(x); // 10
```

##### 13.6、复杂类型传参

> ***函数的形参可以看成一个变量，当我们把引用类型变量传给形参时，其实是把变量在栈空间里保存的堆地址复制给形参，形参和实参其实保存的同一个堆地址，所以操作的是同一个对象***

```js
function Person(name){
	this.name = name
}
function fn(x){
	console.log('fn x before-->', x.name); // 刘德华
	x.name = '张学友';
	console.log('fn x after-->', x.name); // 张学友
}
var p = new Person('刘德华');
console.log('p->>', p.name); // 刘德华
fn(p);
console.log('p--->>>', p.name); // 张学友
```

### 第二部分：WEBAPI

#### 1、WebAPIs

##### 1.1、Web APIs和JS基础关联性

##### 1.2、API 和 WebAPI

#### 2、DOM

##### 2.1、DOM简介

文档对象模型（Document Object Model， 简称DOM），是W3C组织推荐的处理可扩展标记语言（HTML、XML）的标准编程接口

- 文档：一个页面就是一个文档，DOM中用***document***表示
- 元素：页面中的所有的标签都是元素，DOM中使用***element***表示
- 节点：网页中所有的内容都是节点（标签、属性、文本、注释等），DOM中使用***node***表示

##### 2.2、获取元素

- getElementById()
- getElementsByTagName()  返回的是获取过来的元素对象的集合，以伪数组形式存储

​	HTML5支持的，IE9+

- getElementsByClassName() 返回一个包含了所有指定类名的子元素的类数组对象
- querySelector() 方法返回文档中与指定选择器或选择器组匹配的第一个 HTMLElement对象
- querySelectorAll() 返回与指定的选择器组匹配的文档中的元素列表 (使用深度优先的先序遍历文档的节点)
- document.body 获取body元素
- document.documentElement  获取html

```JS
// 1、获取id对象
console.log(document.getElementById('frist'));

// 2、 获取页面中li
// getElementsByTagName 输出的伪数组  当tag元素不存在，返回是空的伪数组
console.log(document.getElementsByTagName('li'));

// 3、只获取ol的li元素
var ol = document.getElementById('ol')
// console.dir(ol)
console.log(ol.getElementsByTagName('li'));

// 4、 HTML5支持 ie9+
var el = document.getElementsByClassName('full-name')
console.log(el);
// 4.1、 querySelector 
var el = document.querySelector('.full-name') // 指定选择器或选择器组匹配的第一个 HTMLElement对象
console.log(el);
var el = document.querySelector('.full-name.sec')
console.log(el);
//  4.2、 querySelector 伪数组
var el = document.querySelectorAll('.full-name') // 指定选择器或选择器组匹配的所有 HTMLElement对象
console.log(el);

// 5、获取body元素
console.log(document.body);
// 6、获取html
console.log(document.documentElement);
```

##### 2.3、事件基础

事件三要素：事件源、事件类型、事件处理程序

##### 2.4、操作元素

- innerText()
- innerHtml()

##### 2.5、节点操作

###### 2.5.8、创建节点

- document.write()：是直接将内容写入页面的内容流，**但是文档流执行完毕，则它会导致页面全部重绘**；
- element.innerHTML：
  - 是将内容写入某个DOM节点，不会导致页面全部重绘；
  - innerHTML创建多个元素效率更高（**不要拼接字符串，采用数组形式拼接**），结构稍微复杂；
- document.createElement()：创建多个元素效率稍低，但是结构更为清晰；

> **总结：不同浏览器，innerHTML效率要比creatElement高**

#### 3、事件高级

##### 3.1、注册事件（绑定事件）

注册事件主要分为：传统方式、方法监听注册方式

传统注册方式：

- 利用关键字on开头的事件，例如：onclick
- 标签属性绑定，例如：<a onclick="aler('你好')"></a>
- 通过JS的DOM对象，例如：btn.onclick = function(){}
- 特点：注册事件的唯一性；
- 同一个元素同一个事件只能设置一个处理函数，最后注册的处理函数将会覆盖前面注册的处理函数

方法监听注册方式：

- W3C标准，推荐方式；
- addEventListener()是一个方法；
- IE9之前IE不支持此方法，可以使用attachEvent()代替；
- 同一个元素同一个事件，可以注册多个监听器；
- 按注册顺序依次执行；

##### 3.2、删除事件

##### 3.3、DOM事件流

概念：事件发生时会在元素的节点之间按照特定的顺序传播，这个传播过程即DOM事件流。

- 捕获阶段：document-->element html-->element body-->element div(目标阶段)，最早网景提出
- 冒泡阶段：element div-->element body-->element html-->document，最早IE提出

注意：

1. JS代码中只能执行捕获或冒泡其中一个阶段；
2. onclick、attachEvent只能得到冒泡阶段；
3. addEventListener(type, listener[,userCapture])第三个参数如果是true，表示在事件捕获阶段调用事件处理程序；如果是false（默认），表示事件冒泡阶段调用事件处理程序；
4. 有些事件是没有冒泡，比如：onblur、onfocus、onmouseenter、onmouseleave；
5. 。。。。

##### 3.4、事件对象

###### 3.4.1、target和this的区别

- e.target 返回的是触发事件的对象（元素）；
- this 返回的是绑定事件的对象（元素）；

```JS
var ul = document.querySelector('ul');
ul.addEventListener('click', function(e) {
  console.log(e.target); // li 即点击元素对象
  console.log(this); // ul 即为绑定事件的元素对象
})
```

###### 3.4.2、e.type事件类型

###### 3.4.3、e.preventDefault() 阻止默认事件

###### 3.4.4、阻止事件冒泡

```JS
var father = document.querySelector('.father');
father.addEventListener('click', function(e) {
	alert('father click--->')
})

var son = document.querySelector('.son');
son.addEventListener('click', function(e) {
  alert('son click--->')
  // 阻止，冒泡
  if (e && e.stopPropagation) {
    e.stopPropagation();
  } else {
    e.cancelBubble = true; // ie6~8 版本 
  }
})
```

###### 3.4.5、事件委托（事件委派）

原理：不用给每一个字节点单独设置事件监听器，而是事件监听器设置其父节点上，然后利用事件冒泡原理影响设置的每一个字节点；

其中子节点可以通过e.target获取当前点击的DOM对象；

###### 3.4.6、其他事件

##### 3.5、this指向

一般情况this的最终指向的是调用它的对象；

- 全局作用域或者普通的函数中this指向全局对象window(注意：定时器里面的this指向的window)
- 方法调用中谁调用this指向谁；
- 构造函数中this指向构造函数的实例；

#### 4、JS执行机制

##### 4.1、JS是单线程

##### 4.2、同步和异步

同步任务：同步任务都在主线程上执行，形成一个执行栈

异步任务：JS的异步是通过回调函数实现，异步任务相关回调函数添加到任务队列中（任务队列也称为消息队列），一般异步任务有三种类型，如下：

1. 普通事件，如click、resize等；
2. 资源加载，如load、error等；
3. 定时器，包括setInterval、setTimeout等；

##### 4.3、JS执行机制

1. 先执行执行栈中的同步任务。
2. 异步任务（回调函数）放入任务队列中。
3. 一旦执行栈中的所有同步任务执行完毕，系统会按照次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行。

![image-20211008232047428](/Users/wangyafei/Library/Application Support/typora-user-images/image-20211008232047428.png)

#### 5、navigator对象

#### 6、history对象

#### 5、JS网页交互

##### 5.1、元素的偏移量offset

注意：

- 获取的元素距离带**有定位父元素**的位置（即没有父元素或是父元素没有定位，会向上遍历到有定位）；
- 获取元素自身的大小
- 返回不带单位

###### offset相关属性

| offset属性   | 作用                                                         |
| ------------ | ------------------------------------------------------------ |
| offsetParent | 返回作为该元素带有定位的父级元素 如果父级都没有定位则返回body |
| offsetTop    | 返回元素相对带有定位父元素上方的偏移                         |
| offsetLeft   | 返回元素相对带有定位父元素左边框的偏移                       |
| offsetWidth  | 返回自身padding、边框、内容区的宽度                          |
| offsetHeight | 返回自身padding、边框、内容区的高度                          |

###### offset与style区别

| offset                                | Style                                  |
| ------------------------------------- | -------------------------------------- |
| 可以获得任意样式的样式值              | 只能获得**行内样式**中的样式值         |
| 获得数值没有单位                      | 获取的是有单位的字符串                 |
| offsetWidth包含padding+border+width   | 获取的不包含padding、border的值        |
| offsetWidth等属性是只读属性，不能赋值 | width是可读写 属性，可以获取也可以赋值 |

