# Card Unbind API

This API is available for your customer who wants to unbind their cards.

***

## Request

###### Endpoint

```html
https: //traderapi.lianlianpay.com/bankcardunbind.htm
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|platform|Optional|String(32)|Used for sharing user information between several ```oid_partner```, do NOT use it if you are not sure, or ask help from LianLian Supports|
|pay_type|Optional|String| 2, Express Payment (Debit card) <br> 3, Express Payment (Credit card) <br> D, Verified Payment <br> By default the value is 2 |
|no_agree|Required|String| A token which represents the key payment information, refer to [Binding Card](card-bind-overview.md) for more details|

###### Sample Request

```html
curl https: //traderapi.lianlianpay.com/bankcardunbind.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{
        "oid_partner":"201103171000000000",
        "user_id":"1234567",
        "offset":"0",
        "no_agree":"2014031710002121",
        "sign_type ":"RSA",
        "sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
    }'
```

***

## Response

For this API, the unbind action is successful if you have received ```ret_code=0000```.

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. refer to [Return Codes](return-codes.md)|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |

The following parameters are returned only when ```ret_code=0000```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|