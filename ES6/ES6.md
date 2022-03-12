

BaBel转码器：

babeljs.io

##### let和const

- 变量不能重复声明；
- 不存在变量提升；
- 块级作用域；
- const声明一个常量，它指向的**内存地址**；对于简单的类型的数据（数值、字符串、布尔值）而言，值就保存在变量指向的内存地址；但是复合类型的数据（对象、数组）而言，变量指向的内存地址只是个指针。若要想冻结对象（**Object.freeze**）

```javascript
var temp = new Date()

// 存在变量提升
function f() {
  console.log(temp); // undefined
  if (false) {
    var temp = 'hello world'
    };
}

// IIFE写法，通过匿名函数方式
// ES5：中声明只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明
// function f() {
//   console.log(temp); // Date
//   if (false) {
//     (function() {
//       var temp = 'hello world'
//       console.log('false--->', temp)
//     }());
//   }
// }
f();
```

##### 2.3.3 声明变量的方法

ES5：var、function

ES6：var、function、let、const、import、class

##### 2.4 顶级对象属性

浏览器环境中widow对象，Node环境中global对象；在ES5中，顶层对象的属性与全局变量是等价的。ES6中，va r、function命令声明的全局变量依旧是顶层对象的属性，let、const、class命令声明的全局变量不属于顶层对象的属性；

```javascript
// 全局变量a由var命令声明变量为顶层对象的属性
var a = 1 
console.log(window.a);  // 1
// 全局变量b由let命令声明，所以不是顶层对象的属性
let b = 1
console.log(window.b); // undefined
```

### Promise

可以查看文档，*./Promise.md*

