## 插件

> vue第三方插件使用细节code

### vue-lazyload

1. 局部使用

```javascript
// main.js
import VueLazyload from 'vue-lazyload'
Vue.use(VueLazyload)
```
   
```html
<template>
    <img v-lazy="getLazyObj(`https://cn.bing.com//th?id=OHR.${scope.data.id}_UHD.jpg&pid=hp&w=720&h=1280&rs=1&c=4&r=0`)" :key="scope.data.id" />
</template>
```

```javascript
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

```javascript
// main.js
import VueLazyload from 'vue-lazyload'
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

## 组件

> 工作中常用到的组件

### 弹出层模态

> 从下往上抽屉效果

```vue
<template>
  <div class="pop">
    <div class="mask" :style="{ display: showModal ? 'block' : 'none' }" @click="closeModal"></div>
    <div class="container" :class="[showModal?'toggle':'']" :style="{backgroundColor: bgColor}">
      <header>
        <div class="close">
          <img src="../assets/icon/close.png" @click="closeModal"/>
        </div>
        <div class="pop-title">{{title}}</div>
        <div v-if="line" class="border_top"></div>
      </header>
      <main class="pop-content" :style="{padding:padding}">
        <slot></slot>
      </main>
      <footer>
        <slot name="footer"></slot>
      </footer>
    </div>
  </div>
</template>
<script setup>
import { ref, watch } from "vue";
let props = defineProps({
  bgColor: {
    type: String,
    default: '#ffffff',
  },
  line: {
    type: Boolean,
    default: false,
  },
  title: {
    type: String,
    default: '标题',
  },
  show: {
    type: Boolean,
    default: false,
  },
  padding: {
    type: String,
    default: '0 16px 30px',
  }
});

let showModal = ref(props.show);
const closeModal = () => {
  showModal.value = !showModal.value;
};
watch(() => props.show, () => {
    showModal.value = true
});

watch(showModal, (newVal) => {
  // 防止滚动穿透：
  if (newVal == true) {
    let cssStr = "overflow-y: hidden; height: 100%;";
    document.getElementsByTagName("html")[0].style.cssText = cssStr;
    document.body.style.cssText = cssStr;
  } else {
    let cssStr = "overflow-y: auto; height: auto;";
    document.getElementsByTagName("html")[0].style.cssText = cssStr;
    document.body.style.cssText = cssStr;
  }
});
</script>
<style scoped>
.pop {
  font-size: 13px;
}
.mask {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, .7);
  z-index: 1001;
  transition: display 0.3s;
  -webkit-tap-highlight-color: transparent;
  overflow:hidden;
}
.container {
  border-radius: 10px 10px 0 0;
  display: block;
  position: fixed;
  min-height: 75%;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1002;
  background-color: #ffffff;
  -webkit-transition: -webkit-transform 0.3s;
  transition: -webkit-transform 0.3s;
  transition: transform 0.3s;
  transition: transform 0.3s, -webkit-transform 0.3s;
  -webkit-transform: translate(0, 100%);
  transform: translate(0, 100%);
}
.container.toggle{
    -webkit-transform: translate(0, 0);
    transform: translate(0, 0);
}
.container .close {
  text-align: right;
  
}
.container .close img {
  width: 12px;
  height: 12px;
  margin: 15px 15px 0 0;
}
.pop-content{
    overflow-x: hidden;
    overflow-y: auto;
    position: absolute;
    top: 5.076923em;
    left: 0;
    right: 0;
    bottom: 0;
    -webkit-overflow-scrolling: touch;
    margin-bottom: constant(safe-area-inset-bottom);
    margin-bottom: env(safe-area-inset-bottom);
}
.pop-title{
    padding-bottom: 7px;
    font-size: 20px;
    text-align: center;
    color: #333333;
}
</style>
```