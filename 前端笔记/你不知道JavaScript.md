##### 5.5、模块

模块加载：

```js
var MyModules = (function Manger() { 
  var modules = [];

  function define(name, deps, impl) { 
    for(var i = 0; i < deps.length; i++){
      deps[i] = modules[deps[i]]
    }
    modules[name] = impl.apply(impl, deps);
  }
  function get(name) {
    return modules[name];
  }
  return {
    define: define,
    get: get
  }
})();

MyModules.define('bar', [], function () {
  function sayHello(who) { 
    return '我是--->' + who;
  }
  return {
    sayHello: sayHello
  };
})

MyModules.define('foo', ['bar'], function (bar) {
  var hungry = 'hippo';
  function awesome() {
    console.log('Hi,', bar.sayHello(hungry));
  }
  return {
    awesome: awesome
  }
})

var bar = MyModules.get('bar');
var foo = MyModules.get('foo');
console.log( bar.sayHello('hippo1'));
foo.awesome()
```

ES6模块加载

```js
// bar.js
function hello (who) {
  return '我是 bar函数，你是' + who;
}
export default {
  hello
}

// foo.js
import bar from './bar.js';
var hungry = 'hippo';
function awesome () {
  console.log('Hi,', bar.hello(hungry));
}
export default {
  awesome: awesome
}

// index.js
import bar from './bar.js'
import foo from './foo.js'
bar.hello()
foo.awesome()
```

