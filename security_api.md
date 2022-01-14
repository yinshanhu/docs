## 前端请求设置签名

- 签名生成

```javascript
import md5 from 'js-md5';
export default {
	//请求头header配置，data是请求的参数
    getBaseHeaders(data) {
        let dataSort = this.sortByKey(data);
        let dataStr = this.strJoin(dataSort);
        let urlStr = window.location.origin;
        let timestamp = new Date().getTime();
        let nonceStr = Math.random().toString(36).substr(2);
        //生成前端签名
        let signature = md5(dataStr + '&Timestamp=' + timestamp + '&nonce=' + nonceStr + '&url=' + urlStr);
        let postBaseHeaders = {
            timestamp: timestamp,
            nonce: nonceStr,
            signature: signature,
        };
        return postBaseHeaders;
    },
    //参数排序
    sortByKey(obj) {
        const newkey = Object.keys(obj).sort();
        let newObj = {};
        for (let i = 0; i < newkey.length; i++) {
            newObj[newkey[i]] = obj[newkey[i]];
        }
        return newObj;
    },
    //参数序列化
    strJoin(resData) {
        let i = 0;
        let str = '';
        let strJoint = '';
        let arr = Object.keys(resData);
        for (let key in resData) {
            str = key + '=' + resData[key];
            i++;
            if (i < arr.length) {
                strJoint = strJoint + str + '&';
            } else if (i == arr.length) {
                strJoint = strJoint + str;
            }
        }
        console.log(strJoint);
        return strJoint;
    },
};
```

- axios应用
  
```javascript
/* 请求拦截 */
axios.interceptors.request.use(
    (config: any) => {
		let headers = getBaseHeaders({name: 'test'});
		config = Object.assign(config, { headers });	   
		return config;
    },
    (error: any) => {
        return Promise.reject(error);
    }
);
```

## 后端nodejs校验签名

```javascript
const Koa = require('koa');
const bodyParser = require('koa-bodyparser');
const md5 = require('js-md5');

const app = new Koa();
app.use(bodyParser());

app.use(async (ctx) => {
    let url = ctx.headers.origin; //前端地址
    let timestamp = ctx.headers.timestamp; //时间戳
    let nonce = ctx.headers.nonce; //随机字符串
    let signature = ctx.headers.signature; //前端签名
    let postData = ctx.request.body; //参数
	
	//参数以key字典顺序(ASCII值大小)升序排序
    let dataSort = sortByKey(postData); 

	//参数序列化拼接
    let dataStr = strJoin(dataSort); 
    
    //md5生成后端签名
    let newSignature = md5(`${dataStr}&Timestamp=${timestamp}&nonce=${nonce}&url=${url}`); 

    //签名校验
    if (signature === newSignature) {
        ctx.body = {
            code: 200,
            msg: '签名校验成功',
            data: 'true',
        };
    } else {
        ctx.body = {
            code: 4000,
            msg: '签名校验失败',
            data: 'false',
        };
    }

    function sortByKey(obj) {
        const newkey = Object.keys(obj).sort();
        var newObj = {};
        for (var i = 0; i < newkey.length; i++) {
            newObj[newkey[i]] = obj[newkey[i]];
        }
        return newObj;
    }

    function strJoin(resData) {
        let i = 0;
        let str = '';
        let strJoint = '';
        var arr = Object.keys(resData);
        for (let key in resData) {
            str = key + '=' + resData[key];
            i++;
            if (i < arr.length) {
                strJoint = strJoint + str + '&';
            } else if (i == arr.length) {
                strJoint = strJoint + str;
            }
        }
        return strJoint;
    }
});

const hostName = '127.0.0.1'; //IP
const port = 80; //端口
app.listen(port, hostName, () => {
    console.log(`服务运行在http://${hostName}:${port}`);
});

console.log('成功启动');
```