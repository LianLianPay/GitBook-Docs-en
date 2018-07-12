# Exchange Payment Apply API

This API is used to apply Exchange Payment request, create corresponding transaction object in LianLian's system and send SMS verification code.

***

## Request

###### Endpoint

```html
https://fe-pay.lianlianpay.com/exchange/v1/bankcardpayapply
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
|platform|Optional|String(32)| ```platform``` is used for sharing user info between multiple ```oid_partner```, this requires additional settings from LianLian side|
|busi_partner|Required|String(6)|Fixed value. Virtual products, ```101001```; Physical products, ```109001```|
|no_order|Required|String(32)|Merchant transaction No.|
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|name_goods|Required|String(40)|Product name. E.g. Pen|
|info_order|Optional|String(255)|```info_order``` will be sent back in synchronous or asynchronous notification for parameters transmission|
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|
|valid_order|Optional|Int|The valid period of ```no_order```, in minute. The status of corresponding transaction will be set to "Closed" once its ```valid_order``` run out. Default: 10080 (7 days). |
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment-risk-item.md)| 
|pay_type|Required|String| M, Easy payment - Debit card <br> N, Easy payment - Credit card <br> D, Authenticate Payment|
|currency_order|Required|String(3)|The currency used in the transaction. Refer to [supported currencies](supported-currencies.md)|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in the currency of ```currency_order```|
|currency_settle|Required|String(3)|The settle currency of transaction. Currently ```currency_settle``` must be same with ```currency_order```|
|vali_date|Optional|String| The expire date of credit card. Required for credit card|
|cvv2|Optional|String|The CVV/CVC2 of credit card. Required for credit card|
|id_type|Optional|String(1)| 0, ID card <br> 2, Passport <br> 3, Military Officer Certificate <br> 4, Hong Kong-Macau laissez-passer <br> 6, Mainland travel permit for Taiwan residents <br> 9, Police Officer card <br> X, other certificates |
|id_no|Optional|String| The number of User's ID card. The length need to be either 15 or 18|
|acct_name|Required|String|The name of payer, in Chinese|
|card_no|Required|String|User's card number|
|bind_mob|Required|String| User's phone number, currently only support China domestic number.|
|no_agree|Optional|String(32)| A token which represents the key payment information, refer to [Card bind document](card-bind-overview.md) for more details|

> ```id_no```, ```acct_name```, ```id_type```, ```card_no``` are ignored if ```no_agree``` is present.

###### Sample Request

```curl
curl https://fe-pay.lianlianpay.com/exchange/v1/bankcardpayapply \
-H "Content-type:application/json;charset=utf-8" \
-d '{
    	"oid_partner": "201103171000000000",
    	"dt_order": "20130515094013",
    	"time_stamp": "20130515094013"
    	"no_order": "2013051500001",
    	"busi_partner": "101001",
    	"name_goods": "羽毛球",
    	"info_order": "用户13958069593购买羽毛球3桶",
    	"money_order": "31.68",
    	"currency_order": "USD",
    	"currency_settle": "USD",
    	"notify_url": "http://payhttp.xiaofubao.com/***/back.shtml",
    	"valid_order": "30",
    	"pay_type": "D",
    	"card_no": "8888888888888888",
    	"bank_code": "01020000",
    	"acct_name": "张三",
    	"bind_mob": "13112345678",
    	"vali_date": "1402",
    	"cvv2": "627",
    	"id_type": "0",
    	"id_no": "330401198001014616",
    	"sign_type ": "RSA",
    	"sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
    }'
```

***

## Response

The response of this API is possible to include the payment result directly when your account has **No SMS Verification Flow** setup, for more details, please contact our technical support.

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. Refer to [return codes](return-codes.md)|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |

The following parameters are returned only when ```ret_code=0000```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|Fixed value, RSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|no_order|Required|String(32)|Merchant transaction No.|
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. Returned when the transaction object is created successfully. |
|currency_order|Required|String(3)|The currency used in the transaction. Refer to [supported currencies](supported-currencies.md) |
|money_order|Required|String(12)|Merchant transaction amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in the currency of ```currency_order```|
|result_pay|Optional|String| Payment result. <br> SUCCESS - Payment proceed successfully <br> PROCESSING -  Payment is processing. Return only when **No SMS Verification Flow** is setup|
|token|Optional|String| Won't return if **No SMS Verification Flow** is setup.  Used for [Exchange Payment Verify API](exchange-payment-verify-api.md)|

The following parameters are returned only when ```result_pay=SUCCESS```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|settle_date|Required|String(8)| Format YYYYMMDD. |
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|
|pay_type|Required|String(1)| The pay type used in current transaction.|
|currency_settle|Required|String(3)|The settle currency of transaction|
|money_settle|Required|String(12)|The settle amount of transaction|
|currency_pay|Required|String(3)| The currency of which the payer used. Refer to [supported currencies](supported-currencies.md)|
|money_pay|Required|String(12)|The amount of which the payer payed|
|exchange_rate|Required|String|The exchange rate of current transaction|
|no_agree|Optional|String(16)| A token which represents the key payment information, refer to [Card bind document](card-bind-overview.md) for more details |

###### Sample Response

Below is a sample response without **No SMS Verification Flow** when ```ret_code=0000```.

```json
{
	"ret_code": "0000",
	"ret_msg": "交易成功",
	"token": "D096DBA0E3E0CC8F1D504C06E71D292D",
	"oid_partner": "201103171000000000",
	"dt_order": "20130515094013",
	"no_order": "2013051500001",
	"oid_paybill": "2013051613121201",
	"money_order": "31.68",
    "currency_order": "USD",
	"sign_type": "RSA",
	"sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
}
```
