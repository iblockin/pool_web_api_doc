## Credentials 

用户中心颁发的有效证书（token），有效时间2小时，过期后请重新获取。
该鉴权方式只支持部分高级接口，不支持的部分请要使用read token

### 使用资格
该功能仅对部分用户开放，如需使用请联系客服。

### 获取证书 token
1. 准备我方提供的client_id,client_secret
1. [post] https://account.blockin.com/oauth/v1/token
参数client_id，client_secret，grant_type
1. 其中grant_type传固定值"client_credentials"

会得到类似返回
```json
[
     "access_token" => "eyJ0e***************************kjerg",
     "expires_in" => 7200,
     "token_type" => "bearer",
     "scope" => "test",
]
```
access_token就是我们需要的token，expires_in是有效期


## 可用API
api的相关信息如返回的格式，携带token的方式请同意参考
[api.md](./api.md)

### 创建子账户
**请求参数**

|字段|类型|必须|备注|
|---|---|---|---|
|name|string|true|子账户名称|
|coin_type|string|true|默认的币种|
|payment_id|string|false|xmr、etn的支付id|


**请求URL**

```
POST https://api-prod.poolin.com/api/public/v2/credentials/subaccount/create
```

**返回结果**

``` json
{
    "err_no": 0,
    "data": {
        "puid": 9999888,
        "name": "111ttbtc",
        "default_coin_type": "zec",
        "coins": {},
        "created_at": 1561537835
    }
}

```
如果err_no不为0，是以下的数字的话，请对应错误原因修改。

|错误码|原因|
|---|---|
|100|目前最多允许9999个子账户，更多需要单独申请|
|101|子账户名称已经存在了|
|102|24小时内只能创建100个子账户，超过频率会报错，需要更高频率单独申请|
|501，505|地址验证失败|
|504|payment_id验证失败|
|507|意外的错误|

**返回字段说明**

|返回值字段|说明|
|---|---|
|puid|子账户id|
|name|名称|
|default_coin_type|默认币种|

### 创建观察者、观察者token
**请求参数**

|字段|类型|必须|备注|
|---|---|---|---|
|name|string|true|子账户名称|
|coin_types|array|true|观察者能看到的币种,如['btc']|
|puid|int|true|子账户id|


**请求URL**

```
POST https://api-prod.poolin.com/api/public/v2/credentials/watcher/token/create
```

**返回结果**

``` json
{
    "err_no": 0,
    "data": {
        "token": "wowEuP*************EiL",
        "puid": 9999888,
        "coin_types": "btc",
        "regions": "*",
        "name": "ttttt",
        "created_at": 1561538162,
        "expired_at": 1577349362,
        "subaccount_name": "111ttbtc"
    }
}

```
如果err_no不为0，是以下的数字的话，请对应错误原因修改。

|错误码|原因|
|---|---|
|100|目前最多50个生效的观察者|


**返回字段说明**

|返回值字段|说明|
|---|---|
|token|观察者token|
|puid|子账户id|
|coin_types|支持的币种信息|
|name|观察者名称|
|created_at|创建时间|
|expired_at|过期时间|
|subaccount_name|子账户名称|


### 更新子账户地址
**请求参数**

|字段|类型|必须|备注|
|---|---|---|---|
|address|int|true|要更新的地址|
|coin_type|string|true|是要更新哪一个币种的地址|


**请求URL**

```
POST https://api-prod.poolin.com/api/public/v2/credentials/subaccount/{puid}/update/address
```

**返回结果**

``` json
{
    "err_no": 0,
    "data": null
}

```
如果err_no不为0，是以下的数字的话，请对应错误原因修改。

|错误码|原因|
|---|---|
|100|子账户不存在|
|501，505|地址验证失败|
|504|payment_id验证失败|


**返回字段说明**

|返回值字段|说明|
|---|---|
