关于iOS兼容的一揽子问题

## 输入元素

1. 若无法正常进行对焦输入

   解决方法：在css中加上以下代码：

   ```css
   textarea {
       user-select: text;
       -webkit-user-select: text !important;
       -moz-user-focus: text !important;
   }
   ```

2. input在使用了FastClick变得难以对焦，需要双击才能对焦

3. textarea在使用了FastClick光标始终定位在文字最前面

   解决方法：对FastClick原型进行修改

   ```javascript
   FastClick.prototype.focus = function(targetElement) {
       // 解决iOS input 无法聚焦问题
       targetElement.focus();
       // 解决iOS textarea光标位置问题
       if(pageState.isIOS && targetElement.setSelectionRange && targetElement.value) {
           const length = targetElement.value.length
           targetElement.setSelectionRange(length, length)
       }
   };
   ```


## 安全区适配

以bottom为例，top/left/right同理

页面最底部元素加上以下css代码（margin-bottom也可以）：

```css
.bottom_ele {
    padding-bottom: constant(safe-area-inset-bottom);
    padding-bottom: env(safe-area-inset-bottom);
}
```

但是若该元素已有padding-bottom，则安卓会被覆盖为0

```css
.bottom_ele {
    padding-bottom: 55px; // 安卓该行不会生效，子元素会直接贴底
    padding-bottom: constant(safe-area-inset-bottom);
    padding-bottom: env(safe-area-inset-bottom);
}
```

解决方法：ios单独判断

```scss
.bottom_ele {
    padding-bottom: 55px;
    &.ios {
        padding-bottom: constant(safe-area-inset-bottom);
    	padding-bottom: env(safe-area-inset-bottom); 
    }
}
```

若页面底部为固定定位的元素，可以操作bottom属性：

```css
.fix_in_bottom {
	position: fixed;
    bottom: 0;
    bottom: constant(safe-area-inset-bottom);
    bottom: env(safe-area-inset-bottom); 
}
```

## 滑动问题

滑动区域设置：

```css
.scroll_area {
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
}
```

若仍无法滑动，结合安卓情况，可怀疑容器或组件问题，如下使用了action-sheet：

```html
<!-- action-sheet里的内容需要设置lock-scroll为false才能滑动 -->
<action-sheet
    v-model:show="show"
    :lock-scroll="false"
>
    <!-- 内容 -->
</action-sheet>
```

> [简书@朋_朋 | VUE前端界面在iOS中遇到的坑](https://www.jianshu.com/p/dcdec7572987)
>
> 在需要滑动的位置加上如下css代码：
>
> -webkit-overflow-scrolling:touch;
>
> 以此，界面滑动卡顿问题得以解决，但是，这一行代码会带来一个副作用：
>
> 1、在滑动界面之中使用的position：fixed 无法固定下来，会随着界面进行一起滚动
>
> 解决方法：将使用的position：fixed（头部导航）写在滑动部位外部，在使用绝对定位进行布局，以此解决问题
>
> 2、vue中使用v-if导致的界面初始化之后无法滑动
>
> 解决方法：将v-if改成v-show进行展示，解决界面进入之后第一次不能滑动的问题

## 其他

1. 若某些css属性在安卓上生效，但是在iOS上不生效，可以添加-webkit-进行适配，如

   ```css
   .box {
       backdrop-filter: blur(10px); // 安卓有高斯模糊
       -webkit-backdrop-filter:blur(10px); // 加上后iOS才有高斯模糊
   }
   ```

   

## 相关博客文章

[简书@朋_朋 | VUE前端界面在iOS中遇到的坑](https://www.jianshu.com/p/dcdec7572987)

[CSDN@weixin_39861918 | vue x 兼容iphone_作为前端你必须知道的iPhoneX适配](https://blog.csdn.net/weixin_39861918/article/details/111755173)

[CSDN@Jarvan大熊 | H5页面适配iPhoneX，就是这么简单](https://blog.csdn.net/dx18520548758/article/details/80010358)

[CSDN@ShiyuTim | vue h5 项目适配ios问题](https://blog.csdn.net/weixin_44623040/article/details/103092877)

