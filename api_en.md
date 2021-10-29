# Document outdated
# Document outdated
# Document outdated
# Document outdated
# [New Document](https://documenter.getpostman.com/view/17982947/UV5cAFWQ)。

------


# pool_web_api_doc


## ReadToken

Can only view the data of the sub-account to which the token belongs. 
And can not modify the sensitive information of related sub-accounts, such as the collection address. 


#### How to get ReadToken 

1. First create a account of Poolin and create a sub-account.
2. Click ```Sub-account Manage``` in the top right corner of the page.
3. Click ```setting```-```Share to Watcher``` On the right side of the corresponding sub-account
4. Once you create a sub-account, you will get a link such as:www.poolin.com/my/8000010/zec?read_token=wowgLj2*****ga.
The string of random codes at the beginning of wow is the ReadToken of the sub-account. 8000010 is the puid of this sub-account.


## PlatformToken

Can create accounts, generate observers, modify addresses.
[Platform](./platform_en.md)


## API
#### Note: Authentication requires passing the accessToken in the request header. The parameter name is: ```authorization```, the value is ```Bearer TOKEN```, and TOKEN is replaced with the token value to be transmitted.


### 1、Get a list of sub-accounts（Require OwnerToken）

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|

**URL**

```
GET https://api-prod.poolin.com/api/public/v1/subaccount
```

