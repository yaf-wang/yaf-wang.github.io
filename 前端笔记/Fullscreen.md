BUG描述：
浏览器网页存在左上侧无法选中问题；影响到整个浏览器其他tab页面；
异常：

Failed to execute 'requestFullscreen' on 'Element': API can only be initiated by a user gesture.

Uncaught (in promise) TypeError: fullscreen error

造成原因：

浏览器的api接口requestFullscreen造成，为了用户体验和用户的浏览安全，**这个方法只能在用户交互或者设备方向改变的时候调用，否则将会失败**

测试结果：
修复完成后必须重启浏览器，目前测试重启后正常。手动切换进入全屏/退出全屏，未发现问题。

建议：
禁止通过脚本直接进入全屏，通过脚本模拟鼠标或键盘事件也会存在抛出异常情况。若是想进入全屏需要用户点击全屏按钮，进行交互动作

Dd

1. ddd
   1. Dd

相关文档：
Element.requestFullscreen()：://developer.mozilla.org/zh-CN/docs/Web/API/Element/requestFullScreen