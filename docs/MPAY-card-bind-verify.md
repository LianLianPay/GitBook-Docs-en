# Card Bind Verify API

This API verify the SMS verification code, token and then do the card bind action, it needs to be used in conjunction with [Card Bind Apply API](MPAY-card-bind-apply.md) to complete card bind action. 

***

## Request

###### Endpoint

```text
https://mpayapi.lianlianpay.com/v1/bankcardbindverfy
```

###### Parameter

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA or SHA256withRSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|token|Required|String| The token returned in [MPAY Card Bind Apply API](MPAY-card-bind-apply.md)|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|verify_code|Required|String|The SMS verification code which is sent out by [MPAY Card Bind Apply API](MPAY-card-bind-apply.md)|
|no_order|Optional|String| Card binding application serial number.|

###### Sample Request

```curl
curl https://mpayapi.lianlianpay.com/v1/bankcardbindverfy \
-H "Content-type:application/json;charset=utf-8" \
-d '{
    	"oid_partner": "201103171000000000",
    	"token": "D096DBA0E3E0CC8F1D504C06E71D292D",
    	"user_id": "2013051500001",
    	"verify_code": "666666",
    	"sign_type": "RSA",
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
|sign_type|Required|String(3)|RSA or SHA256withRSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|card_no|Required|String(4)| The card no used in original request. |
|no_agree|Optional|String(16)| A token which represents the key payment information |
|no_order|Optional|String| Card binding application serial number.|

###### Sample Response

```json
{
	"ret_code":"0000", 
	"ret_msg":交易成功", 
	"oid_partner":"201103171000000000", 
	"user_id":"2013051500001", 
	"card_no":"622202112313213123", 
	"no_agree":"2013051613121201", 
	"sign_type":"RSA", 
	"sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
	}
```