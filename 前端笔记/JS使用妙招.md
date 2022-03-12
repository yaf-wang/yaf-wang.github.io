作者：前端pony
链接：https://zhuanlan.zhihu.com/p/402817988
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



### 1. if多条件判断

```text
// 冗余
if (x === 'abc' || x === 'def' || x === 'ghi' || x ==='jkl') {}

// 简洁
if (['abc', 'def', 'ghi', 'jkl'].includes(x)) {}
```

**2. if...else...**

```text
// 冗余
let test: boolean;
if (x > 100) {
    test = true;
} else {
    test = false;
}

// 简洁
let test = x > 10;
```

**3. Null, Undefined, 空值检查**

```text
// 冗余
if (first !== null || first !== undefined || first !== '') {
    let second = first;
}

// 简洁
let second = first || '';
```

\4. foreach循环

```text
// 冗余
for (var i = 0; i < testData.length; i++)
    
// 简洁
for (let i in testData)
// 或
for (let i of testData)
```

\5. 函数条件调用

```text
// 冗余
function test1() {
  console.log('test1');
};
function test2() {
  console.log('test2');
};
var test3 = 1;
if (test3 == 1) {
  test1();
} else {
  test2();
}

// 简单
(test3 === 1? test1:test2)();
```

\6. switch条件

```text
// 冗余
switch (data) {
  case 1:
    test1();
  break;

  case 2:
    test2();
  break;

  case 3:
    test();
  break;
  // so on...
}

// 简洁
var data = {
  1: test1,
  2: test2,
  3: test
};

data[anything] && data[anything]();
```

\7. 多行字符串

```text
// 冗余
const data = 'abc abc abc abc abc abc\n\t'
    + 'test test,test test test test\n\t'

// 简洁
const data = `abc abc abc abc abc abc
         test test,test test test test`
```

\8. 隐式返回

```text
// 冗余
function getArea(diameter) {
  return Math.PI * diameter
}

// 简洁
getArea = diameter => (
  Math.PI * diameter;
)
```

\9. 重复字符串多次

```text
// 冗余
let test = ''; 
for(let i = 0; i < 5; i ++) { 
  test += 'test '; 
} 

// 简洁
'test '.repeat(5);
```

\10. 幂乘

```text
// 冗余
Math.pow(2,3);

// 简洁而
2**3 // 8
```