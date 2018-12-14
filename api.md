# pool_web_api_doc
## API
#### 注意：身份验证需要在请求的header中传入accessToken，参数名：authorization

### 1、获取子账户列表

**请求参数**

|字段|类型|必须|备注|
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
    ]
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


### 2、批量获取子账户概要信息

**请求参数**

|字段|必须|含义|备注|
|---|---|---|---|
|puids|string|true|puid用逗号拼接的字符串，例如：puids=78,82|


**请求URL**

```
GET https://api-prod.poolin.com/api/public/v1/subaccount/batch
```

**返回结果**

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

**返回字段说明**

|返回值字段|说明|
|---|---|
|puid|子账户id|
|name|名称|
|shares_unit|算力单位|
|workers_active	|活跃矿机数|
|workers_inactive|不活跃矿机数|
|workers_dead|失效矿机数|
|accept_count|接收数|
|yesterday_amount|昨日实收|

### 3、子账户收益概要信息

**请求参数**

|字段|类型|必须|备注|
|---|---|---|---|
|puid|int|true|子账户id|
|coin_type|string|true|币种|

**请求URL**

```
GET https://api-prod.poolin.com/api/public/v2/payment/stats
```

**返回结果**

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

**返回字段说明**

|返回值字段|说明|
|---|---|
|total_paid_amount|累计已支付|
|yesterday_amount|昨日收益|
|balance|余额|
|today_estimate	|今日已挖（预估）|


### 4、子账户历史收益列表

**请求参数**

|字段|类型|必须|备注|
|---|---|---|---|
|puid|int|true|子账户id|
|coin_type|string|true|币种|
|page|int|true|第几页|

**请求URL**

```
GET https://api-prod.poolin.com/api/public/v2/payment/history
```

**返回结果**

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

**返回字段说明**

|返回值字段|说明|
|---|---|
|date|日期|
|coin_type|币种|
|amount|收益数额|
|change|日变动|
|share_accept|每日算力|
|share_unit|算力单位|
|reject_rate|拒绝率|
|address|支付地址|
|txhash|交易哈希|
|unpaid_reason|未支付原因|
|payment_status|支付状态|
|paid_at|支付时间|

### 5、矿机分组列表

**请求参数**

|字段|类型|必须|备注|
|---|---|---|---|
|puid|int|true|子账户id|
|coin_type|string|true|币种|

**请求URL**

```
GET  https://api-prod.poolin.com/api/public/v2/group/all
```

**返回结果**

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

**返回字段说明**

|返回值字段|说明|
|---|---|
|group_id|分组id|                                     
|name|分组名称|
|workers_active|活跃矿机数|
|workers_inactive|不活跃矿机数|
|workers_dead|失效矿机数|

### 6、矿机列表

**请求参数**

|字段|类型|必须|备注|
|---|---|---|---|
|puid|int|true|子账户id|
|coin_type|string|true|币种|
|status|string|true|状态（全部：ALL；活跃：ACTIVE；不活跃：INACTIVE；失效：DEAD）|
|pagesize|int|true|每页数据条数|

**请求URL**

```
GET  https://api-prod.poolin.com/api/public/v2/worker
```

**返回结果**

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

**返回字段说明**

|返回值字段|说明|
|---|---|
|region_id|地区|
|worker_id|矿机id|
|worker_name|矿机名称|
|shares_15m|实时算力|
|shares_24h|24小时算力|
|shares_unit|算力单位|
|reject_rate|拒绝率|
|accept_count|接受数|
|last_share_time|最近提交时间|
|status|状态|


### 7、单台矿机概要信息

**请求参数**

|字段|类型|必须|备注|
|---|---|---|---|
|puid|int|true|子账户id|
|coin_type|string|true|币种|
|region_id|string|true|地区(从矿机列表接口可获取)|

**请求URL**

```
GET  https://api-prod.poolin.com/api/public/v2/worker/{worker_id}
```

**返回结果**

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

**返回字段说明**

|返回值字段|说明|
|---|---|
|worker_id|矿机id|
|worker_name|矿机名称|
|shares_15m|实时算力|
|shares_24h|24小时算力|
|shares_unit|算力单位|
|accept_count|接受数|
|reject_rate|拒绝率|
|last_share_ip|上次提交ip|
|last_share_lan_ip|上次提交内网ip|
|last_share_time|上次提交时间|
|worker_status|矿机状态|


### 8、单台矿机历史算力

**请求参数**

|字段|类型|必须|备注|
|---|---|---|---|
|puid|int|true|子账户id|
|region_id|string|true|地区(从矿机列表接口可获取)|
|count|int|true|数量|
|start_ts|int|true|开始时间戳|
|dimension|string|true|时间维度（一天：1d ；一小时：1h）|


**请求URL**

```
GET  https://api-prod.poolin.com/api/public/v2/worker/{work_id}/share-history
```

**返回结果**

```
{

  "err_no": 0,
  "data": {
      "unit": "K",
      "tickers": [
          [
            1535414400, //时间戳
            15.363,  //算力
            0.0015   //拒绝率
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

**返回字段说明**

|返回值字段|说明|
|---|---|
|unit|算力单位|
|tickers|算力和拒绝率数组|

## PUBLIC API

|字段|类型|必须|备注|
|---|---|---|---|
|coin_tag|string|true|币种TAG, 可选值(BTC,LTC,BCH,DASH,DCR,SC,ZEC,ETH,ETN,XMR)
|start_ts|int|true|起始时间戳
|end_ts|int|false|结束时间戳


### 1、按天统计币种的难度

**请求URL**

```
GET  https://api-prod.poolin.com/api/public/v2/basedata/chain/difficulty/stat_by_day?coin_tag={coin_tag}&start_ts={start_ts}
```

**返回结果**

```
{
    "err_no": 0,
    "data": {
        "2018-10-30":  {
            "avg_difficulty": "7182852313938.34",  //平均难度
            "count": 154 //块数量
        },
        "2018-10-31":  {
            avg_difficulty: "7182852313938.34",
            "count": 163
        },
        ......
    }
}
```