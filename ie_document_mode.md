IE文档模式控制
====

IE8之后的版本，内置了叫做“文档模式”的概念，即高版本IE模拟低版本IE行为的渲染模式。开发者可通`<meta>`标签或者HTTP响应头来控制这一行为。
这里我们以控制IE以edge模式(即使用当前IE内置的最新版本渲染引擎，并非指Edge浏览器)渲染网页为例：

```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  ... 以下代码省略
```

或者用HTTP响应头来控制(以nginx配置文件为例)：
```nginx.conf
add_header "X-UA-Compatible" "IE=Edge,chrome=1";
```
> 注意这里有个深坑，微软的官方文档给你提供了多种“兼容模式”，但是这些兼容模式往往带来更多深坑，因为IE9及以上，实现低版本的兼容模式的办法是“模拟”，而非内置了旧的渲染引擎，所以并不是100%一摸一样的，强烈建议直接采用edge模式，更详细的原因这里就不展开讨论了

**注意：HTML中必须包含文档类型声明(`<!DOCTYPE html>`)，否则会导致IE在“IE5模式(又称作怪癖模式、quirks模式)”下渲染网页**

在上面的例子当中，X-UA-Compatible中包含了两个控制指令，`IE=edge`是IE文档模式控制，`chrome=1`是谷歌浏览器内嵌框架控制指令，[详细信息参考这里](gcf.md)。
