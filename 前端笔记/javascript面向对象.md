函数参数默认，call,

作者：clearlove
链接：https://zhuanlan.zhihu.com/p/422433172
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



**2、原型继承**

继承是指类型和类型之间的继承；通过原型对象将属性继承给子代。即子代的原型对象等于新的父代函数。需要注意的是需要重新修改子代的原型的constructor的指向为子代，否则，会指向父代。

**3、继承--函数的call方法**

call本身是一种函数的执行方法。指向函数时有两种作用：1、更改函数this的指向；2、调用函数执行内部代码。fun.call(,)。传入的第一个参数是this的指向,第二个以及以后的参数为传入的实参。

**4、原型对象方法的继承**

子类型的原型对象里面的方法需要继承父代的原型对象方法。

方法一：通过for in循环拷贝。方法二：通过原型继承，直接让子代的原型对象等于不传参数的新父代。

这两个方法都要修改子代的constructor为子代。

**5、组合继承**

属性通过构造函数继承，方法通过原型继承。通过函数的call方法修改父代函数的this指向为子代的this继承属性，再通过原型对象等于新构造的父代函数继承原型方法。

**6、函数定义方式**

函数声明：function fun (){};

函数表达式：var fun=function(){};

if 语句中如果有函数，高版本浏览器会将其进行变量声明提升，低版本进行函数声明提升。解决方法是提前定义一个变量，在if语句中让变量等于函数。

**7、函数的调用和this**

调用普通函数：给函数或者变量名后面加括号调用fun()。this默认指向window。

调用构造函数：使用new，var f1=new fun();this默认指向将来创建的实例对象。

调用对象中的方法函数：函数为一个对象中的一个方法，o.sayHi();this默认指向对象o。

事件函数：不需要加符号执行，只要事件触发，自动执行函数。this默认指向触发事件的事件源。

定时器和延时器中的函数：也不需要加符号，满足条件自动触发。this默认指向window。

在定义时this指向是默认，具体参考调用函数的方法。

函数内部在调用时。this有自己默认的指向：call、apply。bind

call方法：fun.call(,)。第一个参数是this的指向,第二个以及以后的参数为传入的实参。

apply（应用）方法：fun.apply(o,[4,5]);第一个参数为this的指向，第二个参数为函数参数组成的数组。

bind方法：不能执行函数，但是可以传参。var fn=fun.bind(,)。第一个参数是this的指向,第二个以及以后的参数为传入的实参。返回一个新的函数，还需要进行一次函数的调用。

#### 箭头函数和function的区别

1. 写法不一样

   ```js
   //function
   function fn(a, b){
   	return a + b;
   }
   //arrow function
   var foo = (a, b)=>{ return a + b };
   ```

2. this的指向不一样
   使用function定义的函数，this的指向**随着调用环境的变化而变化**的，而箭头函数中的**this指向是固定不变**的，一直指向的是定义函数的环境。

   ```js
   //使用function定义的函数, this指向是随着调用环境的变化而变化的
   function foo(){
   	console.log(this);
   }
   var obj = { aa: foo };
   foo(); //Window
   obj.aa() //obj { aa: foo }
   
   //使用箭头函数定义函数 this的指向是没有发生变化的
   var foo = () => { console.log(this) };
   var obj = { aa:foo };
   foo(); //Window
   obj.aa(); //Window
   ```

3. 构造函数

   ```js
   //使用function方法定义构造函数
   function Person(name, age){
   	this.name = name;
   	this.age = age;
   }
   var lenhart =  new Person(lenhart, 25);
   console.log(lenhart); //{name: 'lenhart', age: 25}
   
   //尝试使用箭头函数
   var Person = (name, age) =>{
   	this.name = name;
   	this.age = age;
   };
   var lenhart = new Person('lenhart', 25); //Uncaught TypeError: Person is not a constructor
   ```

4. 由于js的内存机制，function的级别最高，而用箭头函数定义函数的时候，需要var(let const定义的时候更不必说)关键词，而var所定义的变量不能得到变量提升，故箭头函数一定要定义于调用之前！

   ```JS
   foo(); //123
   function foo(){
   	console.log('123');
   }
   
   arrowFn(); //Uncaught TypeError: arrowFn is not a function
   var arrowFn = () => {
   	console.log('456');
   };
   ```

   

