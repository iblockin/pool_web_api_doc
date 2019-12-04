## Platform 

A valid certificate (token) issued by the user center, valid for 2 hours. Please re-acquire after expiration.
This authentication method only supports some advanced API. For unsupported parts, please use the read token.


### Qualification
This feature is only available to some users. Please contact customer service if you need to use it.


### get token
1. Prepare the client_id and client_secret provided by us.
2. [post] https://account.blockin.com/oauth/v1/token parameter ```client_id，client_secret，grant_type，scopes```
3. grant_type fixed value of ```client_credentials```
4. The scopes currently have four subaccount-create, watcher-create, address-update, and cloud, corresponding to specific API permissions. Separate into string submissions with spaces.


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
|address|string|false|payout address|
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
### delete observer
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|string|true|PUID|
|token|string|true|observer token|

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

## Cloud Hashrate's Info |require cloud
**We do not allow a sub-account to transfer and be transferee. The same time can only be a state. Now only the transfer of BTC is supported, the unit can only be TH/s.**


### Cloud Hashrate contact list
` Only btc ` 
Display the latest 100 undeleted records。

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|coin_type|string|true|coin_type，only ```btc```|
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
        "status": 3,
        "suspend": 0
    }
}

```

**Return field description**

|Return field|Description|
|---|---|
|id|contact id|
|name|transfer|
|to|transferee|
|hashrate|hashrate|
|unit|Hashrate unit|
|start_ts|begin timestamp|
|end_ts|end timestamp|
|status|1 Not started, 2 Effective, 3 Expired, 4 Deleted|
|suspend|0-normal,1-suspended|

---
### Cloud Hashrate- Query Available Hashrate 
` Only btc `
At present, only 60% of the real-time Hashrate can be transferee, and the cumulative calculation of multiple contracts can be made

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
|can_sell|Available Hashrate|
|unit|Hashrate unit|

---
### Cloud Hashrate-Query transferee
` Only btc `
 
**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|transfer puid|
|name|string|true|transferee name，length 4-30|


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
|101|transferee is a transfer|
|103|The transferor has the hashrate from others|

```

**Return field description**

|Return field|Description|
|---|---|
|data|Is there a transferee|

---
### Cloud Hashrate-delete contact
` Only btc ` 

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|owner puid|
|id|int|true|contact id|


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
### Cloud Hashrate-create contact
` Only btc ` 

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|出让人sub-account id|
|coin_type|string|true|coin_type，目前仅支持btc|
|unit|string|true|hashrate unit ```T```|
|hashrate|int|true|hashrate, for example: 100|
|to|string|true|transferee sub-account name|
|start_time|int|true|Start timestamp, the web only shows the accuracy of day level at present, it is recommended to start at 0:00 a.m. on a certain day.|
|end_time|int|true|End timestamp|


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
|100|transferee not exists|
|101|transferee is transfer hashrate now|
|102|No more than 60% of the hashrate is allowed to be transferred.|
|103|transfer is a transferee|


**Return field description**

|Return field|Description|
|---|---|
|data|contact id|

---
### Suspended Cloud Hashrate-create Contract

Note: this interface is not idempotent. It will turn on or off according to the current contract status (the suspended contract will switch to normal, and the normal contract will switch to suspended)

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|id|int|true|Contract ID|

**URL**

```
POST https://api-prod.poolin.com/api/public/v2/platform/cloud/suspend
```

**Results**

``` json
{
    "err_no": 0,
    "data": {
        "suspend": 1
    }
}
```

**Return field description**

|Return field|Description|
|---|---|
|suspend|current status(0-normal,1-suspended)|

**Error code description**

|error_code|reason|
|---|---|
|113|Not found the Contract|
|114|The contract hasn't started yet|
|115|Contract closed|

---

### Cloud Hashrate contract lists

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|sub-account id|
|coin_type|string|true|coin type|
|page_size|int|false|per page size;default:20;max:100|
|page|int|false|page|

**URL**

```
GET https://api-prod.poolin.com/api/public/v2/platform/cloud/sells
```

**Result**

``` json
{
    "err_no": 0,
    "data": {
        "total": 10,
        "per_page": 5,
        "last_page": 2,
        "current_page": 1,
        "data": [
            {
                "id": 16,
                "name": "",
                "to": "btctest",
                "hashrate": 1,
                "unit": "T",
                "start_ts": 1557936000,
                "end_ts": 1558022400,
                "status": 3,
                "suspend": 0
            },
            {
                "id": 17,
                "name": "",
                "to": "btctest",
                "hashrate": 5,
                "unit": "T",
                "start_ts": 1558022400,
                "end_ts": 1558195200,
                "status": 3,
                "suspend": 0
            },
            ......
        ]
    }
}
```

**Return field description**

|Return field|Description|
|---|---|
|total|total records|
|per_page|page size|
|last_page|last_page|
|current_page|current_page|
|data.id|contact id|
|data.name|transfer|
|data.to|transferee|
|data.hashrate|hashrate|
|data.unit|Hashrate unit|
|data.start_ts|begin timestamp|
|data.end_ts|end timestamp|
|data.status|1 Not started, 2 Effective, 3 Expired, 4 Deleted|
|data.suspend|0-normal,1-suspended|

---

### Cloud Hashrate-get info by contract id

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|-|-|-|-|

**URL**

```
GET https://api-prod.poolin.com/api/public/v2/platform/cloud/contracts/{id} {id}should be replaced with contract ID
```

**Results**

``` json
{
    "err_no": 0,
    "data": {
        "id": 16,
        "name": "",
        "to": "btctest",
        "hashrate": 1,
        "unit": "T",
        "start_ts": 1557936000,
        "end_ts": 1558022400,
        "status": 3,
        "suspend": 0
    }
}
```

**Return field description**

|Return field|Description|
|---|---|
|id|contact id|
|name|transfer|
|to|transferee|
|hashrate|hashrate|
|unit|Hashrate unit|
|start_ts|begin timestamp|
|end_ts|end timestamp|
|status|1 Not started, 2 Effective, 3 Expired, 4 Deleted|
|suspend|0-normal,1-suspended|

**Error code description**

|error_code|reason|
|---|---|
|404|Not Found Contract|

---

### vcash merge-mining swtich 

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

open vcash
``` json
{
    "err_no": 0,
    "data": null
}
```

close vcash
``` json
{
    "err_no": 0,
    "data": null
}
```

---

### Get auto payment status

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|sub-account id|
|coin_type|string|true|coin type|

**URL**
```
GET https://api-prod.poolin.com/api/public/v2/platform/payment/auto
```

**Results**

``` json
{
    "err_no": 0,
    "data": {
        "is_close": 0   
    }
}
```

**Return field description**

|Return field|Description|
|---|---|
|is_close|Is it closed? (0-normal, 1-closed automatic payment)|

---

### OPEN/CLOSE automatic payment

Note:The API automatically reverses the current payment status

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|sub-account id|
|coin_type|string|true|coin type|

**URL**
```
POST https://api-prod.poolin.com/api/public/v2/platform/payment/auto
```

**Results**

``` json
{
    "err_no": 0,
    "data": {
        "is_close": 1  
    }
}
```

**Return field description**

|Return field|Description|
|---|---|
|is_close|Is it closed? (0-normal, 1-closed automatic payment)|
