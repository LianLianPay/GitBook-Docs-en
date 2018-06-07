# Card Bin Query API

This API is useful to obtain the basic information of a card which is issued by UnionPay organization.

***

## Request

###### Endpoint

```html
https://queryapi.lianlianpay.com/bankcardbin.htm
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|api_version|Optional|String(6)| Fixed value: 1.0|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|card_no|Required|String| Card number|

###### Sample Request

```html
curl https://queryapi.lianlianpay.com/bankcardbin.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{
        "oid_partner":"201103171000000000",
        "card_no":"62258888888888888",
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
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|card_no|Required|String(4)| Last 4 digits of used card |
|bank_code|Required|String(8) | The bank short code of used card |
|bank_name|Required|String(32)| The bank name of used card, in Chinese|
|card_type|Required|String(1) | 2, debit card <br> 3, credit card|