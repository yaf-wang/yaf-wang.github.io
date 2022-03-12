### 什么是盒子模型、标准模式和怪异模式？

CSS中组成一个块级盒子需要：

- ContentBox：内容，大小可以通过Width、Height设置；
- Padding BoxL：内边距，大小由padding设置
- Border Box：边框，由border设置；
- Margin Box：外边距，由margin设置；

标准盒模型：width和height设置的ContentBox，padding、border一起决定整个盒子的大小；

IE盒模型：width和height包含padding、border，一般浏览器默认使用的标准模型，但是可以通box-sizing: border-box;

```css
/* 以下代码可以实现所有元素采用替代模式： */
html {
  box-sizing: border-box;
}
*, *::before, *::after {
  box-sizing: inherit;
}
```

### 外边距折叠问题

理解外边距的一个关键是外边距折叠的概念。如果你有两个外边距相接的元素，这些外边距将合并为一个外边距，即最大的单个外边距的大小。触发BFC可以避免

### BFC(块级格式上下文)

BFC目的是其中元素布局不受外界影响

如何形成BFC？

- float值不为none；
- overflow部位visible；
- display的值为table-cell、tab-caption、inline-block、block其中一个；
- postion的值为relative、fixed；

### Flex布局

### Float高度塌陷问题

定义float后浮动元素会脱离文档流；

- 父元素定义高度；
- 使用clear: both;
- overflow: hidden;

### CSS选择器有哪些？哪些属性可以继承？

选择器主要有：

- 通用选择器
- 标签选择器
- 类选择器
- ID选择器
- 属性选择器
- 伪类选择器
- 相邻选择器
- 子选择器
- 后代选择器

可继承的属性：font-size、font-weight、color、text-align（字体属性）
优先级问题：就近原则，!important > id > class > tag，!important比内联的优先级高；



**选择中双冒号和单冒号？**
单冒号是CSS3伪类；双冒号是CSS3伪元素

:before就是以一个子元素的存在，定义在元素主体内容之前的一个伪元素。并不存在于dom之中，只存在在页面之中。
:before 和 :after 这两个伪元素，是在CSS2.1里新出现的。起初，伪元素的前缀使用的是单冒号语法，但随着Web的进化，在CSS3的规范里，伪元素的语法被修改成使用双冒号，成为::before ::after

### CSS3新增伪类有哪些？

- p:fist-of-type 选择属于其父元素的首个元素
- p:last-of-type 选择属于其父元素的最后元素
- p:only-of-type 选择属于其父元素唯一元素
- p:only-child 选择属于其父元素唯一的子元素
- p:nth-child(2) 选择属于其父元素的第二个子元素
- :enabled :disabled 表单控件的禁用状态
- :checked 单选框或复选框被选中

### 浏览器是怎样解析CSS选择器？

CSS选择器的解析是从右向左解析的。若从左向右的匹配，发现不符合规则，需要进行回溯，会损失很多性能。若从右向左匹配，先找到所有的最右节点，对于每一个节点，向上寻找其父节点直到找到根元素或满足条件的匹配规则，则结束这个分支的遍历。两种匹配规则的性能差别很大，是因为从右向左的匹配在第一步就筛选掉了大量的不符合条件的最右节点（叶子节点），而从左向右的匹配规则的性能都浪费在了失败的查找上面。
而在 CSS 解析完毕后，需要将解析的结果与 DOM Tree 的内容一起进行分析建立一棵 Render Tree，最终用来进行绘图。在建立 Render Tree 时（WebKit 中的「Attachment」过程），浏览器就要为每个 DOM Tree 中的元素根据 CSS 的解析结果（Style Rules）来确定生成怎样的 Render Tree

### visibility: collapse在不同浏览器区别？

当一个元素的visibility属性被设置成collapse值后，对于一般的元素，它的表现跟hidden是一样的。

chrome中，使用collapse值和使用hidden没有区别。

firefox，opera和IE，使用collapse值和使用display：none没有什么区别。

### 元素竖向的百分比设定是相对于容器的高度吗？

当按百分比设定一个元素的宽度时，它是相对于父容器的宽度计算的，但是，对于一些表示竖向距离的属性，例如 padding-top , padding-bottom , margin-top , margin-bottom 等，当按百分比设定它们时，依据的也是父容器的宽度，而不是高度。

### 优化CSS图片加载

- 使用雪碧图
- Iconfont 字体库
- base64

### 重绘/回流

- 重绘：不会影响几何数据变化
- 回流：引起元素的几何属性变化

## 视觉效果案例

### 如何绘制0.5px的线

绘制1px通过translate: scale(0.5)

### 怎么让Chrome支持小于12px 的文字？

```css
p{font-size:10px;-webkit-transform:scale(0.8);} //0.8是缩放比例
```

### 视差滚动效果？

视差滚动（Parallax Scrolling）通过在网页向下滚动的时候，控制背景的移动速度比前景的移动速度慢来创建出令人惊叹的3D效果。

CSS3实现
优点：开发时间短、性能和开发效率比较好，缺点是不能兼容到低版本的浏览器

jQuery实现
通过控制不同层滚动速度，计算每一层的时间，控制滚动效果。
优点：能兼容到各个版本的，效果可控性好
缺点：开发起来对制作者要求高

插件实现方式
例如：parallax-scrolling，兼容性十分好