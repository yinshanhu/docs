## 基础

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

### randomString

> 生成指定长度的随机字符串

```javascript
/**
 * @param {len} number 字符串长度
 * @return {string} 随机字符串
 */
const randomString = (len) => {
　　len = len || 32;
    let $chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678';    /****默认去掉了容易混淆的字符oOLl,9gq,Vv,Uu,I1****/
    let maxPos = $chars.length,
        pwd = '';
　　for (let i = 0; i < len; i++) {
　　　　pwd += $chars.charAt(Math.floor(Math.random() * maxPos));
　　}
　　return pwd;
},
```

### sleep

> 线程阻塞毫秒数

```javascript
/**
 * @param ms
 */
const sleep = (ms) => {
    return new Promise(resolve => setTimeout(resolve, ms));
}
```

### supportWebp

> 当前环境是否支持webp

```javascript
const supportWebp = () => {
    if (!inBrowser) return false;
    let support = true;
    let d = document;
    try {
        let el = d.createElement('object');
        el.type = 'image/webp';
        el.style.visibility = 'hidden';
        el.innerHTML = '!';
        d.body.appendChild(el);
        support = !el.offsetWidth;
        d.body.removeChild(el);
    } catch (err) {
        support = false;
    }
    return support;
}
```

### query2json

> query参数转json

```javascript
const query2json = (str) => {
    let param = {}; // 存储最终JSON结果对象
    str.replace(/([^?&]+)=([^?&]+)/g, function (s, v, k) {
        param[v] = k;
        return k + '=' + v;
    });
    return param;
}
```

### json2query

> json转query链接

```javascript
const json2query = (json) => {
    return Object.keys(json).map((key) => {
        return encodeURIComponent(key) + "=" + encodeURIComponent(json[key]);
    }).join("&");
}
```

## 参数获取

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

### getQueryString

> 获取链接参数

```javascript
// 第一种（推荐）
function getUrlParam(url, name) {
    var match = RegExp('[?&]' + name + '=([^&]*)').exec(url);
    if (!match) {
        return "";
    }
    return decodeURIComponent(match[1].replace(/\+/g, ' '));
}
```

```javascript
// 第二种
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

### getScriptLinkParams

> 获得script标签链接上的参数

```javascript
const getScriptLinkParams = () => {
    let scripts = document.getElementsByTagName('script');
    let src = scripts[scripts.length - 1].src;
    let arg = src.indexOf('?') !== -1 ? src.split('?').pop() : '';

    let params = {}; // 解析参数并存储到 params 变量中
    arg.replace(/(\w+)(?:=([^&]*))?/g, (a, key, value) => {
        params[key] = value;
    });

    return params;
}
```

### appendParam

> 给参数里的链接附加参数

```javascript
// 例：/page/common/webview/index?src=https%3A%2F%2Fwwww.baidu.com&islogin=1&show=1
// 给src里的 https://www.baidu.com 附加参数，如附加v2=222 ， https://www.baidu.com?v2=222
const appendParam = (url, json) => {
    let src = this.getUrlParam(url, 'src'); //取出src参数的值，已decodeURIComponent
    Object.keys(json).forEach((key) => {
        src += `${src.indexOf('?') > -1 ? '&' : '?'}${key}=${decodeURIComponent(json[key])}`;
    })
    return url.replace(/[?&]src=([^&]*)/, `?src=${encodeURIComponent(src)}`); // 替换原src的值
}
```


## 数组操作

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

## 对象操作

### searchVal

> 对象中是否存在某值

```javascript
/**
 * 查找对象中第一个value，并返回路径上所有key值
 * @param {Object} object 对象: {a:{b:1},c:{d:{e:2},f:{g:{h:3,i:{j:4,k:5,m:6}}}}}
 * @param {String} value 要查到的value：4
 * @return {Arry} 查找到的路径：['c', 'f', 'g', 'i', 'j']
 */
const searchVal = (object, value) => {
    for (let key in object) {
        if (object[key] == value) return [key];
        if (typeof object[key] == "object") {
            let temp = searchVal(object[key], value);
            if (temp) return [key, temp].flat(); // flat() 方法，这个方法可以抹平一个数组。不管嵌套了多少的数组，都会展开成为一个无嵌套数组
        }
    }
}
```

### removeEmptyP

> 去除对象的空值属性

```javascript
/**
 * @param obj
 */
const removeEmptyP = (obj) => {
    Object.keys(obj).forEach(key => {
      if (obj[key] === null || obj[key] === '' || obj[key] === 'undefined') {
        delete obj[key];
      }
    });
  }
