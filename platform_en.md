## Platform 

A valid certificate (token) issued by the user center, valid for 2 hours. Please re-acquire after expiration.
This authentication method only supports some advanced interfaces. For unsupported parts, please use the read token.


### Qualification
This feature is only available to some users. Please contact customer service if you need to use it.


### 获取证书 token
1. Prepare the client_id and client_secret provided by us.
2. [post] https://account.blockin.com/oauth/v1/token parameter ```client_id，client_secret，grant_type，scopes```
3. grant_type fixed value of ```client_credentials```
4. The scopes currently have four subaccount-create, watcher-create, address-update, and cloud, corresponding to specific interface permissions. Separate into string submissions with spaces.


Will get a similar return
```json
{
     "access_token" : "eyJ0e***************************kjerg",
     "expires_in" : 7200,
     "token_type" : "bearer",
     "scope" : "cloud"
}
```
access_token is the token we need，expires_in is Validity period


## API
The information about the api is in the format returned. Please refer to the method of carrying the token.
[api.md](./api_en.md)


### Sub-account list 
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|filter|string|true|```visible,hidden,all```|


**URL**

```
GET https://api-prod.poolin.com/api/public/v2/platform/subaccount
```

**Results**

``` json
{
    "err_no": 0,
    "data": [
        "puid": 9999888,
        "name": "111ttbtc",
        "nickname": "111ttbtc",
        "default_coin_type": "zec",
        "coins": {},
        "created_at": 1561537835
    ],
    [
        ...
    ]
}

```

**Return field description**

|Return field|Description|
|---|---|
|puid|sub-account id|
|name|Name|
|default_coin_type|Default coin|

---
### Batch query sub-account calculation information
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puids|string|true|puid string comma concatenated，such as：```puids=78,82```|


**URL**

```
GET https://api-prod.poolin.com/api/public/v2/platform/subaccount/batch
```
**Results**

``` json
{
    "err_no": 0,
    "data": [
        "puid": 9999888,
        "name": "111ttbtc",
        "shares_unit": "T",
        "default_coin_type": "zec",
        "workers_active": 21,
        "workers_inactive": 1,
        "workers_dead": 0,
        "accept_count": 21,
        "yesterday_amount": 12389283,
        "shares_15m": 33.1
        "shares_1h": 23.1
        "shares_24h": 33.2
        "reject_1h": 0.03
    ],
    [
        ...
    ]
}

```

**Return field description**

|Return field|Description|
|---|---|
|puid|sub-account id|
|name|Name|
|shares_unit|Hashrate unit|
|default_coin_type|Default coin|
|workers_active|Number of active mining machines|
|workers_inactive|Number of inactive mining machines|
|workers_dead|Number of failed machines|
|accept_count||
|yesterday_amount|Yesterday earnings|
|shares_15m|Real-time Hashrate|
|shares_1h|1h Hashrate|
|shares_24h|24h Hashrate|
|reject_1h|Hashrate unit|

---
### Query information about a sub-account and currency
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|sub-account id|
|coin_type|string|true|coin_type|


**URL**

```
GET https://api-prod.poolin.com/api/public/v2/platform/subaccount/summary
```
**Results**

Return similar as ```Batch query sub-account calculation information```

---
### Create a sub-account |require subaccount-create
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|name|string|true|Sub-account name|
|coin_type|string|true|default coin|
|payment_id|string|false|Payment id of xmr and etn|


**URL**

```
POST https://api-prod.poolin.com/api/public/v2/platform/subaccount/create
```

**Results**

``` json
{
    "err_no": 0,
    "data": [
        "puid": 9999888,
        "name": "111ttbtc",
        "default_coin_type": "zec",
        "coins": {},
        "created_at": 1561537835
    ],
    [
        ...
    ]
}

```
If ```err_no``` is not 0, it is the following number, please correct it for the cause of the error.

|error_code|reason|
|---|---|
|100|Currently up to 9999 sub-accounts are allowed, more need to apply separately|
|101|Sub-account name already exists|
|102|Only 100 sub-accounts can be created within 24 hours. If the frequency exceeds the frequency, an error will be reported.|
|501，505|Address verification failed|
|504|Payment_id verification failed|
|507|Unexpected error|

**Return field description**

|Return field|Description|
|---|---|
|puid|sub-account id|
|name|Name|
|default_coin_type|Default coin|

---
### Create observer, observer token  |require watcher-create
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|name|string|true|Sub-account name|
|coin_types|array|true|Currency that observers can see, like ['btc']|
|puid|int|true|sub-account id|


**URL**

```
POST https://api-prod.poolin.com/api/public/v2/platform/watcher/token/create
```

**Results**

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
If ```err_no``` is not 0, it is the following number, please correct it for the cause of the error.

|error_code|reason|
|---|---|
|100|Up to 50 active observers|


**Return field description**

|Return field|Description|
|---|---|
|token|observers token|
|puid|sub-account id|
|coin_types|coin|
|name|observer name|
|created_at|create date|
|expired_at|expire date|
|subaccount_name|Sub-account name|

---
### Create a new version of the observer token(with observer permission)  |require watcher-create
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|name|string|true|Sub-account name|
|coin_types|array|true|Currency that observers can see, like ['btc']|
|puid|int|true|sub-account id|
|watcher_type|array|true|observer permission['dashboard', 'payment', 'worker']


**URL**

```
POST https://api-prod.poolin.com/api/public/v2/platform/watcher/token/create/new
```

