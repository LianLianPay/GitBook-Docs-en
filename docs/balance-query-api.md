# Balance Query API

This API is used to query the balance in merchant account. 


***

## Request

###### Endpoint

```
https://traderapi.lianlianpay.com/traderAcctQuery.htm
```

###### Parameter

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|api_version|Required|String(6)|1.1|

###### Sample Request

```curl
curl https://traderapi.lianlianpay.com/traderAcctQuery.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{
      "oid_partner": "201103171000000000",
      "api_version": "1.0",
      "sign_type": "RSA",
      "sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vP TfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
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
|amt_unsettle|Required|String|The unsettled amount. It can used for refund payment.|
|amt_balance|Required|String|The available balance of the merchant. It can be used for money transfer.|

###### Sample Response

```json
{   
    "amt_unsettle": "100.0",
    "amt_balance": "100.0",
    "ret_code": "0000",
    "ret_msg": "交易成功",		
    "sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk=",
    "sign_type": "RSA" 
} 
```
