##### 1、margin高度塌陷问题（BFC）

###### 相邻兄弟

解决方法：

- 给父元素增加边框；
- 移除隐藏
- 利用浮动（不建议，这样盒子高度不能撑开）；
- 给父元素添加position: fixed；
- 给父元素设置display:table;
- 使用伪元素clearfix
- 

2、