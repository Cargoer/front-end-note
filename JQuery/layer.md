### layer弹出层组件

* 使用

```vue
<!-- 作为独立组件使用layer -->
<script src='layer.js'></script>
```

```javascript
// 在layui中使用layer
layui.use('layer', function(){
    var layer = layui.layer;
    layer.msg('hello');
})
```

* 参数

**type**：基本层类型

（Number， 0-default信息框，1-页面层，2-iframe层，3-加载层，4-tips层）

> 若你采用*layer.open({type: 1})*方式调用，则type为必填项（信息框除外）

**title：**标题（String，弹出层标题）

**content：**内容（String/DOM/Array，弹出层内容）

**area：**宽高（String，如'500px'只定义宽度/Array，如['500px', '300px']

**offset：**坐标（默认垂直居中，一般不用设置）

**icon：**图标（Number）

**btn：**按钮（应用场景：layer.confirm）

**fixed：**固定（即鼠标滚动时，层是否固定在可视区域。如果不想，设置*fixed: false*即可）

各种回调：yes、cancel、end、full/min/restore

* 方法

**layer.open(options)**：打开弹出层，参数如上，返回当前层索引

**layer.alert(content, options, yes)**：

**layer.confirm(content, options, yes, cancel)**

**layer.msg(content, options, end)**

**layer.load(icon, options)** 加载层

**layer.tips(content, follow,options)** tips层

**layer.close(index)** 关闭指定层

**layer.closeAll(type)** 关闭所有[类型为type]的层

**layer.tab(options)**

...