**Results**

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
    ]
}
```

**Return field description**

|Return field|Description|
|---|---|
|puid|sub-account id|
|name|name|
|nickname|nickname|
|default_coin_type|default coin type|
|status|account status（1:normal;0:hidden）|


### 2、Batch access sub-account summary information（Require OwnerToken）

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puids|string|true|puid string comma concatenated，such as：```puids=78,82```|


**URL**

```
GET https://api-prod.poolin.com/api/public/v1/subaccount/batch
```

**Results**

```
{
    err_no:0,
    data:[
        {
            "puid": 4527,
            "name": "poolin000dcr",
            "shares_unit": "K",
            "workers_active": 0,
            "workers_inactive": 0,
            "workers_dead": 1,
            "accept_count": 92496,
            "yesterday_amount": 0,
            "shares_15m": 0,
            "shares_24h": 0,
            "reject_15m": 0, 
        },
       ......
   ]
}
```

**Return field description**

|Return field|Description|
|---|---|
|puid|sub-account id|
|name|-|
|shares_unit|unit|
|workers_active	|Number of active mining machines|
|workers_inactive|Number of inactive mining machines|
|workers_dead|Number of failed machines|
|accept_count|Number of accept|
|yesterday_amount|Actual income yesterday|

### 3、Sub-account revenue summary information

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|sub-account id|
|coin_type|string|true|coin type|

**URL**

```
GET https://api-prod.poolin.com/api/public/v2/payment/stats
```

**Results**

```
{
    "err_no":0,
    "data": {
           "total_paid_amount": 0,
           "yesterday_amount": "1506664",
           "balance": 770493318,
           "today_estimate": 0 
     }
}
```

**Return field description**

|Return field|Description|
|---|---|
|total_paid_amount|Cumulative paid|
|yesterday_amount|Yesterday earnings|
|balance|Balance|
|today_estimate	|Today earnings(estimated)|


### 4、Sub-account historical income list

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|sub-account id|
|coin_type|string|true|-|
|page|int|true|-|
|start_date|int|false|Starting time like ```20190823```|
|end_date|int|false|End time like ```20190930```|

**URL**

```
GET https://api-prod.poolin.com/api/public/v2/payment/history
```

**Results**

```
{
    "err_no":0,
    "data": {
           "current_page": 1,
           "data": [
                  {
                       "date": 20180920,
                       "coin_type": "zec",
                       "amount": 1506664,
                       "change": -0.775,
                       "share_accept": 3.561,
                       "share_unit": "K",
                       "reject_rate": 0.0072,
                       "address": null,
                       "address_created_at": null,
                       "txhash": null,
                       "unpaid_reason": {
                              "code": "ERR_EMPTY_ADDRESS"
                        },
                        "payment_status": "DELAYED",
                        "paid_at": null,
                        "created_at": 1537489978,
                        "rate": null
                  },
                ......
           ],

           "first_page_url": "https://api-prod.poolin.com/api/public/v2/payment/history?page=1",                              
           "from": 1,
           "last_page": 4,
           "last_page_url": "https://api-prod.poolin.com/api/public/v2/payment/history?page=4",
           "next_page_url": "https://api-prod.poolin.com/api/public/v2/payment/history?page=2",
           "path": "https://api-prod.poolin.com/api/public/v2/payment/history",
           "per_page": 20,
           "prev_page_url": null,
           "to": 20,
           "total": 69
      }
}
```

**Return field description**

|Return field|Description|
|---|---|
|date|-|
|coin_type|-|
|amount|Amount of income|
|change|Daily change|
|share_accept|Daily hashrate|
|share_unit|hashrate unit|
|reject_rate|Reject Rate|
|address|Payment address|
|txhash|Trading hash|
|unpaid_reason|Reasons for non-payment|
|payment_status|Payment status: ```PAID``` paid,```PENDING``` Paying,```DELAYED``` delay payment|
|paid_at|Payment time|


### 5、Mining machine group list

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|-|
|coin_type|string|true|-|

**URL**

```
GET  https://api-prod.poolin.com/api/public/v2/group/all
```

**Results**

```
{

  "err_no": 0,
  "data": [
      {
         "group_id": 0,
         "name": "all",
         "workers_active": 0,
         "workers_inactive": 0,
         "workers_dead": 1
      },
      {
         "group_id": -1,
         "name": "default",
         "workers_active": 0,
         "workers_inactive": 0,
         "workers_dead": 1
      },
      {
         "group_id": 429,
         "name": "123",
         "workers_active": 0,
         "workers_inactive": 0,
         "workers_dead": 0
      },
      ......
  ]
}
```

**Return field description**

|Return field|Description|
|---|---|
|group_id|Group id|                                     
|name|Group Name|
|workers_active|Number of active mining machines|
|workers_inactive|Number of inactive mining machines|
|workers_dead|Number of failed machines|


### 6、Miner List

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|-|
|coin_type|string|true|-|
|status|string|true|Status（ALL；ACTIVE；INACTIVE；DEAD）|
|page|int|true|-|
|pagesize|int|true|Number of data per page|

**URL**

```
GET  https://api-prod.poolin.com/api/public/v2/worker
```

**Results**

```
{
  "err_no": 0,
  "data": {
      "page": 1,
      "page_size": 30,
      "page_count": 1,
      "total_count": 1,
      "data": [
         {
            "region_id": "beijing",
            "worker_id": "-6678562660843308848",
            "worker_name": "100",
            "shares_15m": 0,
            "shares_24h": 0,
            "reject_rate": 0,
            "accept_count": 13708,
            "active_duration_sec": 82765,
            "session_first_share_time": 1537427737,
            "last_share_time": 1537510502,
            "status": "DEAD",
            "shares_unit": "K"
         }
         .....
      ],
      "workers_active": 0,
      "workers_inactive": 0,
      "workers_dead": 1
   }
}
```

**Return field description**

|Return field|Description|
|---|---|
|region_id|Region|
|worker_id|Mining machine id|
|worker_name|Mineral machine name|
|shares_15m|Real-time Hashrate|
|shares_24h|24h Hashrate|
|shares_unit|Hashrate unit|
|reject_rate|Reject rate|
|accept_count|Number of accept|
|last_share_time|Recent submission time|
|status|Status|


### 7、Single miner info

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|-|
|coin_type|string|true|-|
|region_id|string|true|Region(Get From Miner List API)|

**URL**

```
GET  https://api-prod.poolin.com/api/public/v2/worker/{worker_id}
```

**Results**

```
{
  "err_no": 0,
  "data": {
      "worker_id": "-6324186625670527823",
      "worker_name": "test002",
      "shares_15m": 10.486,
      "shares_24h": 14.89,
      "shares_unit": "K",
      "accept_count": 449235,
      "reject_rate": 0.027,
      "last_share_ip": "221.221.255.189",
      "last_share_lan_ip": 0,
      "last_share_time": 1537931052,
      "active_duration_sec": 420517,
      "session_first_share_time": 1537510535,
      "worker_status": "ACTIVE"
   }
}
```

**Return field description**

|Return field|Description|
|---|---|
|worker_id|Mining machine id|
|worker_name|Mineral machine name|
|shares_15m|Real-time Hashrate|
|shares_24h|24h Hashrate|
|shares_unit|Hashrate unit|
|accept_count|Number of accept|
|reject_rate|Reject Hashrate|
|last_share_ip|Last submitted ip|
|last_share_lan_ip|Last submitted to the internal network ip|
|last_share_time|Last submitted time|
|worker_status|status|


### 8、Single mining machine historical calculation power

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|-|
|region_id|string|true|Region|
|count|int|true|number|
|start_ts|int|true|Start timestamp|
|dimension|string|true|Time dimension (one day: 1d; one hour: 1h)|


**URL**

```
GET  https://api-prod.poolin.com/api/public/v2/worker/{work_id}/share-history
```

**Results**

```
{

  "err_no": 0,
  "data": {
      "unit": "K",
      "tickers": [
          [
            1535414400, //Timestamp
            15.363,  //Hashrate
            0.0015   //Rejectrate
          ],
          [
            1536710400,
            15.186,
            0.001
          ],
          ......
       ]
   }
}
```

**Return field description**

|Return field|Description|
|---|---|
|unit|unit|
|tickers|Array of hashrate and rejection rates|


### 9、Sub-accounting hashrate history

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|-|
|count|int|true|Number of scales|
|dimension|string|true|1h hour scale, 1d day scale|
|start_ts|int|true|Start timestamp|
|coin_type|string|true|-|



**URL**

```
GET https://api-prod.poolin.com/api/public/v2/worker/share-history
```

**Results**

```
{
    "err_no": 0,
    "data": {
        "unit": "P",
        "tickers": [
            [
                1561039200,
                163.349,
                0.0013
            ],
            [
                1561042800,
                163.288,
                0.001
            ],
        ]
    }
}
```

**Return field description**

|Return field|Description|
|---|---|
|unit|hashrate unit|
|tickers|Scale information, positive sequence. The first of the three data is the timestamp, the second is the power calculation, and the third is the rejection rate (decimal)|


### 10、Sub-accounting payout history

**Request parameter**

|Field|Type|Require|Remark|
|---|---|---|---|
|puid|int|true|-|
|coin_type|string|true|-|
|start_date|int|false|Starting time like ```20190823```|
|end_date|int|false|End time like ```20190930```|


**URL**

```
GET https://api-prod.poolin.com/api/public/v2/payment/payout-history
```

**Results**

```
{
    "err_no": 0,
    "data": [
        {
            "txhash":"e050c55e8449ae1781b81c25960bc6cecae0ebaeae896fc99bc51d47e1d0bxxx",
            "amount":"12345",
            "pay_time":"2019-12-02 00:00:00",
        }
    ]
}
```

**Return field description**

|Return field|Description|
|---|---|
|txhash||
|amount||
|pay_time||



## PUBLIC API

|Field|Type|Require|Remark|
|---|---|---|---|
|coin_tag|string|true|coin tag, Optional value(BTC,LTC,BCH,DASH,DCR,SC,ZEC,ETH,ETN,XMR)
|start_ts|int|true|Start time stamp
|end_ts|int|false|End time stamp


### 1、Calculate the difficulty of the currency by day

**URL**

```
GET  https://api-prod.poolin.com/api/public/v2/basedata/chain/difficulty/stat_by_day?coin_tag={coin_tag}&start_ts={start_ts}
```

**Results**

```
{
    "err_no": 0,
    "data": {
        "2018-10-30":  {
            "avg_difficulty": "7182852313938.34",  //Average difficulty
            "count": 154 //Number of blocks
        },
        "2018-10-31":  {
            avg_difficulty: "7182852313938.34",
            "count": 163
        },
        ......
    }
}
```
