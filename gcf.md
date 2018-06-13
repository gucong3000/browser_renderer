谷歌浏览器内嵌框架 Google Chrome Frame
====

谷歌浏览器内嵌框架是一款IE插件，他的作用是将IE魔改为双核浏览器，让开发者可以控制页面渲染内核，但不改变用户的浏览器使用习惯。如果你的老板/客户/PM/测试等，无礼的要求你兼容IE6/7/8/9，你可以在他们的电脑上安装这个插件。

该插件已经于2014年停止更新和技术支持，内置的Chrome版本很低，为32.0.1700.107，可能拥有很多安全漏洞，但如果你的用户坚持必须使用IE6/7等超级老的IE版本来访问你的网站，则你可以考虑引导他们安装这个插件。

安装了该插件的IE用户访问你的网站时，如果希望使用Chrome引擎渲染页面，可通`<meta>`标签或者HTTP响应头来控制

```html
<html>
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  ... 以下代码省略
```
或者用HTTP响应头来控制(以nginx配置文件为例)：
```nginx.conf
add_header "X-UA-Compatible" "IE=Edge,chrome=1";
```
在上面的例子当中，X-UA-Compatible中包含了两个控制指令，`IE=edge`是IE文档模式控制, [详细信息参考这里](ie_document_mode.md)，`chrome=1`是谷歌浏览器内嵌框架控制指令。
