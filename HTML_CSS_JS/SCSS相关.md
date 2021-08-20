# SCSS

## 嵌套

避免使用后代选择器，使代码的层级关系更清晰

```scss
parent {
    ...
    child {
        ...
    }
}
// 等同于
parent {
    ...
}
parent child {
    ...
}
```



## 引用父级选择器“&”

同上，&可替换成当前层级的父级选择器

```scss
a {
    ...
    &:hover {
        ...
    }
    &.b {
        ...
    }
}
// 等同于
a {
    ...
}
a:hover {
    ...
}
a.b {
    ...
}
```

> 无论CSS规则嵌套的深度怎样，关键字"&"都会使用父级选择器级联替换全部其出现的位置

```scss
parent {
    ...
    child {
        ...
        &:hover {
            ...
        }
    }
}
// 等同于
parent {
    ...
}
parent child {
    ...
}
parent child:hover {
    ...
}
```



## 变量

```scss
// these are variables
$font-family-val: Helvetica;
$color-val: #fff;

body {
    font: 100% $font-family-val;
    color: $color-val;
}
// 等同于
body {
    font: 100% Helvetica;
    color: #fff;
}
```



## 混合

> 混合（Mixin）用来分组那些需要在页面中复用的CSS声明，开发人员可以通过向Mixin传递变量参数来让代码更加灵活

目的：复用

```scss
@mixin mixin_template(params: default_val){
    content
}
@mixin border-radius($radius: 10px) {
    border-radius: $radius;
    -ms-border-radius: $radius;
    -moz-border-radius: $radius;
    -webkit-border-radius: $radius;
}
.box {
    @include border-radius(10px);
}
// 等同于
.box {
    border-radius: 10px;
    -ms-border-radius: 10px;
    -moz-border-radius: 10px;
    -webkit-border-radius: 10px;
}
```



## 继承

> 继承是SASS中非常重要的一个特性，可以通过@extend指令在选择器之间复用CSS属性，并且不会产生冗余的代码

目的：复用

```scss
%template {...}
%message-common {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333;
}
.message {
    @extend %message-common;
}
.success {
    @extend %message-common;
    ...
}
.error {
    @extend %message-common;
    ...
}
// 等同于
.message,
.success,
.error {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333;
}
.success {
    ...
}
.error {
    ...
}
```

