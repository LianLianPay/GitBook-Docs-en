# Card Bind Query API

This API is useful to obtain all the card bind records of a user identification. 

***

## Request

###### Endpoint

```html
https://queryapi.lianlianpay.com/bankcardbindlist.htm
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|platform|Optional|String(32)|Used for sharing user information between several ```oid_partner```, do NOT use it if you are not sure, or ask help from LianLian Supports|
|pay_type|Optional|String| 2, Express Payment (Debit card) <br> 3, Express Payment (Credit card) <br> D, Verified Payment <br>  M, regular payments <br> F, authorization, used for credit card only. An error throws out if the used card is not a credit card<br>By default the value is 2 |
|no_agree|Optional|String| Permanent token. Only the information which belongs to ```no_agree``` would be returned if it is present |
|card_no|Optional|String| Card number. Only the information which belongs to ```card_no``` would be returned if it is present |
|offset|Required|String(8)| The offset of query result. Generally 0 is enough. Possible value: 0, 1, 2, 3, ... |

###### Sample Request

```html
curl https://queryapi.lianlianpay.com/bankcardbindlist.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{
        "oid_partner":"201103171000000000",
        "dt_refund":"20130515094018",
        "no_refund":"2013051500005",
        "oid_refundno":"2013051613121201",
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
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|count|Required|String(8)|The count of ```no_agree``` |
|aggrement_list|Optional|List| A list of ```no_agree``` which belongs to the current ```user_id```, in reverse chronological order |

Below parameters are included in ```agreement_list```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|no_agree|Required|String(16)| A token which represents the key payment information, refer to [Card Bind document](card-bind-overview.md) for more details |
|card_no|Required|String(4)| Last 4 digits of used card |
|bank_code|Required|String(8) | The bank short code of used card |
|bank_name|Required|String(32)| The bank name of used card, in Chinese|
|card_type|Required|String(1) | 2, debit card <br> 3, credit card|
|bind_mobile|Required|String(11)| Masked user phone number |
