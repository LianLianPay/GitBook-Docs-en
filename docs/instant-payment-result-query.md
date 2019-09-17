# Instant Payment Result Query API

## Request

###### Endpoint

```html
https://instantpay.lianlianpay.com/paymentapi/queryPayment.htm
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|no_order|Required|String(32)|Merchant order No. Optional if ```oid_paybill``` is present |
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. Optional if ```no_order``` is present |
|query_version|Required|String(6)| 1.0 |

###### Sample Request

```curl
curl https://instantpay.lianlianpay.com/paymentapi/queryPayment.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{ 
        "oid_partner":"201103171000000000", 
        "api_version":"1.0", 
        "no_order":"2013051500005", 
        "oid_paybill":"2013051613121201", 
        "sign_type":"RSA",        
        "sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/ +p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vP TfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk=" 
}'
```

***

## Response

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. Refer to [return codes](return_code.md)|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|no_order|Required|String(32)|Merchant order No.|
|dt_order|Required|String(14)|Merchant order date. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|result_pay|Required|String| SUCCESS, payment is successful. <br> WAITING, waiting for payment result form the issuing bank. <br> PROCESSING, payment is processing by banks. <br> CANCEL, the payment is reversed by issuing bank.<br> FAILURE, the payment is failed. <br> CHECK, the payment need to confrim. <br>CLOSED, No confirmation was made within 24 hours when it is needed.|
|settle_date|Optional|String(8)| Format YYYYMMDD. Returns when payment is successful|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|

###### Sample Response

```json
{   
    "dt_order": "20160728144057",
    "info_order": "test",
    "money_order": "46.78",
    "no_order": "2016072814226",
    "oid_partner": "201606020000150062",
    "oid_paybill": "2016072846609676",
    "result_pay": "PROCESSING",
    "ret_code": "0000",
    "ret_msg": "交易成功",
    "sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk=",
    "sign_type": "RSA" 
} 
```