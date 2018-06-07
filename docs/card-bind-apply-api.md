# Card Bind Apply API

This API sends request to LianLian to initialize card bind request and send out SMS verification code, it needs to be used in conjunction with [Card Bind Verify API](card-bind-verify-api.md) to complete card bind action. 

***

## Request

###### Endpoint

```text
https://traderapi.lianlianpay.com/bankcardbind.htm
```

###### Parameter

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|platform|Optional|String(32)| ```platform``` is used for sharing user info between multiple ```oid_partner```, this requires additional settings from LianLian side|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|pay_type|Required|String| M, Easy payment - Debit card <br> N, Easy payment - Credit card <br> D, Authenticate Payment|
|api_version|Required|String(6)|Fixed value, ```2.1```|
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment_risk_item.md)| 
|vali_date|Optional|String| The expire date of credit card. Required for credit card|
|cvv2|Optional|String|The CVV/CVC2 of credit card. Required for credit card|
|id_type|Required|String(1)| 0, ID card <br> 2, Passport <br> 3, Military Officer Certificate <br> 4, Hong Kong-Macau laissez-passer <br> 6, Mainland travel permit for Taiwan residents <br> 9, Police Officer card <br> X, other certificates |
|id_no|Required|String| The number of User's ID card. The length need to be either 15 or 18|
|acct_name|Required|String|The name of payer, in Chinese|
|card_no|Required|String|User's card number|
|bind_mob|Required|String| User's phone number, currently only support China domestic number.|

###### Sample Request

```curl
curl https://traderapi.lianlianpay.com/bankcardbind.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{
    	"card_no": "8888888888888888",
    	"acct_name": "测试",
    	"bind_mob": "13958069593",
    	"id_type": "0",
    	"id_no": "330401198001014616",
    	"sign_type ": "RSA",
    	"sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
    }'
```

***

## Response

###### Parameter

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. Refer to [return codes](return-codes.md)|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |

The following parameters are returned only when ```ret_code=0000```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|token|Optional|String| Used for [Card Bind Verify API](card-bind-verify-api.md)|

###### Sample Response

```json
{
	"ret_code": "0000",
	"ret_msg": "交易成功",
	"token": "D096DBA0E3E0CC8F1D504C06E71D292D",
	"oid_partner": "201103171000000000",
	"user_id ": "20130515094013",
	"sign_type": "RSA",
	"sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
}
```
