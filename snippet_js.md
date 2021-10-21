## 基础

#### getCookie

> 获取Cookie

```javascript
function getCookie(name, cookieName) {
    var cie = new RegExp('(?:^|; )' + encodeURIComponent(cookieName || name || '') + '=([^;]*)').exec(document.cookie)
    if (!cie) return null;
    if (!cookieName) {
        return cie ? decodeURIComponent(cie[1]) : null
    } else {
        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
        var r = cie[1].match(reg);
        if (r != null) return unescape(decodeURIComponent(r[2]));
        return null;
    }
}
```

#### loadJs

> 加载第三方jssdk

```typescript
function promiseFactory(id: string, src: string) {
    if (id && src) {
        return new Promise((resolve, reject) => {
            let tcBri = document.getElementById(id)
            if (!tcBri) {
                let _elem = document.createElement("script")
                _elem.setAttribute("id", id)
                _elem.setAttribute("type", "text/javascript")
                _elem.setAttribute("src", src);
                document.body.appendChild(_elem)
                _elem.onload = () => {
                    resolve(1)
                }
                _elem.onerror = () => {
                    reject()
                }
            } else {
                resolve(1)
            }
        })
    }
    return new Promise((resolve, reject) => {
        reject()
    })
}
```

#### getQueryString

> 获取链接参数

```javascript
function getQueryString(name) {
    let reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
    let r = window.location.search.substr(1).match(reg);
    if (r != null) return unescape(decodeURIComponent(r[2]));
    return "";
}
```

#### getHashString

> 获取hash后面的参数

```javascript
function getHashString(name) {
    let arr = (location.hash || "")
        .substr(location.hash.indexOf("?") + 1)
        .replace(/^\#/, "")
        .split("&");
    let params = {};
    for (let i = 0; i < arr.length; i++) {
        let data = arr[i].split("=");
        if (data.length == 2 && data[0] == name) {
            return data[1];
        }
    }
    return "";
}
```

#### 版本号比较

```javascript
/* *
 * version1 减 version2
 * 返回 1：version1 > version2，-1：version1 < version2，0：version1 == version2
 * */
function compareVersion(version1, version2) {
    let arr1 = version1.split('.')
    let arr2 = version2.split('.')
    let length1 = arr1.length
    let length2 = arr2.length
    let minlength = Math.min(length1, length2)
    let i = 0
    for (i; i < minlength; i++) {
        let a = parseInt(arr1[i])
        let b = parseInt(arr2[i])
        if (a > b) {
            return 1
        } else if (a < b) {
            return -1
        }
    }
    if (length1 > length2) {
        for (let j = i; j < length1; j++) {
            if (parseInt(arr1[j]) != 0) {
                return 1
            }
        }
        return 0
    } else if (length1 < length2) {
        for (let j = i; j < length2; j++) {
            if (parseInt(arr2[j]) != 0) {
                return -1
            }
        }
        return 0
    }
    return 0
}
```

#### queryStringify

> 对象转成字符串形式

```javascript
/* *
 * 返回 key1=value1&key2=value2&key3=value3
 * */
function queryStringify(obj, prefix = null) {
    var pairs = []
    for (var key in obj) {
        if (!Object.prototype.hasOwnProperty.call(obj, key)) {
            continue
        }

        var value = obj[key]
        var enkey = encodeURIComponent(key)
        var pair
        if (typeof value === 'object') {
            pair = this.queryStringify(value, prefix ? prefix + '[' + enkey + ']' : enkey)
        } else {
            pair = (prefix ? prefix + '[' + enkey + ']' : enkey) + '=' + encodeURIComponent(value)
        }
        pairs.push(pair)
    }
    return pairs.join('&')
}
```

## 网络请求

#### post

> 原生网络请求

```javascript
function post(url, data, header = {}, fn) {
    var xhr = new XMLHttpRequest();
    xhr.open("POST", url, true);
    xhr.setRequestHeader("Content-Type", "application/json");
    xhr.setRequestHeader("sectoken", 'xxxxxx'); // 统一设置headers
    if (header && Object.keys(header).length) {
        Object.keys(header).forEach(function(key) {
            xhr.setRequestHeader(key, header[key])
        });
    }
    xhr.onreadystatechange = function() {
        if (xhr.readyState == 4 && (xhr.status == 200 || xhr.status == 304)) {
            fn.call(this, JSON.parse(xhr.responseText));
        }
    };
    xhr.send(data);
}
```

## 界面

#### toast

> 原生网络请求

```javascript
function toast(opt = {}){
    let m = document.createElement('div');
    opt.duration = opt.duration ? opt.duration : 1500
    m.innerHTML = opt.txt;
    m.style.cssText = "width: 60%;min-width: 150px;opacity: 0.7;height: 30px;color: rgb(255, 255, 255);line-height: 30px;text-align: center;border-radius: 5px;position: fixed;top: 40%;left: 20%;z-index: 999999;background: rgb(0, 0, 0);font-size: 12px;overflow: hidden;";
    document.body.appendChild(m);
    setTimeout(() => {
        let d = 0.5;
        m.style.webkitTransition = '-webkit-transform ' + d + 's ease-in, opacity ' + d + 's ease-in';
        m.style.opacity = '0';
        setTimeout(function () { document.body.removeChild(m) }, d * 1000);
    }, opt.duration);
}
```

#### alert

```javascript
function alert(opt = {}) {
    let m = document.createElement('div');
    let html = `<div style="position:fixed;top:25%;width:100%;max-width:640px;">
    <div style="width:80%;margin-left:10%;padding:2% 0 0;background:#fff;border-radius:8px;color:#000;">
    <h3 style="font-size:18px;text-align:center;">${opt.title}</h3>
    <p style="font-size:14px;line-height:14px;width:90%;margin:5% auto;text-align:center;">${opt.msg}</p>
    <div id="nativeAlert-btn" style="width:100%;height:40px;line-height:40px;border-top:1px solid #d4d4d4; color:#0383fe;text-align:center;">确定</div></div></div>`
    m.innerHTML = html;
    document.body.appendChild(m);
    let alertElm = document.getElementById("nativeAlert-btn");
    if (alertElm) {
        alertElm.onclick = () => {
            let d = 0.5;
            m.style.webkitTransition = '-webkit-transform ' + d + 's ease-in, opacity ' + d + 's ease-in';
            m.style.opacity = '0';
            setTimeout(function () { document.body.removeChild(m) }, d * 1000);
        }
    }
}
```