# Easypay Verify Direct API

This API is used to do the verification step of Easypay Payment request

***

## Request

###### Endpoint

```html
https://traderapi.lianlianpay.com/easypayverify.htm
```

> Direct APIs have an additional requirement which involves our payment risk control team and due diligence team. You need to contact your account manager to obtain the access.

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|sign_type|Required|String(3)|RSA or SHA256withRSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|no_order|Required|String(32)|Merchant transaction No.|
|token|Required|String| It'll lose effectiveness in 30 minutes. |
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: YYYYMMddHHmmss, E.g. 20170801225714|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected,|
|verify_code|Required|String(6)|The SMS verification code sent in [Easypay Apply Direct API](easypay-apply-direct-API.md)|


> ```id_no```, ```bind_mob,```, ```id_type```, ```card_no``` are required if ```no_agree``` is NOT present.

###### Sample Request

```curl
curl https://traderapi.lianlianpay.com/easypayverify.htm\
-H "Content-type:application/json;charset=utf-8" \
-d '{
		"oid_partner": "201310102000003524",
    	"no_order": "2013051500001",
    	"money_order": "31.68",
	    "token": "4a08441e066f06e65f902a5993a307c4",
        "verify_code": "123456"
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
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: YYYYMMddHHmmss, E.g. 20170801225714|
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. |
|currency_order|Required|String(3)|The currency used in the transaction. Refer to [supported currencies](supported-currencies.md) |
|money_order|Required|String(12)|Merchant transaction amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected|
|result_pay|Optional|String| Payment result. <br> SUCCESS - Payment proceed successfully <br> PROCESSING -  Payment is processing. Return only when **No SMS Verification Flow** is setup|
|token|Optional|String| It'll lose effectiveness in 30 minutes. Won't return if **No SMS Verification Flow** is setup.  Used for [Easypay Payment Verify Direct API](easypay-verify-direct-api.md)|
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
    "info_order": "商品名称",
    "money_order": "0.01",
    "no_agree": "2015090900197005",
    "no_order": "20150911145230172",
    "oid_partner": "201310102000003524",
    "oid_paybill": "2015091100334077",
    "ret_code": "0000",
    "ret_msg": "交易成功",
    "settle_date": "20150911",
    "sign": "dsC9m7JWJAjvnwLbnEqr3RUMr8FgvLrNC4b4ah91uVGOo81Djki3rIvt6gyFMN3A57D2VGGyMd5C11QRnkLBhimYBeTIIVeUrD/mfMrSJaNvwl76kDECQNxU/lmtPlC+WkRLlHbZnWjI/wHLKbVSZymeTC090tOPfiEhKZI4OLU=",
    "sign_type": "RSA"

}
```
