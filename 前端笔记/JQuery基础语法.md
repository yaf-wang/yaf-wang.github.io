#### jQuery概述

##### 1、描述

##### 2、jQuery版本

- 1x：兼容IE6、7、8等低版本浏览器，官网不再维护。
- 2x：不兼容IE6、7、8等低版本的浏览器，官网不再维护。
- 3x：不兼容IE6、7、8等低版本的浏览器，官网主要维护版本。

#### jQuery基本使用

##### 1、jQuery入口函数

- 等待页面DOM加载完毕后再运行JS，
- 相当于原生JS中的DOMContentLoaded（当初始的 **HTML** 文档被完全加载和解析完成之后，**`DOMContentLoaded`** 事件被触发，而无需等待样式）;
- 不同于原生JS中的load事件（当整个页面及所有依赖资源如样式表和图片都已完成加载时，将触发`load`事件）；

```js
// 第一种写法
$(document).ready(function(){
  console.log('document onready!');
});

// 第二种写法
$(function(){
  console.log('document onready!');
});
```

##### 2、jQuery的顶级对象$

- $是jQuery的别称；
- $同时也是jQuery的顶级对象

##### 3、jQuery对象和DOM对象

- 用原生JS获取来的对象就是DOM对象；
- jQuery方法获取的元素就是jQuery对象；
- jQuery对象本质是：利用$对DOM对象包装后产生的对象（伪数组形式存储）；

##### 4、DOM对象和jQuery对象指向可以项目转换的

因为原生JS比jQuery更大，原生的一些属性和方法jQuery是没有封装，若要使用这些属性就需要把jQuery对象转换成DOM对象才能使用。

```JS
// 1、DOM对象转换成jQuery对象：$(DOM对象)
$('div');

// 2、jQuery对象转换为DOM对象（两种方式）
$('div')[index] // index是索引号
$('div').get[index]; // index是索引号
```

#### jQuery常用API

##### 1、jQuery选择器

###### 1.1、常用的选择器

| 名称       | 用法            | 描述                     |
| ---------- | --------------- | ------------------------ |
| ID选择器   | $('#id')        | 获取指定ID的元素         |
| 全选择器   | $('*')          | 匹配所有元素             |
| 类选择器   | $('.class')     | 获取同一类class的元素    |
| 标签选择器 | $('div')        | 获取同一类标签的所有元素 |
| 并集选择器 | $('div,p,li')   | 选取多个元素             |
| 交集选择器 | $('li.current') | 交集元素                 |

###### 1.2、层级选择器

| 名称       | 用法       | 描述                         |
| ---------- | ---------- | ---------------------------- |
| 子代选择器 | $('ul>li') | 使用>号，获取子层级的元素    |
| 后代选择器 | $('ul li') | 使用空格，代表选择后代选择器 |

###### 1.3、筛选选择器

| 语法         | 用法                    | 描述                                                      |
| ------------ | ----------------------- | --------------------------------------------------------- |
| :first       | $('li:first')           | 获取第一个li元素                                          |
| :last        | $('li:last')            | 获取最一个li元素                                          |
| :eq(index)   | $('li:eq(2)')           | 获取li元素中，索引号为2的元素，索引号index从0开始         |
| :odd         | $('li:odd')             | 获取li元素中，索引号为奇数的元素                          |
| :even        | $('li:even')            | 获取li元素中，索引号为偶数的元素                          |
| [attr]       | $("[href]")             | attr属性值，选取带有 href 属性的元素                      |
| [attr='val'] | $("a[target='_blank']") | 选取所有 target 属性值**（不）等于** "_blank" 的 <a> 元素 |

###### 1.4、**筛选方法**（重要）

| 语法               | 用法                          | 描述                               |
| ------------------ | ----------------------------- | ---------------------------------- |
| parent()           | $('li').parent()              | 查找父级                           |
| children(selector) | $('ul').children('li')        | 相当于$('ul>li')，查找子元素       |
| find(selector)     | $('ul').find('li')            | 相当于$('ul li')，后代选择器       |
| siblings(selector) | $('.first').siblings('li')    | 查找兄弟节点，不包含自己本身       |
| nextAll([expr])    | $('.first').nextAll()         | 查找当前元素之后所有同辈元素       |
| prevtAll([expr])   | $('.first').prevtAll()        | 查找当前元素之前所有同辈元素       |
| hasClass(class)    | $('li').hasClass('protected') | 检查当前元素是否包含某个类名       |
| eq(index)          | $('li').eq(2)                 | 获取索引号为2的li，同$('li:eq(2)') |

