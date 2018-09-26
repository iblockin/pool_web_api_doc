# pool_web_api_doc
## API

### 获取子账户列表

**请求参数**

|字段|必须|含义|备注|
|---|---|---|---|



**请求URL**

```
GET https://api-prod.poolin.com/api/public/v1/subaccount
```

**返回结果**

```
{
    err_no:0,
    data:[
         {
             "puid": 9002613,
             "name": "123123123123asdasd",
             "nickname": "",
             "default_coin_type": "xmr",
             "coins": {},
             "status": 1,
             "created_at": 1537425355
         },
        ......
}
```

**返回字段说明**

|返回值字段|说明|
|---|---|
|puid|子账户id|
|name|名称|
|nickname|昵称|
|default_coin_type|默认币种|
|status|账户状态（1：正常；0：隐藏）|


