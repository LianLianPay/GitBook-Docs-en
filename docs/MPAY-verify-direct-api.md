# MPAY Verify Direct API

This API is used to do the verification step of MPAY Payment request

***

## Request

###### Endpoint

```html
https://mpayapi.lianlianpay.com/v1/bankcardprepay
```

> Direct APIs have an additional requirement which involves our payment risk control team and due diligence team. You need to contact your account manager to obtain the access.

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|no_order|Required|String(32)|Merchant transaction No.|
|token|Required|String| It'll lose effectiveness in 30 minutes. |
|sign_type|Required|String(3)|RSA or SHA256withRSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|money_order|Required|String(12)|Merchant order amount,  2 decimal places are expected,|
|verify_code|Required|String(6)|The SMS verification code sent in [MPAY Apply Direct API](MPAY-apply-direct-API.md)|


> ```id_no```, ```bind_mob,```, ```id_type```, ```card_no``` are required if ```no_agree``` is NOT present.

###### Sample Request

```curl
curl https://mpayapi.lianlianpay.com/v1/bankcardprepay\
-H "Content-type:application/json;charset=utf-8" \
-d '{
      "money_order": "55.44", 
	  "no_order": "20180828153543", 
	  "oid_partner": "201304121000001004",
	  "sign": "3DdWXwYzWuXHZ0oCc1vDDEKIMv0QpjSfAYXV6t8nTjR0QcAWvN65BL4zS+Csys1Hx1rdAciMqg4yoevq4jxHOPi4T3h1ryld51x0uUGzcyeymp8aQGtkexiZPwxLTdQ62KR7czfebwbUt5uDny5dstIZ26Bupfm3YgBFb3kdiIg=", 
	  "sign_type": "RSA", 
	  "token": "E4A27991786F7114DCAF88C266A465C2", 
	  "verify_code": "123456"
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
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: YYYYMMddHHmmss, E.g. 20170801225714|
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. |
|money_order|Required|String(12)|Merchant transaction amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected|
|token|Optional|String| It'll lose effectiveness in 30 minutes. Won't return if **No SMS Verification Flow** is setup.  Used for [MPAY Payment Verify Direct API](MPAY-verify-direct-api.md)|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|

The following parameters are returned only when ```result_pay=SUCCESS```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|settle_date|Optional|String(8)| Format YYYYMMdd. |
|no_agree|Optional|String(16)| A token which represents the key payment information |

###### Sample Response

Below is a sample response without **No SMS Verification Flow** when ```ret_code=0000```.

```json
{
    "dt_order": "20180828153543", 
	"money_order": "55.44",
	"no_agree": "2017092339974797", 
	"no_order": "20180828153543", 
	"oid_partner": "201304121000001004", 
	"oid_paybill": "2018082971158876", 
	"ret_code": "0000", 
	"ret_msg": "交易成功", 
	"settle_date": "20180829", 
	"sign": "BdKDxbxqRQlTq/aV9KxYYFV6qVAgy+Z2u5MkefDjcwsshkWK6H6WtEr59AApGKgdOU5/DD6aw0TOeZBYba+zQpAFHSmhsTsoC2tiUoi8dWNXc2rARVmC4HdSoUpyr39ICO3PL/SwqyWwmolZEu1LR5/SqYw/n7glDY0jsq2ghxA=", 
	"sign_type": "RSA"
 
}
```
