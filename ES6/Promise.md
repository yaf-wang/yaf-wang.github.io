## 异步操作有哪些？

- ajax异步请求
- 事件，如click、resize等
- 定时器，setInterval、setTimeout
- 资源加载，如load、error等；

##  什么是Promise

Promise是异步编程的一种解决方案，传统解决方案——回调函数和事件。从语法上Promise是个对象，存放着异步操作；

## 为什么使用Promise

支持链式调用，可以解决回调地狱问题

### Promise特点

- 对象的状态不受外界影响。有三种状态：
  - Pending（进行中）
  - Fulfilled（已成功）
  - Rejected（已失败）
- 一旦状态改变就不会再变。

### promise缺点

- 无法取消Promise；
- 不设置回调函数，Promise内部抛出错误不会反应到外部；
- 当是Pending状态时，无法得知目前进行到哪一个阶段（如：开始、即将完成）；

> 如果某些事件不断反复发生，一般来说，使用[***Stream模式***](http://nodejs.cn/api/stream.html)是比Promise更合适





## 基础用法

Promise是一个构造函数，传入“处理器函数”作为参数，可以是箭头函数，也可以是匿名函数；处理函数接受两个函数——resolve和reject——作为其参数。

创建一个Promise实例

```javascript
const promise = new Promise(function(resolve, reject){
  // ...some code
  if(true){
    resolve(res); // 状态从pending到Resolved，异步操作成功调用
  }else{
    reject(error); // 状态从pending到rejected，异步操作失败调用
  }
})
```

Promise实例创建成功后，可以用then方法指定Resolved和Rejected状态回调；

```javascript
// then方法接受两个回调函数作为参数
promise.then(function(res){
  // ...success
},function(error){
  // ...error
})
```

```javascript
// p2返回的是另一个Promise，导致p2的壮体啊无效，有p1决定p2的状态 
const p1 = new Promise(function (resolve, reject) {
  setTimeout(()=>{
    reject(new Error('P1 rejected'))
  }, 3000)
})

const p2 = new Promise(function(resolve, reject){
  setTimeout(()=>{
    resolve(p1)
  }, 1000)
})

p2.then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```

### Promise.prototype.then()

作用是：为Promise的状态成功时执行回调函数

then方法第一个参数是Resolve状态的回调函数，第二个参数（可选）是Rejected状态的回调函数。一般是建议不使用then的第二个参数，而是使用catch方法

- then可以采用链式写法

```javascript
// p1、p2 返回都是一个Promise对象
p1().then(function(){
  // 返回一个新的Promise实例
  return p2;
}),then(function(){
  // ...
})

// 采用箭头函数，更简洁
p1().then(
	res => p2()
).then(
	res => console.log('res'),
  err => conslot.log('err')
)
```

### Promise.prototype.catch()

- 如果promise状态已经变成Resolved，再抛异常错误是无效的

```javascript
const promise = function(){
  return new Promise(function(resolve, reject){
    resolve('OK')
    throw new Error('error')
  })
} 

promise().then(()=>{
  console.log('promise then');
})

// OK
```

- 没有指定catch错误处理的回调函数，Promise对象抛出错误不会传递到外层代码

```javascript
const promise = function(){
	return new Promise(function(resolve, reject){
		resolve(x + 2); // 这里会报错，x是没有声明
	})
} 

promise().then(()=>{
	console.log('promise then');
})
// 浏览器会打印错误，但不会终止脚本执行
```

- Promise指定在下一轮“事件循环”再抛出错误，Promise已经结束，所以这个错误是在Promise函数体外抛出，会冒泡到最外层，成了未捕获的错误

```javascript
const promise = function(){
  return new Promise(function(resolve, reject){
    resolve('OK')
    setTimeout(() => {
      throw new Error('ddddd')
    }, 1000);
  })
} 

promise().then(()=>{
  console.log('promise then');
})

// Node中 可用于了解发生错误的环境信息
process.on('unhandleRejection', function(err, p){
	console.log(err.stack)
})

```

- catch方法后接着运行then方法，如果没有错误，则会跳过catch方法；要是then里面报错，就与前面catch无关了

```javascript
Promise.resolve().catch(err => {
  console.log('err-->');
}).then(res =>{
  console.log('res-->');
  x + 2; // 这里错误，与catch无关
})
```

### Promise.all()

用于将多个Promise实例包装成一个新的Promise实例。

```JavaScript
const promises = Promise.all([p1, p2, p3, ...])
```

- Promise.all接受一个数组参数，都是Promise对象实例；若不是会先调用Promise.resolve方法，将参数转为Promise实例，再进一步处理。
- 只有全部的Promise实例的状态都变成fulfilled，外层的Promise状态才变成Fulfilled，此时返回值是一个数组；
- 只要一个Promise实例的状态变成Rejected，外层的Promise状态就变成Rejected，此时第一个被Rejected的实例返回值会传递到回调函数中；

> 如果作为参数的Promise实例自身定义了catch方法，那么它的Rejected时并不会触发Promise.all()的catch方法；

```javascript
const p1 = new Promise(function (resolve, reject) {
  resolve('p1 resolve')
})
.then(res => res)
.catch(err => err)

const p2 = new Promise(function (resolve, reject) {
  throw new Error('P2 Error')
})
.then(res => res)
.catch(err => {
  return err
})

Promise.all([p1, p2])
  .then(res => console.log(res))
  .catch(err => console.log(err))
// ['p1 resolve', Error: P2 Error]
```

### Promise.race()

多个Promise实例包装成一个新的Promise实例。[race: 赛跑]

```javascript
const promises = Promise.race([p1, p2, p3, ...])
```

- 接受一个数组参数，都是Promise对象实例；若不是会先调用Promise.resolve方法，将参数转为Promise实例，再进一步处理。
- 只要有一个实例率先改变状态，Promise的状态就跟着改变。

```javascript
const p1 = new Promise(function(resolve, reject){
  setTimeout(()=>{
    resolve('接口请求，返回结果()');
  }, 3000)
})

const p2 = new Promise(function(resolve, reject){
  setTimeout(()=>{
    reject('2s 操作超时了。。。');
  }, 2000)
})

Promise.race([p1, p2])
  .then(res => console.log(res))
  .catch(err => console.log(err))
```

### Promise.resolve()

可以将现有对象转为Promise对象；

```javascript
Promise.resolve('foo');
// 等价于
new Promise(resolve => resolve('foo'))
```

Promise.resolve参数分为四种情况：

- 参数是一个Promise实例

  promise.resolve不做任何处理，原封不动返回这个实例；

- 参数是一个thenable对象

  promise.resolve方法将这个对象转化成promise对象，然后立即执行thenable对象的then方法

  ```javascript
  const thenable = {
    then: function(resolve, reject){
      resolve('thenbale');
    }
  };
  
  const p = Promise.resolve(thenable);
  p.then(res => console.log(res))
  ```

- 参数不是具有then方法的对象或根本不是对象

  参数是一个原始值，或者是一个不具有then方法的对象，那么Promise.resolve方法返回一个新的Promise对象，状态为Resolved

- 不带有任何参数

  Promise.resolve不带任何参数，直接返回一个Resolved状态的Promise对象

  > 注意：立即resolve的Promise对象是在本轮“事件循环”结束时，而不是在下一轮“事件循环”开始时。

### Promise.reject()

返回一个新的Promise实例，状态为Rejected；

```javascript
const p = Promise.reject('出错了');
// 等价于
const p = new Promise((resolve, reject) => reject('出错了'))
```

> 注意：Promise.reject()方法的参数会原封不动地作为reject的理由变成后续方法的参数，这点与Promise.resolve方法不一致

```javascript
const thenable = {
  then: function(resolve, reject){
    reject('thenbale');
  }
};

const p = Promise.reject(thenable);
p.catch(err => console.log(err === thenable)); // true
```





## 附加方法

**Promise.prototype.done()**

无论Promise对象的回调链以then方法还是catch方法结尾，只要最后一个方法抛出错误，都有可能无法捕获到（因为Promise内部的错误不会冒泡到全局）。为此，done方法总是处于回调链尾端，保证抛出任何可能出现的错误。

**Promise.prototype.finally()**

指定不管状态如何都会执行的操作，与done方法区别，他接受到一个普通回调函数作为参数，该函数不管怎样都必须执行。

**Promise.any()**





## Promise关键问题

### 1、如何修改对象的状态？

- Resolve(value)方法可以让pending状态变成fulfilled；
- Reject(value) 方法可以让pending状态变成rejected；
- 抛出异常可以让pending状态变成rejected；

### 2、Promise指定多个成功/失败回调函数？

​	当Promise指定多个成功、失败的回调函数，当状态改变为对应状态都会被执行

```javascript
const p = new Promise((resolve, reject)=>{
	resolve('ok');
});
p.then(res => console.log('第一个then'));
p.then(res => console.log('第二个then'));
// 第一个then
// 第二个then
```

### 3、Promise的改变状态和指定回调函数谁先谁后？

### 4、Promise.then返回的新的promise的结果状态由什么决定？

​	由then()指定的回调函数执行的结果决定。

> - 如果结果抛出异常，新的promise变成rejected，reason为抛出的异常；
>
> - 如果返回的是非promise的任意值，新promise变成resolved，value为返回的值；
> - 如果返回的是另外一个新的promise，此promise的结果就会成为新promise的结果

### 5、promise如何串连多个操作任务？

- promise的then()返回一个新的promise，可以看成then的链式调用；
- 通过then的链式调用串连接多个同步/异步任务

### 6、promise异常穿透？

- 当使用Promise的then链式调用时，可以在最后指定失败的回调；
- 前面任何操作出了异常，都会传到最后失败的回调中处理；

```javascript
 const p = new Promise((resolve, reject) => {
   // 1、 先返回resolve/rejected，catch并未捕获到异常
   // resolve('ddd')
   // throw 'error'

   // 2、先返回resolve/reject，异步函数抛出异常， 会把错误作用到全局
   // resolve();
   // setTimeout(() => {
   //   throw 'error'
   // }, 1000);

   // 3、先抛出异常，再返回resolve/reject/其他代码，catch会捕获到异常，下面的代码不会执行
   // throw 'error';
   // resolve('ddd');
 })

 p.then(res => {
   console.log('res');
 }).then(res2 => {
   console.log('res2')
 }).catch((err) => {
   console.log(err)
 })
```

### 7、如何中断Promise链？

当使用Promise的then的链式调用时，在中间中断，不再调用后面的回调函数？

可以通过：回调函数中返回一个pending状态的promise对象

```javascript
const p = new Promise((resolve, reject) => {
  resolve('ddd')
})

const promise = p.then(res => {
  console.log('res')
  return new Promise(()=>{}) // pending 状态的promise
}).then(res2 => {
  console.log('res2')
}).catch((err) => {
  console.log(err)
})
```





## Promise的事件循环

### 1、Promise新建后立即执行

### 2、Promise与etTimeout执行先后





## async和await

### 实现原理

await底层是基于Promise和事件循环机制实现的。

### 常见使用陷阱

1、多个awit并执行

```javascript
async function f(){
  const res1 = await fetch('...url...');
  const res2 = await fetch('...url2..')
}
f()
```

以上代码，虽然不存在逻辑错误，但会打破fetch操作的并行，因为这样会等到第一个任务执行完成之后才会开始执行第二任务。这里更高效的做法是将所有的Promise用 Promise.all 组合起来，如下：

```javascript
async function f(){
  const [res1, res2] = await Promise.all([
    fetch('...url...'),
    fetch('...url2..')
  ])
}
f()
```

2、如果需要在循环中执行异步操作，是不能直接调用forEach、map这类方法的；如果我们需要等待循环中的异步操作都一一完成之后才继续执行， 可以选用传统的for循环，

```javascript
// 错误写法  这里的foreach会立刻返回，并不会等到所有异步操作完成
async funcion f(){
  [1, 2, 3].forEach(async (i) => {await fetch('...') } )
}

 // 并不会等到foreach遍历完成执行done
async function f () {
  const arr = [1, 2, 3]
  arr.forEach(async item => {
    console.log(Date.now(), await p);
  });
  console.log(Date.now(), 'done')
}
f(); // done     1 'ok'....

// 可以使用传统for
async function f () {
  const arr = [1, 2, 3]
  for(let i of arr){
    const res = await p
    console.log(Date.now(), res);
  }
  console.log('done')
}
f(); // 1 'ok'....     done 

// for await 可以循环中的所有操作并发执行
async function f () {
  const promises = [p, p, p]
  for await(let res of promises) {
    console.log(Date.now(), res);
  }
  console.log(Date.now(), 'done')
}
f(); // 1 'ok'....     done 
```

3、我们不能在全局或者普通函数中直接使用await关键字，await只在异步函数async中有效；

## 

## 常见 Promise 面试题

1. 了解 Promise 吗？

   解决异步编程的一种方案，避免回调地狱问题，

2. Promise 解决的痛点是什么？

   异步编程，回调地狱问题，可以通过链式写法，代码易懂、扁平

3. Promise 解决的痛点还有其他方法可以解决吗？如果有，请列举。

   - 之前把各部拆分成单个函数

   - 通过 **Generator** 函数暂停执行的效果方式
   - async\await

4. Promise 如何使用？

   promise主要由三种状态：pending、fulfilled、rejected

   常用方法：

   - resolve：Promise.resolve(value)方法返回一个以给定值解析后的Promise 对象。如果这个值是一个 promise ，那么将返回这个 promise ；如果这个值是thenable（即带有"then" 方法），返回的promise会“跟随”这个thenable的对象，采用它的最终状态；否则返回的promise将以此值完成。此函数将类promise对象的多层嵌套展平。
   - reject：返回一个带有拒绝原因的`Promise`对象。
   - then：当Promise 被 fulfille 时，被调用方法
   - catch：当Promise 被rejected时,被调用的一个Function
   - finally: 当Promise结果无论成功/失败，都会执行的方法；
   - all：全部返回
   - race：优先返回

5. Promise 在事件循环中的执行过程是怎样的？

   请参考“Promise的事件循环”

6. Promise 的业界实现都有哪些？bluebird、Q、ES6-Promise

7. 能不能手写一个 Promise 的 polyfill。

## 手写Promise



