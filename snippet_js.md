## 基础

### getCookie

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

### loadJs

> 加载第三方jssdk

```javascript
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

### getQueryString

> 获取链接参数

```javascript
function getQueryString(name) {
    let reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
    let r = window.location.search.substr(1).match(reg);
    if (r != null) return unescape(decodeURIComponent(r[2]));
    return "";
}
```

### getHashString

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

### compareVersion

> 字符串版本号比较

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

### queryStringify

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

### get

> 安全获取对象字段

```javascript
/**
 * 安全获取对象字段
 * @param {Object} obj 目标对象
 * @param {String} path 字段路径，支持 'a.b[0].c[d]'、'[a][b][c]'、'[a,b,c]'、'[a,b,c]'
 * @param {*} deflt 取不到时返回的默认值
 */
const get = (obj, path, deflt) => {
    if (obj == null) return deflt

    let keys = JSON.stringify(path).match(/[^.,\s\[\]\'\"]+/g)

    if (!keys) return deflt

    let ret

    for (let i = 0, j = keys.length; i < j; i++) {
        ret = obj[keys[i]]

        if (ret == null) return deflt

        obj = ret
    }

    return ret
}
```

### set

> 安全设置对象字段

```javascript
/**
 * 安全设置对象字段
 * @param {Object} obj 目标对象
 * @param {String} path 字段路径，支持 'a.b[0].c[d]'、'[a][b][c]'、'[a,b,c]'、'[a,b,c]'
 * @param {*} value 设置的值
 */
const set = (obj, path, value) => {
    if (obj == null || path == null) return obj

    // eslint-disable-next-line no-useless-escape
    let keys = JSON.stringify(path).match(/[^.,\s\[\]\'\"]+/g)

    if (!keys) return obj

    let _obj = obj

    for (let i = 0, j = keys.length; i < j; i++) {
        let temp = _obj[keys[i]]

        if (temp == null) {
            if (i + 1 === j) {
                _obj[keys[i]] = value

                break
            }

            _obj[keys[i]] = /\d+/.test(keys[i]) ? [] : {}
        } else if (i + 1 === j) {
            _obj[keys[i]] = value
        }

        _obj = _obj[keys[i]]
    }

    return obj
}
```

### debounce

> 函数防抖

```javascript
/**
 * 函数防抖
 * @param {*} fn 回调函数
 * @param {Number} interval 间隔
 */
const debounce = (fn, interval) => {
    let timeid

    return function (...args) {
        if (timeid) {
            clearTimeout(timeid)
        }

        timeid = setTimeout(() => {
            fn.apply(this, args)
        }, interval)
    }
}
```

### throttle

> 函数节流

```javascript
/**
 * 函数节流
 * @param {*} func 回调函数
 * @param {Number} wait 间隔
 * @param {Object} options 间隔
 */
const throttle = (func, wait, options) => {
    /* options的默认值
     *  表示首次调用返回值方法时，会马上调用func；否则仅会记录当前时刻，当第二次调用的时间间隔超过wait时，才调用func。
     *  options.leading = true;
     * 表示当调用方法时，未到达wait指定的时间间隔，则启动计时器延迟调用func函数，若后续在既未达到wait指定的时间间隔和func函数又未被调用的情况下调用返回值方法，则被调用请求将被丢弃。
     *  options.trailing = true;
     * 注意：当options.trailing = false时，效果与上面的简单实现效果相同
     */
    var context, args, result
    var timeout = null
    var previous = 0
    if (!options) options = {}
    var later = function () {
        previous = options.leading === false ? 0 : Date.now()
        timeout = null
        result = func.apply(context, args)
        if (!timeout) context = args = null
    }
    return function () {
        var now = Date.now()
        if (!previous && options.leading === false) previous = now
        // 计算剩余时间
        var remaining = wait - (now - previous)
        context = this
        args = arguments
        // 当到达wait指定的时间间隔，则调用func函数
        // 精彩之处：按理来说remaining <= 0已经足够证明已经到达wait的时间间隔，但这里还考虑到假如客户端修改了系统时间则马上执行func函数。
        if (remaining <= 0 || remaining > wait) {
            // 由于setTimeout存在最小时间精度问题，因此会存在到达wait的时间间隔，但之前设置的setTimeout操作还没被执行，因此为保险起见，这里先清理setTimeout操作
            if (timeout) {
                clearTimeout(timeout)
                timeout = null
            }
            previous = now
            result = func.apply(context, args)
            if (!timeout) context = args = null
        } else if (!timeout && options.trailing !== false) {
            // options.trailing=true时，延时执行func函数
            timeout = setTimeout(later, remaining)
        }
        return result
    }
}
```

### isCoincide

> 判断两区间是否有交集（重合）

```javascript
/**
 * 函数节流
 * @param {Array} section1 区间1，示例：[12,56]
 * @param {Array} section2 区间2，示例：[32,87]
 */
const isCoincide = (section1, section2) => {
    let maxStart = [section1[0], section2[0]],
        minEnd = [section1[1], section2[1]];
    if (Math.max(...maxStart) <= Math.min(...minEnd)) {
        return true; //有交集
    }
    return false;
}
```

### deepCopy

> 对象深拷贝

```javascript
//对象深拷贝
const deepCopy = (obj) => {
    var res = {};
    for (var key in obj) {
        res[key] = obj[key];
    }
    return res;
}
```

### uniqueItem

> 对象数组去重

```javascript
/**
 * 对象数组去重
 * @param {Array} objArray 对象数组
 * @param {String} byKey 去重对比key
 */
const uniqueItem = (objArray, byKey) => {
    let hash = {};
    objArray = objArray.reduce( (item, next) => {
        hash[next[byKey]] ? '' : hash[next[byKey]] = true && item.push(next);
        return item;
    }, []);
    return objArray;
}
```

## 时间

### currentDateTime

> 当前日期时间

```javascript
currentDateTime() {
    let now = time ? new Date(time) : new Date(),
        year = now.getFullYear(), //年
        month = now.getMonth() + 1, //月
        day = now.getDate(), //日
        hh = now.getHours(), //时
        mm = now.getMinutes(), //分
        ss = now.getSeconds(), //秒
        clock = year + "/";

    if (month < 10) clock += "0";
    clock += month + "/";
    if (day < 10) clock += "0";
    clock += day + " ";
    if (hh < 10) clock += "0";
    clock += hh + ":";
    if (mm < 10) clock += "0";
    clock += mm + ":";
    if (ss < 10) clock += "0";
    clock += ss;

    return clock;
}
```

## 网络请求

### post

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

### toast

> taost弹窗

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

### alert

> 确认弹窗

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

## 设计模式

### decorate

> 装饰者

```javascript
let fuc = function handle() {

    // 业务代码
    // ... ...
    console.log('业务代码')
}

// 声明
fuc.decorate = function (beforefn, afterfn) {
    const _self = this;
    return function () {
        beforefn.apply(this, arguments);
        try {
            _self.apply(this, arguments);
        } catch (error) {
            // 一般会记日志
            throw error;
        }
        afterfn.apply(this, arguments);
    };
};
const handleDecorate = fuc.decorate(() => {

    // fuc 方法执行前需要执行的代码：
    // ......

}, () => {

    // fuc 方法执行后需要执行的代码：
    // ......

});

// （上下文）参数
const CONTEXT = {
    param1: '',
    param2: ''
}
handleDecorate(CONTEXT);
```