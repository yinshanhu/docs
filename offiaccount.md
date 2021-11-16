## 微信公从号

> 网页开发者可借助微信高效地使用拍照、选图、语音、位置等手机系统的能力，同时可以直接使用微信分享、扫一扫、卡券、支付等微信特有的能力，为微信用户提供更优质的网页体验。

1. 微信网页开发说明文档：[官方文档](https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html)
2. 前奏（步骤一、二、三、四、五）忽略，仔细阅读官方文档。 

### 设置分享内容

```javascript
const wxShareConig = () => {
    var wxJs = document.createElement("script");
    wxJs.type = "text/javascript";
    wxJs.src = "//res.wx.qq.com/open/js/jweixin-1.6.0.js";
    document.getElementsByTagName("HEAD")[0].appendChild(wxJs);
    wxJs.onload = function() {
        $.ajax({
            type: "GET",
            url: 'https://xxx/xxx/xxx?url=' + encodeURIComponent(location.href.split('#')[0]), // 签名算法获取接口地址
            success: function(data) {
                var d = JSON.parse(data);
                wx.config({
                    debug: false, // 调试开关
                    appId: d.AppId,//必填，公众号的唯一标识
                    timestamp: d.TimeStamp,// 必填，生成签名的时间戳
                    nonceStr: d.NonceStr,// 必填，生成签名的随机串
                    signature: d.Signature,// 必填，签名
                    jsApiList: [//必填，需要使用的JS接口列表
                        'checkJsApi',
                        'hideMenuItems',//隐藏菜单项api
                        'onMenuShareAppMessage',//（即将废弃）
                        'onMenuShareTimeline',//（即将废弃）
                        'updateAppMessageShareData',// 设置分享内容（好友分享）
                        'updateTimelineShareData'// 设置分享内容（朋友圈分享）
                    ],
                    //openTagList: ['wx-open-launch-app'] //开放标签，唤起第三方app使用
                });
                wx.ready(function() {
                    wx.checkJsApi({
                        jsApiList: [
                            'hideMenuItems',
                            'onMenuShareAppMessage',
                            'onMenuShareTimeline',
                            'updateAppMessageShareData',
                            'updateTimelineShareData'
                        ], // 需要检测的JS接口列表，所有JS接口列表见附录2,
                        success: function(res) {
                            if (res.checkResult.hideMenuItems || res.checkResult.hideMenuItems) {
                                wx.hideMenuItems({
                                    menuList: [ // 隐藏以下功能
                                        // 'menuItem:copyUrl',
                                        // 'menuItem:openWithSafari',
                                        "menuItem:share:email",
                                        "menuItem:share:qq",
                                        "menuItem:share:weiboApp",
                                        "menuItem:share:facebook",
                                        "menuItem:share:QZone"
                                    ]
                                });
                            }
                            if (res.checkResult.updateAppMessageShareData || res.checkResult.onMenuShareAppMessage) {
                                wx.updateAppMessageShareData({
                                    title: 'XXX', // 分享标题
                                    desc: 'XXXXXX', // 分享描述
                                    link: 'https://m.xxx.com/xxx/xx/xx/xx', // 分享链接
                                    imgUrl: 'https://xxxx/xxx/xx/xxxx.png', // 分享图标
                                    success: function(data) {
                                        // 设置成功
                                        console.log('updateAppMessageShareData success:', data);
                                    },
                                    fail: function(error) {
                                        console.log('updateAppMessageShareData error:', error);
                                    }
                                });
                            }

                            if (res.checkResult.updateTimelineShareData || res.checkResult.onMenuShareTimeline) {
                                wx.updateTimelineShareData({
                                    title: 'XXX', // 分享标题
                                    link: 'https://m.xxx.com/xxx/xx/xx/xx', // 分享链接
                                    imgUrl: 'https://xxxx/xxx/xx/xxxx.png', // 分享图标
                                    success: function(data) {
                                        // 设置成功
                                        console.log('updateTimelineShareData success:', data);
                                    },
                                    fail: function(error) {
                                        console.log('updateTimelineShareData error:', error);
                                    }
                                });
                            }
                        }
                    });
                });
            }
        })
    };
},
```

