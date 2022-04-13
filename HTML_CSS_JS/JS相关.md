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