```

### deepCopy

> 对象深拷贝

```javascript
//对象深拷贝
const deepCopy = (obj) => {
    if (obj == null) { return null } 
    let result = Array.isArray(obj) ? [] : {}; 
    for (let key in obj) { 
        if (obj.hasOwnProperty(key)) { 
            if (typeof obj[key] === 'object') { 
                result[key] = deepCopy(obj[key]); // 如果是对象，再次调用该方法自身 
            } else {
                result[key] = obj[key]; 
            } 
        } 
    } 
    return result;
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

### getObjectVal

> 安全获取对象字段

```javascript
/**
 * 安全获取对象字段
 * @param {Object} obj 目标对象
 * @param {String} path 字段路径，支持 'a.b[0].c[d]'、'[a][b][c]'、'[a,b,c]'、'[a,b,c]'
 * @param {*} deflt 取不到时返回的默认值
 */
const getObjectVal = (obj, path, deflt) => {
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

### setObjectVal

> 安全设置对象字段

```javascript
/**
 * 安全设置对象字段
 * @param {Object} obj 目标对象
 * @param {String} path 字段路径，支持 'a.b[0].c[d]'、'[a][b][c]'、'[a,b,c]'、'[a,b,c]'
 * @param {*} value 设置的值
 */
const setObjectVal = (obj, path, value) => {
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

## 浮点计算

### plus

> 加 +

```javascript
const plus = (num1, num2) => {
    const num1Digits = (num1.toString().split('.')[1] || '').length; // 小数点后几位
    const num2Digits = (num2.toString().split('.')[1] || '').length; // 小数点后几位
    const baseNum = Math.pow(10, Math.max(num1Digits, num2Digits)); // 放大小数位最大的倍数
    return (multiply(num1, baseNum) + multiply(num2, baseNum)) / baseNum  // 调用乘法，放大，变成整数后相加，再还原。
}
```

### minus

> 减 -

```javascript
const minus = (num1, num2) => {
    const num1Digits = (num1.toString().split('.')[1] || '').length; // 小数点后几位
    const num2Digits = (num2.toString().split('.')[1] || '').length; // 小数点后几位
    const baseNum = Math.pow(10, Math.max(num1Digits, num2Digits)); // 放大小数位最大的倍数
    return (multiply(num1, baseNum) - multiply(num2, baseNum)) / baseNum  // 调用乘法，放大，变成整数后相减，再还原。
}
```

### multiply

> 乘 *

```javascript
const multiply = (num1, num2) => {
    const num1Digits = (num1.toString().split('.')[1] || '').length; // 小数点后几位
    const num2Digits = (num2.toString().split('.')[1] || '').length; // 小数点后几位
    const baseNum =  Math.pow(10, num1Digits + num2Digits); // 与加、减不同，放大两小数位数之和的倍数
    return Number(num1String.replace('.', '')) * Number(num2String.replace('.', '')) / baseNum
}
```

### divide

> 除 /

```javascript
const divide = (num1, num2) => {
  const num1Digits = (num1.toString().split('.')[1] || '').length; // 小数点后几位
  const num2Digits = (num2.toString().split('.')[1] || '').length; // 小数点后几位
  const baseNum = Math.pow(10, num1Digits + num2Digits); // 与加、减不同，放大两小数位数之和的倍数
  let n1 = multiply(num1, baseNum) // 调用乘法，扩大
  let n2 = multiply(num2, baseNum) // 调用乘法，扩大
  return Number(n1) / Number(n2)
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

### countDown

> 60 秒倒计时（锁屏不停止）

```javascript

let time = 60; // 60 秒倒计时

const countDown = () => {
    let prevTime = Date.now();
    window._setTimeInterval = setInterval(() => {
        time = time - (((Date.now() - prevTime)/1000).toFixed(0));
        if(time <= 0) {
            clearInterval(_setTimeInterval);
            time = 60;
        }
        prevTime = Date.now();
        console.log("倒计时：" + time + " s")
    }, 1000);
}
```

> 日期倒计时（锁屏不停止，秒倒计时改装下即可）

```javascript

let endtime = '2100/01/01 00:00:00';  // 结束日期

const countDown = () => {
    let prevTime = Date.now();
    let time = ((new Date(endtime).getTime() - prevTime) / 1000 ).toFixed(0); // 计算距离结束之日的总秒数

    // 补全个位数的时间，如04天01时05分02秒
    function complete(str) {
        if (str < 10) {
            return `${str}`.padStart(2, "0");
        }
        return str;
    }

    window._setTimeInterval = setInterval(() => {
        time = time - (((Date.now() - prevTime) / 1000).toFixed(0));
        if(time <= 0) {
            clearInterval(_setTimeInterval);

            console.log("抢购进行中...");
        }
        prevTime = Date.now();

        //总秒数
        let totalSeconds = time,
            //天数
            days = Math.floor(totalSeconds / (60 * 60 * 24)),
            //取模（余数）
            modulo = totalSeconds % (60 * 60 * 24),
            //小时数
            hours = Math.floor(modulo / (60 * 60));

        modulo = modulo % (60 * 60);
        //分钟
        let minutes = Math.floor(modulo / 60),
            //秒
            seconds = modulo % 60;

        console.log(`剩余抢购:${complete(days)}天${complete(hours)}时${complete(minutes)}分${complete(seconds)}秒`);
    }, 1000);
}
```

>　列表倒计时，适用于只有一个计时器的场景（为了提升性能）

```javascript
/**
 * 只有一个计时器的倒计时
 * @param {Array} list 截止时间列表 {state:1, startTime:'2022-06-24 14:00', endTime:'2022-06-30 15:00'}  state：1，未开始  2，进行中  3，已结束
 * @param {String} serverTime 服务器时间
 */
const countDown = (list, serverTime) =>  {
    let prevTime = new Date(serverTime.replace(/-/g, '/')).getTime();
    let diff = prevTime - Date.now(); // 计算时间差
    let timeObj = {}; // 记录每一项的总秒数
    let timeHander = null; // 倒计时句柄，用于倒计时完成时清除句柄

    // 补全两位数
    const complete = (str) => {
        if (str < 10) {
            return `${str}`.padStart(2, "0");
        }
        return str;
    }

    // 求得 时-分-秒
    const setTheTime = (index) => {
        let totalSeconds = timeObj[index],
            modulo = totalSeconds % (60 * 60 * 24),
            hours = Math.floor(modulo / (60 * 60));
        modulo = modulo % (60 * 60);
        let minutes = Math.floor(modulo / 60),
            seconds = modulo % 60;

        // 设置倒计时字段
        this.setData({
            [`goodslist.items[${index}].timeDown`]: `${complete(hours)}:${complete(minutes)}:${complete(seconds)}`
        })
    }

    // 遍历列表
    // isInit: true,初始展示， false，倒计时展示 
    const forEachList = (isInit) => {
        list.forEach((item, index) => {
            if (item.state != 3) { // state != 3 不是已结束的数据项，也可以依据时间范围来过虑
                //  求得 当前剩多少秒：(活动截止时间 与 当前时间 做差)
                if (isInit) { // 初始展示
                    timeObj[index] = ((new Date((item.state == 1 ? item.startTime : item.endTime).replace(/-/g, '/')).getTime() - prevTime) / 1000).toFixed(0);
                } else {
                    // 倒计时展示 
                    timeObj[index] = timeObj[index] - ((((Date.now() + diff) - prevTime) / 1000).toFixed(0));
                }
                if (timeObj[index] >= 0) { // 还有秒数
                    setTheTime(index); // 求得 时-分-秒
                } else {
                    // 修改每一项的状态：将 未开始 变为 进行中，进行中 变为 已结束
                    list[index].state = item.state == 1 ? 2 : 3;
    
                    // 进行中的倒计时，取结束时间进行倒计时：
                    if (list[index].state == 2) {
                        timeObj[index] = ((new Date(item.endTime.replace(/-/g, '/')).getTime() - prevTime) / 1000).toFixed(0);
                        setTheTime(index);
                    }
                    // 更改状态：state：1，未开始  2，进行中  3，已结束
                    this.setData({
                        [`goodslist.items[${index}].state`]: list[index].state
                    })
                }
            }
        });
    }

    // 初始展示倒计时：
    forEachList(true);
    
    // 开始倒计时：
    timeHander = setInterval(() => {
        // 每1秒遍历一次列表：
        forEachList();

        // 都结束了就清除句柄：
        if (list.filter(f => new Date().getTime() <= new Date(f.endTime.replace(/-/g, '/')).getTime()).length == list.length) {
            clearInterval(timeHander); // 消除句柄
        }

        prevTime = Date.now() + diff; // 重设当前时间
        
    }, 1000);
}

```

### diffTime

> 当前日期时间

```javascript

/**
 * 计算两个日期时间相差的年数、月数、天数、小时数、分钟数、秒数
 * diffTime(开始时间,结束时间,[单位])，单位可以是 "y" 、"M"、"d"、"h"、"m"、"s"'
 * console.log(diffTime('2019-6-30 13:20:00', '2020-10-01 11:20:32', 's'))
 */ 
const diffTime = function (startTime, endTime, unit) {
	// 判断当前月天数
    function getDays (mouth, year) {
        let days = 30
        if (mouth === 2) {
            days = (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0 ? 29 : 28
        } else if (mouth === 1 || mouth === 3 || mouth === 5 || mouth === 7 || mouth === 8 || mouth === 10 || mouth === 12) {
            days = 31
        }
        return days
    }
    const start = new Date(startTime)
    const end = new Date(endTime)
    // 计算时间戳的差
    const diffValue = end - start
    // 获取年
    const startYear = start.getFullYear()
    const endYear = end.getFullYear()
    // 获取月
    const startMouth = start.getMonth() + 1
    const endMouth = end.getMonth() + 1
    // 获取日
    const startDay = start.getDate()
    const endDay = end.getDate()
    // 获取小时
    const startHours = start.getHours()
    const endHours = end.getHours()
    // 获取分
    const startMinutes = start.getMinutes()
    const endMinutes = end.getMinutes()
    // 获取秒
    const startSeconds = start.getSeconds()
    const endSeconds = end.getSeconds()

    if (unit === 'y' || unit === 'M') {
        // 相差年份月份
        const diffYear = endYear - startYear
        // 获取当前月天数
        const startDays = getDays(startMouth, startYear)
        const endDays = getDays(endMouth, endYear)
        const diffStartMouth = (startDays - (startDay + ((startHours * 60 + startMinutes + startSeconds / 60) / 60 / 24) - 1)) / startDays
        const diffEndMouth = (endDay + ((endHours * 60 + endMinutes + endSeconds / 60) / 60 / 24) - 1) / endDays
        const diffMouth = diffStartMouth + diffEndMouth + (12 - startMouth - 1) + endMouth + (diffYear - 1) * 12
        if (unit === 'y') {
            return Math.floor(diffMouth / 12 * 100) / 100
        } else {
            return diffMouth
        }
    } else if (unit === 'd') {
        const d = parseInt(diffValue / 1000 / 60 / 60 / 24)
        return d
    } else if (unit === 'h') {
        const h = parseInt(diffValue / 1000 / 60 / 60)
        return h
    } else if (unit === 'm') {
        const m = parseInt(diffValue / 1000 / 60)
        return m
    } else if (unit === 's') {
        const s = parseInt(diffValue / 1000)
        return s
    } else {
        console.log('请输入正确的单位')
    }
}
```

## 缓存

> 过期缓存

```javascript
class storage {
    constructor() {
        this.source = window.localStorage;
        this.initClear();
    }

    /**
     * 初始化清除已经过期的数据
     */
    initClear() {
        // const reg = new RegExp("__expires__");
        const reg = /__expires__/;
        let data = this.source;
        let list = Object.keys(data);
        if (list.length > 0) {
            list.forEach((key) => {
                // 如果为非包含__expires__键名的key进行判断清除过期的
                if (!reg.test(key)) {
                    let now = Date.now();
                    let expires = data[`${key}__expires__`];
                    if (now >= expires) {
                        this.remove(key);
                    }
                }
            })
        };
    }

    /**
     * set 存储方法
     * @ param {String} 	key 键名
     * @ param {String} 	value 键值
     * @ param {String} 	expired 过期时间，以分钟为单位，非必须。（不传则为永久有效）
     */
    set(key, value, expired) {
        let source = this.source;
        source[key] = JSON.stringify(value);
        if (expired !== undefined) {
            // // 过期时间单位为天
            // source[`${key}__expires__`] = Date.now() + 1000 * 60 * 60 * 24 * expired
            // // 过期时间单位为小时
            // source[`${key}__expires__`] = Date.now() + 1000 * 60 * 60 * expired
            // 过期时间单位为分钟
            source[`${key}__expires__`] = Date.now() + 1000 * 60 * expired;
        };
    }

    /**
     * get 获取方法
     * @ param {String} 	key 键名
     * @ return  如果没过期则返回数据，否则返回null
     */
    get(key) {
        const source = this.source;
        const expires = source[`${key}__expires__`];
        // 获取前把已经过期的数据删除掉
        if (expires) {
            const now = Date.now();
            if (now >= expires) {
                this.remove(key);
                return null;
            }
        }
        // 获取数据
        const value = source[key] ? JSON.parse(source[key]) : null;
        return value;
    }

    /**
     * remove 删除方法
     * @ param {String} 	key 键名  非必须 （不传则删除所有）
     */
    remove(key) {
        if (key) {
            // localStorage自带的removeItem()方法
            this.source.removeItem(key);
            this.source.removeItem(`${key}__expires__`);
            // delete data[key];
            // delete data[`${key}__expires__`];
        } else {
            // 清除所有，localStorage自带的clear()方法
            this.source.clear();
        }
    }
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

## 文件上传

### 表单实现

```javascript
var FormData = require('form-data');
var fs = require('fs');

var form = new FormData();
form.append('my_file', fs.createReadStream('/foo/bar.jpg'));
form.submit('example.org/upload', function(err, res) {
  console.log(res.statusCode);
});
```

### 模拟文件流

```javascript
const string2fileStream = require('string-to-file-stream');
const FormData = require('form-data');

const form = new FormData();
const flieContent = 'Oh, my great data!';
form.append('my_file', string2fileStream(flieContent, { path: 'no-this-file.txt' }));
form.submit('example.org/upload', function(err, res) {
  console.log(res.statusCode);
});
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