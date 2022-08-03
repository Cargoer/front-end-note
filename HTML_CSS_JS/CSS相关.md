## @support

> 该语法可以防止某些属性在某些平台或机型上不支持而导致无法使用的问题

用法：

```css
@support (display: flex) {
    // 支持display的对应填写样式
    .box {
        display: flex;
    }
}
```

## 文字相关

[知乎@魏学文 | 大部分前端工程师的font-family都用错了](https://zhuanlan.zhihu.com/p/465313889)

## 段落相关

### line-height

> 相关博客
>
> [CSDN@小乔与你同在 | CSS样式：line-height、height与font-size的联系](https://blog.csdn.net/weixin_43109549/article/details/100387545)

CSS中添加了line-height之后会使得每行文字有一定程度的上下留隙，而UI进行页面切分时第一行与最后一行是紧贴文字的

* 若不设置line-height，也没有对容器设置高度，**font-size ≠ height**。这是因为line-height会有一个默认值，可能略大于font-size的值，导致撑起来的容器高度略大于font-size
* 若设置了line-height， 行间距 = line-height - fontsize，效果等同于height = (半间距 + fontsize + 半间距) * 行数

总结：

1. 遇到单行文本，应设置line-height与font-size相等，否则会无形增加高度
2. 遇到多行文本，容器与上下容器的间距应分别减少一个半间距

满足上面两个条件才能够与UI保持完全的一致

## 更多相关博客文章

[博客园@晴明桑 | [css]一些兼容性(奇怪)问题](https://www.cnblogs.com/qingmingsang/articles/8414101.html)

[CSDN@HIES | CSS浏览器兼容性的4个解决方案](https://blog.csdn.net/woshixiaodaoasu/article/details/94409268)

## 

