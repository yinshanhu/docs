## 文档地址

[element](https://element.eleme.io/#/zh-CN/component/layout)

## 选择时间限制

```html
<el-date-picker
    v-model="datetime"
    type="datetimerange"
    unlink-panels
    range-separator="至"
    start-placeholder="开始日期"
    end-placeholder="结束日期"
    value-format="yyyy-MM-dd HH:mm:ss"
    :default-time="['00:00:00', '23:59:59']"
    :picker-options="pickerOptions"
>
</el-date-picker>
```

```javascript
data() {
    return {
        pickerOptions: {
            disabledDate(time) {
                // 只能选择当前时间以后的
                return time.getTime() < Date.now() - 8.64e7;

                // 只能选择当前时间之前的
                return time.getTime() > Date.now() - 8.64e6;

                //减去一天的时间代表可以选择同一天;
                return time.getTime() < new Date(this.value1).getTime()- 1*24*60*60*1000;
            }
        }
    }
}
```

## 使用原生事件

> 如果需要使用vue的事件（非element事件），事件后面须加 `.native`

```html
<el-button @dblclick.native="setTag(tag)"><el-button>
```
