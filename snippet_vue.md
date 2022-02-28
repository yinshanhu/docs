## 插件

> vue第三方插件使用细节code

### vue-lazyload

1. 局部使用

```js
// main.js
import VueLazyload from 'vue-lazyload'
Vue.use(VueLazyload)
```
   
```html
<template>
    <img v-lazy="getLazyObj(`https://cn.bing.com//th?id=OHR.${scope.data.id}_UHD.jpg&pid=hp&w=720&h=1280&rs=1&c=4&r=0`)" :key="scope.data.id" />
</template>
```

```js
methods: {
    getLazyObj(src) {
      return {
        src,
        error: "//xxx.xx.com/default.png",
        loading: "//xxx.xx.com/default.png",
      };
    },
}
```


2. 全局使用
   
```js
// main.js
import VueLazyload from 'vue-lazyload'

Vue.use(VueLazyload)

// 全局使用
Vue.use(VueLazyload, {
  preLoad: 1.3, //（预加载高度比例），默认1.3
  error: 'dist/error.png', //（图片路径错误时加载图片），默认data-src
  loading: 'dist/loading.gif', //（预加载图片），默认data-src
  attempt: 1 //（尝试加载图片数量）,默认3
})
```

```html
<template>
    <!-- 图片路径绑定:src修改为v-lazy -->
    <img v-lazy="imgUrl" width="100%" height="400">

    <!-- 如果是背景图片添加v-lazy:background-image -->
    <div v-lazy:background-image="imgUrl"></div>
</template>
```

3. CSS state

- 在图片加载时，可以自定义图片加载的样式，有三个状态 `laoding` `error` `loaded`

```html
<style>
  img[lazy=loading] {
    /*your style here*/
  }
  img[lazy=error] {
    /*your style here*/
  }
  img[lazy=loaded] {
    /*your style here*/
  }
  
  /*
  or background-image
  */
  .yourclass[lazy=loading] {
    /*your style here*/
  }
  .yourclass[lazy=error] {
    /*your style here*/
  }
  .yourclass[lazy=loaded] {
    /*your style here*/
  }
</style>
```