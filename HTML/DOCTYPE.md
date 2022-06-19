### DOCTYPE

`DOCTYPE`是`document type`（文档类型）的简写，在`web`设计中用来说明使用的`XHTML | HTML`是什么版本。

**`XHTML`:** 可扩展超文本标记语言，是一种置标语言，表现方式与超文本标记语言（HTML）类似，不过语法上更加严格。

从继承关系上讲，HTML是一种基于标准通用置标语言的应用，是一种非常灵活的置标语言，而XHTML则基于可扩展标记语言，**可扩展标记语言是标准通用置标语言的一个子集**。

```xml
<!-- <!DOCTYPE html>声明必须是HTML文档的第一行，位于<html>标签之前。 -->
<!DOCTYPE html>
<html>
    <head>
	    <title></title>
    </head>
    <body>
    
    </body>
</html>
```

在`HTML 4.01`中，`<!DOCTYPE>` 声明引用 `DTD`，因为`HTML 4.01` 基于 `SGML`。



其中`DTD`叫文档类型定义， 规定了标记语言的规则，浏览器根据定义的 `DTD` 来解释你页面的标识，并展现出来。

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```

要建立符合标准的网页，`DOCTYPE` 声明是必不可少的关键组成部分；除非你的 `XHTML` 去顶了一个正确的 `DOCTYPE` ，否则标识和 `CSS` 都不会生效。



### XHTML 1.0 提供了三种可供选择的DTD声明

1. 过渡的 `Transitional`：要求非常宽松的 `DTD`，它允许继续使用 `HTML4.01` 的标识（但是要符合 `xhtml` 的写法），完整代码如下：

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

2. 严格的 `Strict`：要求严格的 `DTD`，不能使用任何表现层的标识和属性，例如`<br>`，完整代码如下：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

3. 框架的`Frameset`：专门针对框架页面设计使用的 `DTD`，如果页面中包含有框架，需要采用这种`DTD`，完整代码如下：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```



