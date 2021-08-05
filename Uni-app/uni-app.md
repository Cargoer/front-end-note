# uni-app

## 介绍

## 框架

### 配置

#### pages.json

>`pages.json` 文件用来对 uni-app 进行全局配置，决定页面文件的路径、窗口样式、原生的导航栏、底部的原生tabbar 等。它类似微信小程序中`app.json`的**页面管理**部分。注意定位权限申请等原属于`app.json`的内容，在uni-app中是在manifest中配置。

```json
{
    // 设置页面路径及窗口表现，必填
    "pages": [
        {
            "path": "path/index",
            "style": {
                "navigationBarTitleText": "小金卡",
                "app-plus": {
                    // 返回按钮
                    "backButton": {
                        "background": "#00FF00"
                    }
                }
                ...
            }
        },
        ...
    ],
    // 设置默认页面的窗口表现
    "globalStyle": {
        "navigationBarTextStyle": "black",
        ...
    },
    // 设置底部 tab 的表现
    "tabBar": {
        "selectedColor": "#12171C",
        "list": [
            {
                "pagePath": "pagePath/index",
                "iconPath": "iconPath/icon.png",
                "selectedIconPath": "selectedIconPath/selectedIcon.png",
                "text": "member"
            },
            ...
        ],
        ...
    }
}
```

自定义导航栏？[uni-nav-bar](https://ext.dcloud.net.cn/plugin?id=52)

#### App.vue

> `App.vue`是uni-app的主组件，所有页面都是在`App.vue`下进行切换的，是页面入口文件。但`App.vue`本身不是页面，这里不能编写视图元素。

* [应用生命周期](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=%e5%ba%94%e7%94%a8%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f)

* globalData

js中操作globalData的方式如下： 

`getApp().globalData.text = 'test'`

在应用onLaunch时，getApp对象还未获取，暂时可以使用this.$scope.globalData获取globalData。

* 全局样式

> 在`App.vue`中，可以定义一些全局通用样式，例如需要加一个通用的背景色，首屏页面渲染的动画等都可以写在App.vue中。

#### main.js

> `main.js`是uni-app的入口文件，主要作用是初始化`vue`实例、定义全局组件、使用需要的插件如vuex。

```vue
import Vue from 'vue'
import App from './App'
import pageHead from './components/page-head.vue' //全局引用page-head组件

Vue.config.productionTip = false
Vue.component('page-head', pageHead) //全局注册page-head组件，每个页面将可以直接使用该组件
App.mpType = 'app'

const app = new Vue({
    ...App
})
app.$mount() //挂载Vue实例
```

### 框架接口

页面通讯：

* uni.$emit
* uni.$on
* uni.$once
* uni.$off

### 自动化测试

[doc](https://uniapp.dcloud.io/collocation/auto/quick-start)

## 组件

```vue
// 示例
<template>
    <view>
        <p>{{information}}</p>
        <input type="text" v-model="information">
        <button :disabled="button_disable" @click="showInformation">{{button_text}}</button>
        <view v-if="show_me">you see me</view>
    </view>
</template>
<script>
    export default {
        data() {
            return {
                "button_disable": false,
                "button_text": "点我显示信息",
                "information": "not input yet"
            }
        }
        methods: {
        	showInformation() {
                console.log(information);
            }
    	}
    }
</script>
```

### 基础组件：

| 视觉容器           |      |
| ------------------ | ---- |
| 组件名             | 说明 |
| view               |      |
| scroll-view        |      |
| swiper             |      |
| match-media        |      |
| movable-area       |      |
| movable-view       |      |
| cover-view         |      |
| cover-image        |      |
| **基础组件**       |      |
| icon               |      |
| text               |      |
| rich-text          |      |
| progress           |      |
| **表单组件**       |      |
| button             |      |
| checkbox           |      |
| editor             |      |
| form               |      |
| input              |      |
| label              |      |
| picker             |      |
| picker-view        |      |
| radio              |      |
| slider             |      |
| switch             |      |
| textarea           |      |
| **路由与页面跳转** |      |
| navigator          |      |
| **媒体组件**       |      |
| audio              |      |
| camera             |      |
| image              |      |
| video              |      |
| live-player        |      |
| live-pusher        |      |
| **地图**           |      |
| map                |      |
| **画布**           |      |
| canvas             |      |
| **webview**        |      |
| webview            |      |
| **广告**           |      |
| ad                 |      |
| ad-draw            |      |
| **页面属性配置**   |      |
| custom-tab-bar     |      |
| navigation-bar     |      |
| page-meta          |      |
| **uniCloud**       |      |
| unicloud-db        |      |

### 扩展组件

[doc](https://uniapp.dcloud.io/component/README?id=uniui)

## API

