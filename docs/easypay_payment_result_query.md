# Easy Payment Result Query API

## Request

###### Endpoint

```html
https://queryapi.lianlianpay.com/orderquery.htm
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|no_order|Required|String(32)|Merchant order No. Optional if ```oid_paybill``` is present |
|dt_order|Required|String(14)|Merchant order date. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. Optional if ```no_order``` is present |
|query_version|Optional|String(6)| 1.0, regular response(default value) <br> 1.1, return additional parameter ```memo```, ```bank_name``` |

###### Sample Request

```curl
curl https://queryapi.lianlianpay.com/orderquery.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{
    	"oid_partner":"201103171000000000",
    	"dt_order":"20130515094013",
    	"no_order":"2013051500001",
    	"sign_type":"RSA",
    	"sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
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
|no_order|Optional|String(32)|Merchant order No.|
|dt_order|Optional|String(14)|Merchant order date. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|money_order|Optional|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|result_pay|Optional|String| SUCCESS, payment is successful <br> WAITING, waiting for payment result <br> PROCESSING, payment is processing by banks <br> REFUND, the payment is reversed <br> FAILURE, the payment is failed <br> This is the final transaction status. You may do delivery according to this field|
|settle_date|Optional|String(8)| Format YYYYMMDD. Returns when payment is successful|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|
|pay_type|Optional|String(1)| The payment method used in this transaction. <br> M, regular payment, debit card <br> N, regular payment, credit card <br>  F, authorization payment, credit card| 
|bank_code|Optional|String(8)| Short code of banks|
|bank_name|Optional|String(32)| Name of banks, returns only when ```query_version``` is 1.1, this filed is not involved in [signature](signature.md)|
|memo|Optional|String(255)| Payment failed information from bank side,  returns only when ```query_version``` is 1.1, this filed is not involved in [signature](signature.md) |
|card_no|Optional|String(15~19)| The masked number of used card. E.g. 622208*********0000 |

###### Sample Response

```json
{
	"oid_partner":"201103171000000000",
	"dt_order":"20130515094013",
	"no_order":"2013051500001",
	"result_pay":"SUCCESS",
	"oid_paybill":"2013051500001",
	"money_order":"49.65",
	"settle_date":"20130515",
	"info_order":"测试商品信息",
	"pay_type":"M",
	"bank_code":"01020000",
	"sign_type":"RSA",
	"sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
}
```