## webview

### 内嵌h5与miniprogram通讯

#### 一、error event 和 hashchange

```javascript
// 内嵌h5页面

// 一、发：h5 -> 小程序
if (typeof window.WeixinJSBridge == "undefined" || !window.WeixinJSBridge.invoke) {
    if ( window.document.addEventListener ) {
        window.document.addEventListener('WeixinJSBridgeReady', _onBridgeReady, false);
    } else if (window.document.attachEvent) {
        window.document.attachEvent('WeixinJSBridgeReady', _onBridgeReady);
        window.document.attachEvent('onWeixinJSBridgeReady', _onBridgeReady);
    }
} else {
    _onBridgeReady();
}

function _onBridgeReady() {
    window.WeixinJSBridge.invoke("invokeMiniProgramAPI", {
        name: 'error', // 触发error事件
        arg: {url: 'http://xx.xx.com?'+encodeURIComponent(data)} // data：需要传的数据
    }, function(res) {});
}

// 二、收：h5 <- 小程序
window.addEventListener("hashchange", function (res) {
    console.log(res)
}, false);
```

```html
<!-- 小程序：webview.wxml -->
<web-view binderror="loaderror"></web-view>
```

```javascript
// 小程序：webview.js

// 一、收：小程序 <- h5
loaderror(res){
 try {
    let param = decodeURIComponent((res.detail.url || '').split('xx.xx.com?')[1])
    let data = JSON.parse(param)

    console.log(data)
 } catch (e) { }
}

// 二、发：小程序 -> h5

// 参数附加到当前webview的链接上

```

#### 二、websocket

## 原生开发

#### 动态修改数组中的某一值

```javascript

// data中的数据：
data:{
  goodsList: [
    {
      _id:1,
      title: "我是标题111",
      num:0      // 需要改变的数据
    }, {
      _id:2,
      title: "我是标题222",
      num: 1      // 需要改变的数据
    }
  ]
}

// 修改数组中的某一值：
add:function(e) {
    let index = e.currentTarget.dataset.index  // 数组的索引
    let temp = `goodsList[${index}].num`  // 获取 goodsList[index].num
    this.setData({
      [temp]: this.data.goodsList[index].num + 1 //核心代码
    })
}
```
