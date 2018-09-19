# Easypay  Apply Direct API

This API is used to apply card Payment, create corresponding transaction object in LianLian's system and send SMS verification code.

***

## Request

###### Endpoint

```html
https://traderapi.lianlianpay.com/easypay.htm
```

> Direct APIs have an additional requirement which involves our payment risk control team and due diligence team. You need to contact your account manager to obtain the access.

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|api_version|Required|String(6)|Fixed value, ```1.0```|
|sign_type|Required|String(3)|RSA or SHA256withRSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|platform|Optional|String(32)| ```platform``` is used for sharing user info between multiple ```oid_partner```, this requires additional settings from LianLian side|
|busi_partner|Required|String(6)|Fixed value. Virtual products, ```101001```; Physical products, ```109001```|
|no_order|Required|String(32)|Merchant transaction No.|
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|name_goods|Required|String(40)|Product name. E.g. Pen|
|info_order|Optional|String(255)|```info_order``` will be sent back in synchronous or asynchronous notification for parameters transmission|
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|
|valid_order|Optional|Int|The valid period of ```no_order```, in minute. The status of corresponding transaction will be set to "Closed" once its ```valid_order``` run out. Default: 10080 (7 days). |
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment-risk-item.md)| 
|pay_type|Required|String| M, Easy payment - Debit card <br>  F, Easy payment - pre authorization|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected,|
|vali_date|Optional|String| The expire date of credit card. Required for credit card|
|cvv2|Optional|String|The CVV/CVC2 of credit card. Required for credit card|
|id_type|Required|String(1)|0, ID card <br> 2, Passport <br> 3, Military Officer Certificate <br> 4, Hong Kong-Macau laissez-passer <br> 6, Mainland travel permit for Taiwan residents <br> 9, Police Officer card <br> X, other certificates |
|id_no|Required|String| The number of User's ID card. The length need to be either 15 or 18|
|acct_name|Required|String|The name of payer, in Chinese|
|card_no|Required|String|User's card number|
|bind_mob|Required|String| User's phone number, currently only support China domestic number.|
|no_agree|Optional|String(32)| A token which represents the key payment information, refer to [Card bind document](card-bind-overview.md) for more details|
|bank_code|Optional|String(18)|The bank short code of used card.|
|client_chnl|Optional|String|client type<br>10：APP<br>13：PC<br>16：WAP(H5)<br>18：IVR|

> ```id_no```, ```bind_mob,```, ```id_type```, ```card_no``` are required if ```no_agree``` is NOT present.

###### Sample Request

```curl
curl https://traderapi.lianlianpay.com/easypay.htm\
-H "Content-type:application/json;charset=utf-8" \
-d '{
    	"api_version": "1.0",
		"bank_code": "01020000",
		"oid_partner": "201310102000003524",
    	"dt_order": "20150911145230",
    	"time_stamp": "20130515094013"
    	"no_order": "2013051500001",
    	"busi_partner": "101001",
    	"name_goods": "表",
    	"info_order": "用户13958069593购买羽毛球3桶",
    	"money_order": "31.68",
		"client_chnl": "13",
    	"notify_url": "http://payhttp.xiaofubao.com/***/back.shtml",
    	"valid_order": "10080",
    	"pay_type": "M",
    	"card_no": "6222081202008631114",
    	"bank_code": "01020000",
    	"acct_name": "测试用户",
    	"bind_mob": "18667140528",
    	"vali_date": "1402",
    	"cvv2": "221",
    	"id_type": "0",
    	"id_no": "339005198901194918",
		"risk_item": "{\"frms_ware_category\":\"2009\",\"user_info_mercht_userno\":\"123456\",\"user_info_dt_register\":\"20141015165530\"}",
    	"sign_type ": "RSA",
    	"sign": "mwSbuKNVqDZe5Bs1bjK8sRWnwfZD605I5JqqormozxCjQBf0tmH+yDo0lG5ovgYBSO7Fwv21vBoyr+Aq7dwLVmGbEzjJgcqXYYLeXrexNKpNepXCAE+CDBHgbOLujBPpNIiEf+sr1ABXN69HjDvKaz0SS8qt6aMNHda2HhCZapU="
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
|sign_type|Required|String(3)|Fixed value, RSA or SHA256WITHRSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|no_order|Required|String(32)|Merchant transaction No.|
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. Returned when the transaction object is created successfully. |
|money_order|Required|String(12)|Merchant transaction amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in the currency of ```currency_order```|
|result_pay|Optional|String| Payment result. <br> SUCCESS - Payment proceed successfully <br> PROCESSING -  Payment is processing. Return only when **No SMS Verification Flow** is setup|
|token|Optional|String| It'll lose effectiveness in 30 minutes. Won't return if **No SMS Verification Flow** is setup.  Used for [Easypay Verify Direct API](easypay-verify-direct-api.md)|
|name_goods|Required|String(40)|Product name. E.g. Pen|
|pay_type|Optional|String(1)| The pay type used in current transaction.|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|
|bank_code|Optional|String(18)|The bank short code of used card.|

The following parameters are returned only when ```result_pay=SUCCESS```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|settle_date|Optional|String(8)| Format YYYYMMdd. |
|no_agree|Optional|String(16)| A token which represents the key payment information, refer to [Card bind document](card-bind-overview.md) for more details |

###### Sample Response

Below is a sample response without **No SMS Verification Flow** when ```ret_code=0000```.

```json
{
	"ret_code": "0000",
	"ret_msg": "交易成功",
	"name_goods": "表",
	"token": "4a08441e066f06e65f902a5993a307c4",
	"oid_partner": "201310102000003524",
	"dt_order": "20150911145230",
	"no_order": "20150911145230172",
	"oid_paybill": "2013051613121201",
	"money_order": "31.68",
	"sign_type": "RSA",
	"sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
}
```
