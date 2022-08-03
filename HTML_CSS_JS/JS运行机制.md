## Javascript线程

参考：[博客园@youxin | javascript线程解释（setTimeout,setInterval你不知道的事）](https://www.cnblogs.com/youxin/p/3354924.html)

## Javascript执行上下文和执行栈

参考：[github@yygmind |【进阶1-1期】理解Javascript中的执行上下文和执行栈](https://github.com/yygmind/blog/issues/12)

### 名词解释

**执行上下文**共有三种类型

```javascript
console.log(a) // 1
var a = 'global a'
console.log(a) // 2
var b = 'global b'
function f(b, c) {
    console.log(a) // 3 这里访问的不是函数外的变量a，因为下面的a变量提升了
    var a = 'a in f'
    console.log(a) // 4
    function ff() {
        console.log(a) // 4-1
    }
    ff()
}
function g(b, c) {
    console.log(a) // 5
}
f('b', 'c')
ff('b', 'c')

////// 运行结果
1 < undefined
2 < global a
3 < undefined
4 < a in f
4-1 < a in f
5 < global a
```

如上例，js执行过程如下：

1. 创建全局上下文

   ```javascript
   globalEC = {
       // arguments 全局没有此过程
       
       // function
       f: <f reference>,
       g: <g reference>,
       
       // variable
       a: undefined,
       b: undefined,
       
       // this
       this: window
       
       // 作用域链
       scopeChain: [global]
   }
   
   执行1/2操作时只有全局上下文
   当执行到1时，a在全局上下文中，且未被赋值，则取默认的undefined
   当执行到2时，a已经被赋值为'global a'，则取其值
   ```

2. 创建f上下文

   ```javascript
   fEC = {
       // arguments
       arguments: {
           b: undefined,
           c: undefined,
           length: 2
       }
       
       // function
       ff: <ff reference>
       
       // variable
       a: undefined,
       
       // this
       this: window // 为什么还是window？
       
       // 作用域链
       scopeChain: [f, global]
   }
   
   操作3/4发生在f上下文创建后
   当执行到3时，f上下文（即当前上下文）中有a，且未被赋值，则取默认的undefined
   当执行到4时，当前上下文的a被赋值为'a in f'，则取其值
   ```

3. 创建ff上下文

   ```javascript
   ffEC = {
       // arguments
       arguments: {
           length: 0
       }
       
       // function
       
       // variable
       
       // this
       this: window // 这里也为什么还是window？
       
       // 作用域链
       scopeChain: [ff, f, global]
   }
   
   操作4-1发生在ff上下文创建后
   当执行到4-1时，当前上下文（ff上下文）没有a，故顺着作用域链往前面的上下文寻找，在f上下文中找到a，其值为'a in f'，故取该值
   ```

4. 弹出ff上下文

5. 弹出f上下文

6. 创建g上下文

   ```javascript
   gEC = {
       // arguments
       arguments: {
           b: undefined,
           c: undefined,
           length: 2
       }
       
       // function
       
       // variable
       
       // this
       this: window // 为什么还是window？
       
       // 作用域链
       scopeChain: [g, global]
   }
   
   操作5发生在g上下文创建后
   当执行到5时，当前上下文（g上下文）没有a，故顺着作用域链往前面的上下文寻找，在全局上下文中找到a，其值为'global a'，故取该值
   ```

7. 弹出g上下文

【注：若通过函数名加小括号的方式调用函数，this指向window】
