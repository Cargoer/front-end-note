报错：`error 'xxx' is assigned a value but never used  no-unused-vars`

原因：eslint的验证语法

解决方法一：

1. 全局搜索“rules”，可能在package.json也可能在单独的js如.eslintrc.js

2. 在rules里添加以下代码

```json
"generator-start-spacing": "off",
"no-tabs": "off",
"no-unused-vars": "off", // 此为检测变量未使用，加上此行解决本报错
"no-console": "off",
"no-irregular-whitespace": "off",
"no-debugger": "off"
```

解决方法二（尚未证实有效）：在错误语句后添加注释 `// eslint-disable-line no-unused-vars`，如：

```javascript
methods: {
    aFunction() {
        let aVarThatWillNeverUsed = '' // eslint-disable-line no-unused-vars
        ...
    }
}
```



报错：`Error: EPERM: operation not permitted, mkdir`

原因：dist文件夹被占用

解决方法：关闭占用dist文件夹的进程
