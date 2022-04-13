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

