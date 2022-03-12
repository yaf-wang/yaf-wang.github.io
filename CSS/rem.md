**（一）、rem的使用**

　  rem是css3中新增加的一个单位属性(font size of the root element)

　　**相对长度单位。相对于根元素(即html元素)[font-size](http://www.css88.com/book/css/properties/font/font-size.htm)计算值的倍数**

　　rem的在桌面浏览器上的初始值是16px（即1rem = 16px）

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<!-- rem的初始赋值 -->
<html style="">
    <head></head>
    <body>
         <div style="font-size: 1rem">此处的1rem即设备的默认字体大小，桌面浏览器默认字体大小是16px</div>
    </body>
</html>

<html style="font-size: 12px">
    <head></head>
    <body>
         <div style="font-size: 1rem">此处的1rem即12px</div>
    </body>
</html>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　当然还可以在css上进行初始赋值，或者使用js进行动态的初始赋值

```
/*使用css进行rem的初始赋值*/
html{
      font-size:  14px;    /*此处的初始赋值表示1rem=14px*/
}
```

 

**（二）、rem的 62.5% 和 10px的区别**

　　在桌面浏览器上 font-size 的默认值是16px;

　　可知 font-size: 62.5%; 即表示10px （通过计算 16 * 62.5% = 10 得到）

　　那么 font-size: 62.5%;  和 font-size: 10px; 的区别在什么地方呢？

 

　　比较好的解释：

　　　　桌面浏览器默认页面字体大小是16px，这种情况下设置成具体像素大小或者对应的百分比，看起来的效果是一样的；

　　　　但是其他类型的设备的默认字体大小不一定是16px，

　　　　特别是高分辨率的设备，16px大小的字体在它们上面看起来会非常小，所以不能在body上设置具体像素值，

　　　　设置成百分比，可以按照设备的基准字体大小给编写的网页设置好最适合用户浏览的字体大小。

　　《响应式Web设计实践》书中原文：最重要的不是屏幕实际的像素大小，屏幕上文字的可读性才是最重要的。

　　原文地址https://segmentfault.com/q/1010000002411895　

 

**（三）、正确的rem使用方法**

　　如上文所述：

　　使用 font-size: 62.5%; 更好

```
/*rem的初始赋值*/
html{
      font-size:  62.5%;    /*此处的初始赋值表示1rem=10px*/
}
```

　　然而坑无处不在。。。

　　新的问题： 我们开发常用的chrome浏览器，支持的最小字体大小是12px，

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
/*rem在不同浏览器下的结果*/
html{
    font-size:62.5%;
} 
header{
    height:8rem;    /*在其他浏览器表示80px，在chrome上表示96px*/
} 
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　解决办法：

```
　　font-size:625%;
```

　　1rem = 100px， 以此为单位进行换算，可以避免以上问题的出现

 

**（四）、em 和 rem的区别**

　　rem是相对于**根节点**的font-size计算

　　em是相对于**父节点**的font-size计算　　

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<html>
    <head>
        <style>
            html{ 
                 font-size: 625%  
            }
            .child{
                 height: 1rem;
                 width: 1rem;  
            }  
        </style>
    </head>
    <body>
　　　　<!-- rem是相对于根节点的font-size计算 -->
        <div class="child">此处的1rem = 100px</div>

　　　　<!-- em是相对于父节点的font-size计算 -->
        <div style="font-size:10px;"><!-- 父元素的字体大小是10px-->
            <div style="height:1em;width:1em;"></div><!-- 所以子元素的em是1em=10px；-->
        </div>
    </body>
</html>        
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

**（五）、<meta name="viewport">的含义**　　　

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<!-- html头部一般会加这么一行代码 -->
<html>
    <head>
        <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0,user-scalable=no">
    </head>    
</html>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　那么，它有什么作用呢？？

　　viewport用于设置移动端自适应——　　

　　但如果是浏览流动布局的网页那情况会非常糟糕，设想一个宽 度为30%的侧边栏对于320px[手机屏幕](https://www.baidu.com/s?wd=手机屏幕&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)而言也就96px，只能容纳8个12px的汉字，可阅读性非常差。

　　*为了让手机也能获得良好的网页浏览体验，Apple找到了一个办法：在移动版(iOS)的Safari中定义了viewport meta标签①，它的作用就是创建一个虚拟的窗口(viewport)，而且这个虚拟窗口的分辨率接近于桌面显示器*

| width         | 设置layout viewport 的宽度，为一个正整数，或字符串"width-device" |
| ------------- | ------------------------------------------------------------ |
| initial-scale | 设置页面的初始缩放值，为一个数字，可以带小数                 |
| minimum-scale | 允许用户的最小缩放值，为一个数字，可以带小数                 |
| maximum-scale | 允许用户的最大缩放值，为一个数字，可以带小数                 |
| height        | 设置layout viewport 的高度，这个属性并不重要，很少使用       |
| user-scalable | 是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许 |

 

 

 

 

 

 

 

 

**（六）、根据屏幕宽度等比压缩网页**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 //根据屏幕计算设计rem的标准中
 var documentWidth = document.documentElement.offsetWidth;

 if(documentWidth > 1268 ){
    document.documentElement.style.fontSize = documentWidth/166 + "px";
 } 
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　使用上面的代码，即可根据屏幕宽度等比压缩网页

　　但有2个前提：

　　　　1. css代码涉及大小的，统一使用rem进行设置；

　　　　2. html头部不能使用：<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0,user-scalable=no">

 　　　同理：当不需要等比压缩页面时，记得把这行代码加上去，否则等比压缩一直存在。。。

　　原理： 

　　　　是根据屏宽动态的设置根节点的font-size，以此进行rem的初始值设置。实现对不同屏幕宽度的适配。