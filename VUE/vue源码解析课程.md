课程地址：https://www.bilibili.com/video/BV1LE411e7HE?from=search&seid=16987348311857809535&spm_id_from=333.337.0.0

#### 1、数据驱动

##### 1.1、vue 与模版

```JS
// 仿模版引擎替换
var reg = /\{\{(.+?)\}\}/gi
var template = '<p>姓名：{{name}}，年龄：{{age}}</p>';
var data = {
  name: '张三',
  age: 12
}
var str_p = template.replace(reg, function(_, g) {
  console.log(_, g);
  return data[g]
})
console.log(str_p);
```

> 语法规则：内部数据使用下划线开头，只读数据使用$开头

##### 1.2、函数柯里化

（[阮一峰函数式编程-函数柯里化](https://www.bookstack.cn/read/es6-3rd/spilt.1.docs-fp.md)）

```JS
function add(a) {
  return function(b) {
    return a + b;
  }
}
const f = add(1);
res = f(1) // 2
console.log(res);

//或者采用箭头函数写法
const add2 = x => y => x + y;
const f2 = add2(1);
res = f2(1) // 2
console.log(res);
```

##### 1.4 虚拟DOM



#### 2、函数柯里化和函数模型

波兰式

#### 3、响应式原理

#### 其他

1. 函数调用模式
2. 函数 柯里化
3. 响应式原理：object.defineProperty