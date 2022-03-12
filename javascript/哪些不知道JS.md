

### 对象的键是字符串，若是对象（数组）为键，会转化成[object Object]（[object Array]）, Object.prototype.toString.apply(obj)

```js
var obj1 = { name: '112'}
var obj2 = { name: '222'}
var Obj = {
  [obj1]: '1111',
  [obj2]: '2222',
}
console.log(Obj); // 只有一个键值对 {[object Object]: '2222'}
```

### this指向问题

[JavaScript 的 this 原理 阮一峰](https://www.ruanyifeng.com/blog/2018/06/javascript-this.html)

一般解释：this指向的是函数运行时所在的环境，

- 一般函数，this指向全局对象window；
- 在严格模式下‘use strict’,为undefined；
- 对象方法里调用，this指向调用该方法的对象；
- 构造函数里的this，指向创建出来的实例；
- 箭头函数没有this，内部this指向外层的作用域；
- 对象方法赋值问题，赋值的是函数地址，this跟随函数运行环境变化；

```js
// 1、在一般模式，函数this指向window
function foo1(){
  console.log(this);
}
foo1() // window

// 2、严格模式下，this指向是undifineds
function foo2(){
  "use strict"
  console.log(this);
}
foo2(); // undefined

// 3、对象方法调用 this指向当前对象
const foo = {
  name: '李云',
  sing: function(){
    console.log(this.name + '在唱歌。。。');
  }
}
foo.sing()

// 4、构造函数this指向实例对象
function Stu(name){
  this.name = name,
    this.study = function(){
    console.log(this.name + '学习中。。。');
  }
}
const stu1 = new Stu('韩梅梅')
const stu2 = new Stu('李雷雷')
stu1.study()
stu2.study()
```

```js
var name = "window.name: 李四"
var obj = {
  name:  'obj.name: 章三',
  say: function(){
    console.log(this.name + '在说话');
  },
  run: ()=>{
    console.log(this.name + '在跑步');
  },
  sing: function(){
    var song = () => {
      console.log(this.name + '在唱歌');
    }
    return song
  }
}

obj.say() // obj.name: 章三在说话  this指向obj
b = obj.say // b相当于获取say函数内存地址
b() // window.name: 李四在说话   this指向window
b.apply(obj) // obj.name: 章三在说话   apply改变this指向

var obj2 = {
  name: 'obj2.name: 王五'
}
obj2.say = obj.say;
obj2.say(); // obj2.name: 王五在说话

// 箭头函数 this指向是外层作用域， 箭头函数和run是同级，外层window
run = obj.run;
run() // window.name: 李四在跑步
obj.run()  // window.name: 李四在跑步

// 箭头函数包含在sing内部，
var song = obj.sing()  // obj
song() // obj.name: 章三在唱歌
obj.sing()() // 同上 obj.name: 章三在唱歌 

var sing1 = obj.sing // 相当于函数地址赋值给sing1
sing1()() // window.name: 李四在唱歌
```



### apply、call、bind

区别：

- apply、call会立即执行，bind不会立即执行；
- apply传入的是参数数组，call传入的是参数列表；

注意：apply、call参数**thisArg**在**非严格模式**下，指定为null或undifined会自动替为指向全局对象，原始值被包装；