**Results**

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
If ```err_no``` is not 0, it is the following number, please correct it for the cause of the error.

|error_code|reason|
|---|---|
|100|Up to 50 active observers|


**Return field description**

|Return field|Description|
|---|---|
|token|observer token|
|puid|sub-account id|
|coin_types|coin types|
|name|observer name|
|created_at|create date|
|expired_at|expire date|
|subaccount_name|Sub-account name|

---
### Update sub-account address |require address-update
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|address|string|true|Address to update|
|coin_type|string|true|Which coin address is to be updated|


**URL**

```
POST https://api-prod.poolin.com/api/public/v2/platform/subaccount/{puid}/update/address
```

**Results**

``` json
{
    "err_no": 0,
    "data": null
}

```
If ```err_no``` is not 0, it is the following number, please correct it for the cause of the error.

|error_code|reason|
|---|---|
|100|Sub account does not exist|
|501，505|Address verification failed|
|504|Payment_id verification failed|


**Return field description**

|Return field|Description|
|---|---|

---
### 删除观察者
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|string|true|PUID|
|token|string|true|要删除的观察者token|

**URL**

```
POST https://api-prod.poolin.com/api/public/v2/platform/watcher/token/delete
```

**Results**

``` json
{
    "err_no": 0,
    "data": null
}
```

---

## 云算力功能的基本情况 |需要cloud
**我们不允许一个子账户既出让算力，也受让算力。同一时间只能是一种状态。现在仅支持比特币btc的转移，单位只能是T。从美东区域接入的算力暂时无法被转移。**


### 云算力-合约、转移列表
` 云算力现在仅支持btc ` 
显示最新的100条未删除云算力分配记录。

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|coin_type|string|true|coin_type，目前仅支持btc|
|puid|int|true|sub-account id|


**URL**

```
GET https://api-prod.poolin.com/api/public/v2/platform/cloud/transfer-hashrate?coin_type=btc&puid=9001000
```

**Results**

``` json
{
    "err_no": 0,
    "data": {
        "id": 101,
        "name": "suba",
        "to": "subb",
        "hashrate": 23,
        "unit": "T",
        "start_ts": 1557936000,
        "end_ts": 1558022400,
        "status": 3
    }
}

```

**Return field description**

|Return field|Description|
|---|---|
|id|转移合约id|
|name|转让人|
|to|受让人|
|hashrate|算力值|
|unit|Hashrate unit|
|start_ts|开始时间戳|
|end_ts|过期时间戳|
|status|1未开始 2生效中 3过期 4已删除|

---
### 云算力-可转让算力查询
` 云算力现在仅支持btc `
目前最多只能转让实时算力的60%，多个合约累计计算。 

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|coin_type|string|true|coin_type，目前仅支持btc|
|puid|int|true|sub-account id|


**URL**

```
GET https://api-prod.poolin.com/api/public/v2/platform/cloud/allow-sell?coin_type=btc&puid=9001000
```

**Results**

``` json
{
    "err_no": 0,
    "data": {
        "can_sell": 89,
        "unit": "T",
    }
}

```

**Return field description**

|Return field|Description|
|---|---|
|can_sell|允许转移的算力大小|
|unit|Hashrate unit|

---
### 云算力-受让方查询
` 云算力现在仅支持btc `
 

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|出让方puid|
|name|string|true|受让方名称，长度4-30|


**URL**

```
GET https://api-prod.poolin.com/api/public/v2/platform/cloud/check-name?name=testname&puid=9001000
```

**Results**

``` json
{
    "err_no": 0,
    "data": true
}
|error_code|reason|
|---|---|
|101|受让方出让算力中|
|103|出让方有来自别人的算力|

```

**Return field description**

|Return field|Description|
|---|---|
|data|是否存在受让方|

---
### 云算力-删除合约
` 云算力现在仅支持btc ` 

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|合约所属转让人的puid|
|id|int|true|合约id|


**URL**

```
POST https://api-prod.poolin.com/api/public/v2/platform/cloud/delete
```

**Results**

``` json
{
    "err_no": 0,
    "data": null
}

```

**Return field description**

|Return field|Description|
|---|---|

---
### 云算力-创建合约、转移算力
` 云算力现在仅支持btc ` 

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|出让人sub-account id|
|coin_type|string|true|coin_type，目前仅支持btc|
|unit|string|true|转让算力单位，目前只能是T|
|hashrate|int|true|转让的算力值|
|to|string|true|受让方子账户名|
|start_time|int|true|开始时间，web上目前只显示到天级别的精度，建议以某天0点开始。|
|end_time|int|true|结束时间|


**URL**

```
POST https://api-prod.poolin.com/api/public/v2/platform/cloud/{puid}/transfer
```

**Results**

``` json
{
    "err_no": 0,
    "data": 123
}

```
|error_code|reason|
|---|---|
|100|受让方不存在|
|101|受让方出让算力中|
|102|不允许出让超过60%的算力|
|103|出让方有来自别人的算力|


**Return field description**

|Return field|Description|
|---|---|
|data|合约id|

---

### vcash 合并挖矿开关 

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|sub-account id|

**URL**
```
POST https://api-prod.poolin.com/api/public/v2/platform/vcash/open  打开vcash合并挖矿
```

```
POST https://api-prod.poolin.com/api/public/v2/platform/vcash/close  关闭vcash合并挖矿
```

**Results**

打开vcash接口
``` json
{
    "err_no": 0,
    "data": null
}
```

关闭vcash接口
``` json
{
    "err_no": 0,
    "data": null
}
```

