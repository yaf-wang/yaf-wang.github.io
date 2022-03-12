## ES6

### 变量声明let和const

let声明一个变量：

- 变量不能重复声明
- 块级作用域
- 不存在变量提升
- 不影响作用域链

const声明一个常量

- 声明时需要赋初始值
- 常量不能修改
- 块级作用域
- 数组和对象的元素可以修改，不会报错；

### 解构赋值

- 交换变量；
- 从函数返回多个值；
- 函数参数定义；
- 抽取JSON数据；
- 函数参数的默认值， 避免函数体内进行默认值判断；
- 遍历MAP结构，map结构原声支持Iterator接口
- 输入模块指定方法

```javascript
// 1、 交换变量
let x1 = 1;
let y1 = 2;
[x1, y1] = [y1, x1];
console.log(x1, y1);

// 2、从函数返回多个值
function fn() {
  return [1, 2, 3];
}
const [x2, y2, z2] = fn()
console.log(x2, y2, z2);

// 3、函数参数定义
function fn3([x3, y3, z3]) {
  console.log(x3, y3, z3);
}
fn3(['li', 'sd', 'zad'])

// 4、抽取JSON数据
let res = {
  code: 200,
  message: 'SUCCESS',
  data: {}
}
const {
  code,
  message,
  data
} = res
console.log(code, message, data);

// 5、函数参数的默认值， 避免函数体内进行默认值判断
var ajax = function(url, {
  async = true,
  beforeSend = function() {},
  cache = true,
  complete = function() {},
  crossDomain = false,
  global = true,
  // ... more config
} = {}) {
  console.log(async, beforeSend);
};
ajax('baidu.com', {
  async: false,
  beforeSend: function() {
    console.log('beforeSend----->');
  }
})

// 6、遍历MAP结构，map结构原声支持Iterator接口
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');
for (let [key, value] of map) {
  console.log(key, '--->', value);
}
// 获取键名
for (let [key] of map) {
  console.log('key--->', key);
}
// 获取键值
for (let [, value] of map) {
  console.log('value--->', value);
}

// 7 输入模块指定方法
// const { SourceMapConsumer,SourceNode } = require("source-map");
```



### 模版字符串

使用反印号

- 内容中直接使用换行符；
- 进行变量拼接；

```javascript
// 内容直接换行，
const temp = `<ul>
								<li></li>
							</ul>`

// 拼接变量
const zhangsan = '章三'
const str = `hello, ${zhangsan}`
```

### 简化对象写法

```javascript
const name = '里斯';
const age = 18;
const person = {
  name, 
  age,
  sing: function(){
    console.log('唱歌')
  },
  say(){ // 简化方法命令
    console.log('说话')
  }
}
```

### 箭头函数

- this是静态，始终指向声明时所在的作用域下的this值；
- 不能作为构造函数实例对象；
- 不能使用arguments变量；
- 箭头函数简写
  - 省略小括号，当行参有且只有一个时候；
  - 省略花括号，当代码体只有一条语句时候，return必须省略，语句执行的结果就是返回值；
- 应用场景
  - 适合与this无关的回调，比如：定时器、数组方法回调（fitler、map）;
  - 不适合与this有关的回调，比如：事件回调、对象方法；

### 函数参数的初始值

- 形参初始值
- 与解构赋值结合

### rest参数

- 在ES5中，使用arguments获取传递给所有（非剪头）函数参数的类数组对象，是局部变量；
- 在ES6中，获取（箭头）函数的多余参数返回时数值，rest参数必须放到所有的参数的最后； 

```javascript
// ES5中获取函数的参数
function foo(a, b, c){
	console.log(arguments);
  console.log(type arguments); // object
  
  // 转换成真实数组
  // Array.prototype.slice.call(arguments);
  // Array.from(arguments); // from是浅拷贝
  // const arr = [...arguments]; // es6的扩展运算
}
foo(1,2,3,4,5,6);

//ES6 rest参数
function foo(a, b, c, ...args){
    console.log(a);
    console.log(b);
    console.log(c);
    console.log(args);
}
foo(1,2,3,4,5,6);
```

### 扩展运算符

- 数组的合并；
- 数组克隆（浅拷贝）；
- 伪数组转为真实数组；

### symbol数据类型

### 迭代器

主要作用：

- 为各种数据提供一个统一、简单的访问接口；
- 使得数据结构成员能够按照某种次序排列；
- 为for..of消费；

主要遍历过程：

1. 创建一个指针对象，指向当前数据结构的起始位置，也就是说，遍历器对象本质就是一个指针对象；
2. 第一次调用指针对象的next方法，可以将指针指向数据结构的一个成员；
3. 第二次调用指针对象的next方法，指针指向数据结构的第二个成员；
4. 不断调用对象的next方法，直到指向对象结束位置。

```javascript
 const obj = {
   name: '张三',
   age: '18',
   hobby: [
     '打球',
     '唱歌',
     '跳舞',
     'rap'
   ]
 };


// 创建一个迭代器方法
const iterator = function (){
  let index = 0
  const self = this
  return {
    name: '迭代器',
    next(){
      if (index < self.hobby.length) {
        return {value: self.hobby[index++], done: false }
      }
      return { done: true }
    }
  }
}
obj[Symbol.iterator] = iterator
for(const v of obj){
  console.log(v)
}
```

### 生成器

### Promise

Promise是ES6引入的异步编程的新的解决方案。用来解决异步编程的回调地狱的问题；Promise是一个构造函数。三种状态：

- pending：进行中
- fulfilled：成功
- rejected：失败

Promise具有以下特点：

- 不受外界影响；
- 一旦执行无法取消；
- 一旦状态改变就不会改变；
- 支持链式调用；

内置函数：

- Promise.prototype.then，接受两个参数，第一个参数resolve成功回调函数，第二参数reject失败回调函数（可选）；
- Promise.prototype.catch，执行发生错误的回调函数；
- Promise.resolve()，返回一个fulfilled状态的Promise对象；可以将对象转为Promise对象（字符串、thenable对象，无参数）
- Promise.reject()，返回一个rejected状态的Promise对象
- Promise.all()，执行多个promise，全部成功返回成功，只要有一个失败就是失败；
- Promise.race()，执行多个promise，返回率先第一个执行完成的结果；
- Promise.any()，执行多个promise，只要有一个成功就成功；

