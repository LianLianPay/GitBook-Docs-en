# Refund Apply API

This API is used to apply a refund. 
***

## Request

###### Endpoint

```html
https://traderapi.lianlianpay.com/exchangerefund.htm
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|no_refund|Required|String(32)|Refund transaction ID. in your system. It is recommended to set a different value from ```no_order```|
|dt_refund|Required|String(14)|Refund date. Format: YYYYMMddHHmmss, E.g. 20170801225714|
|money_order|Required|String(12)|Refund amount. Refund amount should be less than or equal to the amount of original ```no_order```, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, the currency is same as the settlement currency of original transaction.|
|no_order|Optional|String(32)|Original merchant order No. Either ```no_order``` or ```oid_paybill``` is required for refund|
|dt_order|Optional|String(14)|Original merchant order date. Format: YYYYMMddHHmmss, E.g. 20170801225714|
|oid_paybill|Optional|String(32)|Original unique transaction No. in LianLian system. E.g. 2011030900001098. Either ```no_order``` or ```oid_paybill``` is required for refund |
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|

###### Sample Request

```html
curl https://traderapi.lianlianpay.com/exchangerefund.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{
        "oid_partner":"201103171000000000",
        "dt_refund":"20130515094018",
        "no_refund":"2013051500005",
        "money_order":"200.01",
        "dt_order":"20130515094013",
        "no_order":"2013051500001",
        "oid_paybill":"2013051613121201",
        "notify_url":"http://payhttp.xiaofubao.com/***/refund.htm",
        "sign_type ":"RSA",
        "sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
    }'
```

***

## Response

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. Refer to [Return Codes](return-codes.md)|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |


The following parameters are returned only when ```ret_code=0000```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|no_refund|Required|String(32)|Refund No. in your system. It is recommended to set a different value from ```no_order```|
|dt_refund|Required|String(14)|Refund date. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|money_refund|Required|String(12)|The actual refund amount, in CNY.|
|money_foreign_refund|Required|String(12)|The actual refund amount, which is ```money_order``` in original request|
|rate_id|Required|String|The ID of rate|
|rate|Required|String|The rate using in this request|
|exchange_bankcode|Required|String|The bank code of the bank where currency exchange occurs|
|oid_refundno|Required|String(16)| Unique refund transaction No. in LianLian system |
|sta_refund|Required|String(1) | 0, refund initialized <br> 1, refund processing <br> 2, refund success <br> 3, refund failed |
|settle_date|Required|String(8) | Format YYYYMMDD. Returns when refund is successful |

###### Sample Response

```json
{
    "ret_code":"0000",
    "ret_msg":"交易成功",
    "oid_partner":"201103171000000000",
    "no_refund":"2013051500001",
    "dt_refund":"20130515094018",
    "oid_refundno":"2013051613121201",
    "money_refund":"200.01",
    "money_foreign_refund":"30.03",
    "rate_id":"1",
    "rate":"6.66",
    "exchange_bankcode":"01040000",
    "sta_refund":"2",
    "settle_date":"20130627",
    "sign_type":"RSA", 
    "sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
}
```
