## 尽量少用 innerHTML
* 原因：因为 innerHTML 会调用一个沉重且高消耗的HTML解析器。

## 缓存ajax请求

## 缓存DOM操作
* `NodeList` 或 `HTMLCollection` 由于这两个集合的实时性原因，每次访问此集合都会重新查询文档。这将导致性能问题，应该做缓存处理

## JavaScript默认是同步解析的
* 当DOM在解析时遇到 `<script>` 标签，将停止解析文档，并执行JavaScript脚本，如果是外部脚本，必须要下载后再解析，这将导致性能问题。

## 处理JavaScript事件时的性能问题
* 过多的绑定事件处理函数，每个函数都是对象，都会占用内存
* 在绑定事件的时候务必要访问DOM元素，如果访问的过多，这也将导致性能问题
* 内存中留有废掉的事件处理函数：
    * 使用 removeChild() 或 replaceChild() 函数移除或替换节点时，如果被移除或替换的节点有绑定事件函数，那么该函数不会被当做垃圾回收。另外使用 innerHTML 替换DOM时也会出现这种情况

使用事件委托解决以上问题：减少DOM元素与事件函数的链接数，减少DOM访问次数，减少事件函数数量以减少内存占用。

## 最小化重排和重绘

改变元素多种样式的时候，最好用className，一次性完成操作，这样只会修改一次DOM。

[重排和重绘的概念及触发条件查看这里](/note/performance/reflow-repaint)

## FOUC (无样式内容闪烁)

该问题主要出现在 IE 浏览器，原因有两个：

* 1、使用@import方法导入CSS

```html
<style>
    @import "../reset.css";
</style>
```

`IE` 加载HTML文档后，会先解析文档，然后再去加载由 `import` 导入的外部CSS文件，在CSS没有被加载的这段时间内，页面是无样式的。

* 2、零散的添加样式引用

将样式表链接放在页面不同位置时，在IE5/6下某些页面会无样式显示内容且瞬间闪烁，这现象就是文档样式短暂失效（Flash Of Unstyled Content），即FOUC。

解决方案：

* 避免使用 `@import` 引入外部样式
* 将样式表引入放在 `<head>` 标签内
