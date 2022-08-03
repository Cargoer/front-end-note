## 数据类型相关

### 数据类型判断

* 运算符 typeof

  ```javascript
  typeof 3 // number
  typeof true // boolean
  typeof undefined // undefined
  typeof '123' // string
  
  typeof [1,2,3] // object
  typeof {a:1,b:2} // object
  typeof new Date() // object
  typeof null // object
  
  const f = a => a*a
  typeof f // function
  ```

* 手写，具体化object

  ```javascript
  function typeOf(obj) {
      return Object.prototype.toString.call(obj).slice(8, -1).toLowerCase()
  }
  typeOf(3) // number
  typeOf(true) // boolean
  typeOf(undefined) // undefined
  typeOf('123') // string
  
  typeOf([1,2,3]) // array
  typeOf({a:1,b:2}) // object
  typeOf(new Date()) // date
  typeOf(null) // null
  
  const f = a => a*a
  typeOf(f) // function
  ```

* 判断数组的方法

  1. ```javascript
     Array.isArray(arr)
     ```

  2. ```javascript
     Object.prototype.toString.call(arr).slice(8, -1).toLowerCase() == 'array'
     ```

  3. 待补充

## 数组相关

### 数组扁平化

```javascript
// arr.flat(depth)
let arr = [1,[2,[3]]]
arr.flat(0) // [1,[2,[3]]]，等于没有扁平化
arr.flat(1) // [1,2,[3]], 扁平一层嵌套
arr.flat(2) // [1,2,3], 扁平两层
```

## 函数相关

### 立即执行函数

参考：[CSDN@garrulousabyss | 什么是立即执行函数？有什么作用](https://blog.csdn.net/garrulousabyss/article/details/85193494)

1. 什么是立即执行函数

    简单来说，就是【匿名函数+调用】，形式为：

    ```javascript
    function() {...}()
    ```

    不过为了通过浏览器的语法检查，需要加点东西：

    ```javascript
    (function() {...}())
    (function() {...})()
    !function() {...}()
    +function() {...}()
    -function() {...}()
    ~function() {...}()
    void function() {...}()
    new function() {...}()
    ```

2. 有什么用

    > 只有一个作用：创建一个独立的作用域。
    >
    > 这个作用域里面的变量，外面访问不到（即避免「变量污染」）。

## 防抖

定义：在一段时间内的执行连续操作，只实现最后一次操作，防止重复操作

### 代码实现：

```javascript
function debounce(fn, delay) {
    // timer为私有变量，函数外无法访问
    let timer = null
    return function(...args) {
        // 但是返回的匿名函数可以访问timer变量
        // 所以下一次调用时，若上一次的计时器还没结束并执行，会被清除
        if(timer) {
            clearTimeout(timer)
        }
        timer = setTimeout(fn(...args), delay)
    }
}
```

调用示例：

```javascript
// 若执行点击操作后500毫秒内再次点击，则会覆盖之前的操作
document.getElementById("xxx").onclick = debounce(function() {
    ...
}, 500)

// 问题：在vue内的实现
```

### 思考：

> 若每次操作后在delay时间内都又执行了操作，那么永远都等不到最后一次操作，操作会迟迟无法进行
>
> 是否可能存在此场景？
>
> 无聊的人疯狂快速地点击按钮
>
> 页面持续滑动
>
> 鼠标不断移动（物理上抖动）

思路：点击后立即执行操作，并设置一个计时器阻挡后续短时间内的连续操作

可能实现（未证实）：

```javascript
function debounce_head(fn, delay) {
    let timer = null
    return function(...args) {
        if(timer) {
            return
        }
        fn(...args)
        timer = setTimeout(() => {
            clearTimeout(timer)
        }, delay)
    }
}
```

### lodash实现

```javascript
function debounce(func, wait, options) {
    // 闭包变量
    let lastArgs,
        lastThis,
        result,
        timerId,
        lastCallTime,
        lastInvokeTime
    
    // 没传wait
    let useRAF = (!wait && wait !== 0 && typeof root.requstAnimationFrame === 'function')
    
    // 校验func
    if(typeof func !== 'function') {
        throw new TypeError('Expected a function')
    }
    
    // 校验wait
    wait = +wait || 0
    
    // 获取options
    let leading = false
    let maxing = false
    let maxWait
    let trailing = true
    if(isObject(options)) {
        leading = !!options.leading
        trailing = 'trailing' in options? !!options.trailing: trailing
        // 节流函数预留
        maxing = 'maxWait' in options
        maxWait = maxing? Math.max(+options.maxWait || 0, wait): maxWait
    }
    
    function shouldInvoke(time) {
        const timeSinceLastCall = time - lastCallTime
        const timeSinceLastInvoke = time - lastInvokeTime
        return (lastCallTime === undefined || 
               timeSinceLastCall >= wait ||
               timeSinceLastCall < 0 ||
               (maxing && timeSinceLastInvoke >= maxWait))
    }
    
    function startTimer(pendingFunc, wait) {
        if(useRAF) {
            root.cancelAnimationFrame(timerId)
            return root.requestAnimationFrame(pendingFunc)
        }
        return setTimeout(pendingFunc, wait)
    }
    
    function invokeFunc(time) {
        const args = lastArgs
        const thisArg = lastThis
        
        lastArgs = lastThis = undefined
        lastInvokeTime = time
        result = func.apply(thisArg, args)
        return result
    }
    
    function leadingEdge(time) {
        lastInvokeTime = time
        timerId = startTimer(timerExpired, wait)
        return leading? invokeFunc(time): result
    }
    
    function trailingEdge(time) {
        timerId = undefined
        
        if(trailing && lastArgs) {
            return invokeFunc(time)
        }
        lastArgs = lastThis = undefined
        return result
    }
    
    function remainingWait(time) {
        const timeSinceLastCall = time - lastCallTime
        const timeSinceLastInvoke = time - lastInvokeTime
        const timeWaiting = wait - timeSinceLastCall
        return maxing? Math.min(timeWaiting, maxWait - timeSinceLastInvoke): timeWaiting
    }
    
    function timerExpird() {
        const time = Date.now()
        if(shouldInvoke(time)) {
            return trailingEdge(time)
        }
        timerId = startTimer(timerExpired, remainingWait(time))
    }
        
    // 入口函数
    function debounced(...args) {
        const time = Date.now()
        const isInvoking = shouldInvoke(time)
        
        lastArgs = args
        lastThis = this
        lastCallTime = time
        
        if(isInvoking) {
            // 无timerId
            if(timerId === undefined) {
                return leadingEdge(lastCallTime)
            }
            
            // 设置了最大等待时间
            if(maxing) {
                timerId = startTimer(timerExpired, wait)
                return invokeFunc(lastCallTime)
            }
        }
        if(timerId === undefined) {
            timerId = startTimer(timerExpired, wait)
        }
        return result
    }
}
```