###### 1.5、隐式迭代

遍历内部DOM元素（伪数组形式存储）的过程就叫做**隐式迭代**

即：给匹配到的所有元素进行循环遍历，执行相应的方法，而不用我们进行循环，简化我们的操作；

```html
<div>1</div>
<div>2</div>
<div>3</div>
<script>
  $('div').css('color', 'red'); // 隐式迭代 把所有的div进行遍历重新设置样式
</script> 
```

###### 1.6、**链式编程（重要）**

链式编程实际是将多个方法（函数）通过某种方式链接在一起。

- 浏览器就不必多次查找相同的元素
- 使多个逻辑块能按流程逐步执行（或跳过执行），从而实现解耦

```JS
$("#p1").css("color","red").slideUp(2000).slideDown(2000);
```

###### 1.7、**扩展知识**

- 链式编程
- promise

##### 2、jQuery样式操作

###### 2.1、CSS 操作的

| 事件          | 描述                                |
| ------------- | ----------------------------------- |
| addClass()    | 向被选元素添加一个或多个类          |
| removeClass() | 从被选元素删除一个或多个类          |
| toggleClass() | 对被选元素进行添加/删除类的切换操作 |
| css()         | 设置或返回样式属性                  |

###### 2.2、显示隐藏

| 事件     | 描述          |
| -------- | ------------- |
| show()   | 显示          |
| hide()   | 隐藏          |
| toggle() | 显示/隐藏切换 |

2.3、滑动动画

| 事件                                                         | 描述                                             |
| ------------------------------------------------------------ | ------------------------------------------------ |
| [slideDown()](https://www.runoob.com/try/try.php?filename=tryjquery_slide_down) | 向下滑动元素                                     |
| [slideUp()](https://www.runoob.com/try/try.php?filename=tryjquery_slide_up) | 向上滑动元素                                     |
| [slideToggle()](https://www.runoob.com/try/try.php?filename=tryjquery_slide_toggle) | 可以在 slideDown() 与 slideUp() 方法之间进行切换 |

语法示例：

> $(*selector*).slideUp(*speed,callback*);
>
> 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
>
> 可选的 callback 参数是滑动完成后所执行的函数名称。

```JS
// hover事件 只传递一个参数， 则它将会在 mouseenter 和 mouseleave 事件上运行。
// 使用stop()取消动画排队效果，相当于停止或结束上一次动画，防止频繁切换动画问题，注意的是必须放到动画之前
$('.tab>ul>li').hover(function() {
  $(this).toggleClass('active').children('ul').stop().slideToggle(300)
})
```



###### 2.2、鼠标事件：

| 事件                                                         | 描述                                               |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [mouseenter()](https://www.runoob.com/jquery/event-mouseenter.html) | 只有鼠标指针**移入**事件所**绑定的元素时**         |
| [mouseleave()](https://www.runoob.com/jquery/event-mouseleave.html) | 只有鼠标指针**移出**事件所**绑定的元素时**         |
| [mouseover()](https://www.runoob.com/jquery/event-mouseover.html) | 只有鼠标指针**移入**事件所**绑定的元素或其子元素** |
| [mouseout()](https://www.runoob.com/jquery/event-mouseout.html) | 只有鼠标指针**移出**事件所**绑定的元素或其子元素** |

**mouseover ,mouseout ,mouseenter,mouseleave的区别**

1. 只要鼠标指针移入（或移出）事件所绑定的元素或其子元素，都会触发mouseover（或mouseout）事件
2. 只有鼠标指针移入（或移出）事件所绑定的元素时，才会触发mouseenter（或mouseleave）事件
3. 也就是说：mouseover支持事件冒泡，而mouseenter不支持事件冒泡