## 语法短路求值

### &&

`expr1 && expr2` 当expr1为true，expr2才执行

### ??

`expr1 ?? expr2` 当expr1为null或undefined，expr2才执行

## 逻辑空赋值（??=）

> 逻辑空赋值运算符 (`x ??= y`) 仅在 `x` 是 [nullish](https://developer.mozilla.org/zh-CN/docs/Glossary/Nullish) (`null` 或 `undefined`) 时对其赋值。
>
> 其等价于`x ?? (x = y)`

```javascript
const a = { duration: 50 };

a.duration ??= 10;
console.log(a.duration);
// expected output: 50

a.speed ??= 25;
console.log(a.speed);
// expected output: 25
```

在项目中的应用多为：赋值（键值对赋值）时对得到的数据进行非空判断，如：

```javascript
try {
    ...
}catch(e) {
    uni.showToast {
        title: e.message ?? '领取失败'
    }
}
```

## !!

!为取反运算符，当操作值不是布尔类型时，会将值先转换成布尔类型，然后取反

!!中第一个!执行转换取反操作，第二个!再取反，则!!的作用就是直接将非布尔类型的数值转换成布尔类型

**但是个人觉得没必要**，因为非布尔类型的值好像也能直接作为判断条件

在项目中的应用：

```javascript
!!timer && clearTimeout(timer)
```

## +

除了可以作为算术运算符之外，还可以将变量转为数字类型，如：

```javascript
let string_var = '123'
+string_var // 123

string_var = 'oahwoghwag'
+string_var // NaN

let boolean_var = false
+boolean_var // 0

let object_var = {a: 4}
+object_var // NaN

+undefined // NaN
```

运用：lodash源码中有使用到
