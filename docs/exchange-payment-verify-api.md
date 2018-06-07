# Exchange Payment Verify API

This API is used to do the verification step of Exchange Payment request. It is not required if you have **No SMS Verification Flow** setup in your account.

***

## Request

###### Endpoint

```html
https://fe-pay.lianlianpay.com/exchange/v1/bankcardpayverify
```

> Direct APIs have an additional requirement which involves our payment risk team and due diligence team. You need to contact your business development to obtain the access.

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|api_version|Required|String(6)|Fixed value, ```1.0```|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|time_stamp|Required|String(14)|The time when request is initialized. Format: YYYYMMDDHHMMSS, E.g. 20170801225714. The deviation can NOT exceed 30 minutes between your server and LianLian's server(GMT +8). |
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|token|Required|String|The token returned in [Exchange Payment Apply API](exchange-payment-apply-api.md)|
|verify_code|Required|String(6)|The SMS verification code sent in [Exchange Payment Apply API](exchange-payment-apply-api.md)|
|no_order|Required|String(32)|Merchant transaction No.|

###### Sample Request

```curl
curl https://fe-pay.lianlianpay.com/exchange/v1/bankcardpayverify \
-H "Content-type:application/json;charset=utf-8" \
-d '{
    	"oid_partner": "201103171000000000",
    	"no_order": "2013051500001",
    	"api_version": "1.0",
    	"verify_code": "222222",
    	"token": "D096DBA0E3E0CC8F1D504C06E71D292D",
    	"user_id": "01020000",
    	"time_stamp": "20130515094137",
    	"sign_type ": "RSA",
    	"sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
    }'
```

***

## Response

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. Refer to [return codes](return-codes.md)|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |

The following parameters are returned only when ```ret_code=0000```(Could be considered as ```result_pay=SUCCESS``` in this API):

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|Fixed value, RSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|no_order|Required|String(32)|Merchant transaction No.|
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|result_pay|Required|String| Payment result. <br> SUCCESS - Payment proceed successfully <br> PROCESSING -  Payment is processing. Return only when **No SMS Verification Flow** is setup|
|settle_date|Required|String(8)| Format YYYYMMDD. |
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|
|pay_type|Required|String(1)| The pay type used in current transaction.|
|currency_order|Required|String(3)|The currency used in the transaction. Refer to [supported currencies](supported-currencies.md) |
|money_order|Required|String(12)|Merchant transaction amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in the currency of ```currency_order```|
|currency_settle|Required|String(3)|The settle currency of transaction|
|money_settle|Required|String(12)|The settle amount of transaction|
|currency_pay|Required|String(3)| The currency of which the payer used. Refer to [supported currencies](supported-currencies.md)|
|money_pay|Required|String(12)|The amount of which the payer payed|
|exchange_rate|Required|String|The exchange rate of current transaction|
|no_agree|Optional|String(16)| A token which represents the key payment information, refer to [Card binding document](card-bind-overview.md) for more details |

###### Sample Response

```json
{
	"oid_partner": "201103171000000000",
	"dt_order": "20130515094013",
	"no_order": "2013051500001",
	"oid_paybill": "2013051613121201",
	"money_order": "31.68",
	"currency_order": "USD",
	"money_pay": "210.97",
	"currency_pay": "CNY",
	"money_sellte": "31.47",
	"currency_sellte": "USD",
	"exchange_rate": "6.66",
	"result_pay": "SUCCESS",
	"settle_date": "20130516",
	"info_order": "用户13958069593购买了3桶羽毛球",
	"pay_type": "D",
	"bank_code": "01020000",
	"ret_code": "0000",
	"ret_msg": "交易成功",
	"sign_type": "RSA",
	"sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
}
```

