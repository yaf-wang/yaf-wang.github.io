## JavaScript中this的指向

### 普通函数this指向问题

```javascript
const UserName = '李嗣源'; // const、let不会挂在到window

var username = '李元霸';
function Person(){
    this.username = '章三';
    this.sing = function(){
        console.log(this.username); // 取决于运行时的上下文
    }
}
const p = new Person();
p.sing(); // 章三

const sing = p.sing;
sing(); // 李元霸
```

### 箭头函数this指向问题

