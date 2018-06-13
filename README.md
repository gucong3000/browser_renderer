双内核浏览器内核切换控制技术
====

## 什么是双核浏览器
双核浏览器支持使用两个或者以上的浏览器引擎来渲染网页，目前绝大多数国产浏览器均为双核甚至多核。

## 双核到底是什么内核
- 基于Chromium的Blink/Webkit内核。一般在国产浏览器中被称为“极速内核/极速模式”。该内核随着该浏览器的更新而更新。
- IE内核。一般在国产浏览器中被称为“IE内核/兼容模式”，是指调用Windows系统中内置的IE，并非该浏览器单独内置了一套IE，该内核随着Windows或者IE的更新而更新。
  > 唯一的例外情况是2012年360安全浏览器曾经推出内置IE的版本

## IE内核的兼容模式
某些国产浏览器在“IE内核”下，可以切换其“兼容模式”，这并不是切换不同的IE内核版本，而是通过调用系统中IE内核的不同“文档模式”来实现的，[详细的信息请参阅这里](ie_document_mode.md)。这可能造成一些问题。比如A用户系统中安装了IE8，使用QQ浏览器的“兼容模式 - 7”；B用户系统中安装了IE11，也使用QQ浏览器的“兼容模式 - 7”，虽然都用的同一个浏览器且选择了同一个兼容模式，但是对于html5表单项等诸多DOM细节，有着很大的差异。

## 如何配置网站要使用的渲染引擎
在html的`<head>`标签中加入如下代码：
```html
<!DOCTYPE html>
<html>
<head>
  <meta name="renderer" content="webkit">
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  ... 以下代码省略

```

这里我们一共添加了三条有关浏览器渲染方面的指令：
 - `<meta name="renderer" content="webkit">`中的`webkit`指令，可以让QQ、傲游、360等浏览器默认使用Chromium内核渲染页面。
 - `X-UA-Compatible` 中的`IE=edge`指令，可以让IE或者调用IE内核的浏览器，使用标准模式渲染网页，注意这里和“Edge浏览器”无关，只是恰巧重名罢了。
 - `X-UA-Compatible` 中的`chrome=1`指令，可以让安装了[GCF插件](gcf.md)的IE，在打开网页时使用Chromium内核渲染页面。

## 需要注意几个重要的神坑：
- `<meta>`标签必须出现在`<head>`内的顶部，否则浏览器可能无法识别。
- `<!DOCTYPE html>`文档类型声明必须写，否则各种浏览器内核均会以“IE5模式(又称作怪癖模式、quirks模式)”渲染网页。
- 测试效果时，网站必须以域名访问，内网或者本地地址方式可能对部分浏览器无效。
- 如果用户曾经自主选择过渲染引擎，浏览器将记住这个选择，它的优先级高于我们的指令。如果测试时不小心点了，在必要的情况下需要卸载浏览器并清空用户数据，然后重装。
- 应该尽量保证整站的渲染内核一致，以便避免内核切换可能带来的cookie丢失问题。

## 通过js判断当前浏览器内核及文档模式

```html
<script src="//gucong3000.github.io/browser.js/browser.min.js"></script>
<script>
if (browser.MSIE) {
  alert("系统IE版本：" + browser.rv + "\n文档模式：" + browser.MSIE);
} else if (browser.Edge) {
  alert("Edge内核浏览器");
} else if (browser.Webkit) {
  alert("Blink/Webkit内核的浏览器");
} else if (browser.Gecko) {
  alert("Gecko内核的浏览器");
}
</script>
```

## 参考文档链接

### 开发者可控制内核切换
- [QQ浏览器内核切换控制](http://browser.qq.com/faq/#/detail/36)
- [傲游浏览器内核切换控制](http://bbs.maxthon.cn/thread-13159-1-1.html)
- [360安全浏览器内核切换控制](http://se.360.cn/v6/help/meta.html)
- [IE网站开发人员的兼容性功能](https://blogs.msdn.microsoft.com/ie/2010/06/16/ies-compatibility-features-for-site-developers/)

### 只支持用户自主切换内核
- [搜狗浏览器内核手动切换](http://ie.sogou.com/help2/help-4-7.html)
- [UC浏览器电脑版手动切换](http://kf.uc.cn/self_service/web/faqdetails-8311415_8311606_5917767_3.html)

### 资料暂缺或不明确
- 猎豹安全浏览器：[官方在论坛说](http://bbs.duba.net/thread-23378708-1-1.html)不支持内核切换控制，但[非官方资料](https://blog.csdn.net/a0405221/article/details/78903104)说可以
- 360急速浏览器：无官方资料，但根据[非官方资料](https://blog.csdn.net/a0405221/article/details/78903104)，支持内核切换控制
- 百度浏览器：无相关资料