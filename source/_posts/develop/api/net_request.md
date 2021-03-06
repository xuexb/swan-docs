---
title: 请求
header: develop
nav: api
sidebar: net_request
---

## request

**解释：**发起网络请求

**方法参数：**Object

#### **Object 参数说明：**

|参数名 |类型  |必填 | 默认值 |说明|
|---- | ---- | ---- | ----|----|
|url |String | 是   |-|    开发者服务器接口地址|
|data  |  Object/String  | 否  |-| 请求的参数|
|header | Object | 否    |-|   设置请求的 header，header 中不能设置 Referer。|
|method | String | 否  | GET （大写）|有效值：OPTIONS, GET, HEAD, POST, PUT, DELETE 。|
|dataType   | String | 否  | json  | 有效值：string,json。 如果设为 json，会尝试对返回的数据做一次 JSON.parse 。|
|responseType   | String | 否  | text  | 设置响应的数据类型, 合法值：text、arraybuffer。|
|success |Function    |否 |-|      收到开发者服务成功返回的回调函数。|
|fail |   Function|    否  |-|     接口调用失败的回调函数。|
|complete  |  Function  |  否   |-|    接口调用结束的回调函数（调用成功、失败都会执行）。|


#### **success 返回参数说明：**


|参数 | 类型 | 说明  |
|---- | ---- | ---- |
|data  |  Object/String  | 开发者服务器返回的数据|
|statusCode | Number | 开发者服务器返回的 HTTP 状态码|
|header | Object | 开发者服务器返回的 HTTP Response Header|

**data 数据说明：**

最终发送给服务器的数据都是 String 类型，如果传入的 data 不是 String 类型，会被转换成 String 。转换规则如下：
1、对于 GET 方法的数据，会将数据转换成 query string（encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）；
2、对于 POST 方法且 header['content-type'] 为 application/json 的数据，会对数据进行 JSON 序列化；
3、对于 POST 方法且 header['content-type'] 为 application/x-www-form-urlencoded 的数据，会将数据转换成 query string （encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）。

**示例 1**
<a href="swanide://fragment/ea0f77c0b00c111f9a4ed3f269412d861540397106" title="在开发者工具中预览效果" target="_blank">在开发者工具中预览效果</a>
```js
swan.request({
    url: 'https://smartprogram.baidu.com/xxx', // 仅为示例，并非真实的接口地址
    method: 'GET',
    dataType: 'json',
    data: {
        key: 'value'
    },
    header: {
        'content-type': 'application/json' // 默认值
    },
    success: function (res) {
        console.log(res.data);
    },
    fail: function (err) {
        console.log('错误码：' + err.errCode);
        console.log('错误信息：' + err.errMsg);
    }
});
```

**返回值：**

返回一个 requestTask 对象，通过 requestTask，可中断请求任务。

**requestTask 对象的方法列表：**

|方法 | 参数 | 说明  |
|---- | ---- | ---- |
|abort  |      | 中断请求任务 |

**示例 2**

```js
const requestTask = swan.request({
    url: 'test.php', //仅为示例，并非真实的接口地址
    data: {
        x: '' ,
        y: ''
    },
    header: {
        'content-type': 'application/json'
    },
    success: function(res) {
        console.log(res.data)
    }
});
//取消请求任务
requestTask.abort();
```

**说明**
*  content-type 默认为 'application/json'；
*  url 中不能有端口。
