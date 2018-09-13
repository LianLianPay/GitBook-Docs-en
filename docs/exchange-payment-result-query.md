# Exchange Payment Result Query API

This API is available for you to query the status of a specific transaction of Exchange Payment. Before we start, you need to learn the change of transaction status.

***

## The Change of Transaction Status

There are 6 statuses for an Express Payment transaction object:

* **CREATED**， this status is the initial transaction status, which is changed to **WAITING** immediately after transaction object creation。

* **PROCESSING**， which means the transaction is processing by our gateways. Generally this status lasts for 3 minute, if we can't obtain the result from our gateways during this period, it would be changed to **WAITING**. In this case, we would setup a [transaction recovery mechanism](#transaction-recovery-mechanism) for corresponding transaction.

* **WAITING**，there are several cases for this transaction status:
    * The payer never complete the verification step. 
    * The payer have finished the verification step  but we haven't obtain the payment result from our gateways.

* **SUCCESS**， the transaction is paid successfully.

* **REFUND**， the transaction is refunded。

* **FAILURE**， the transaction is never paid hence it is closed as the validity period runs out. The validity period is set by parameter ```valid_order``` in [Express Payment Apply API](exchange-payment-apply-api.md).

> All the transaction status other than **SUCCESS** can only be obtained by [Card Bin Query API](#request).

***

## Transaction Recovery Mechanism

Transaction recovery mechanism is used for rare cases whenever LianLian submit a payment request to our gateways but never get the expect response. In this case, a polling is performed to keep asking for the payment results, which is the major of transaction recovery mechanism. 

> By default, the transaction recovery mechanism would expand the validity period of one transaction to 7 days. Contact our technical support if you want it to be shorter. The recommended minimum period is 2 hours.

According to the validity period of one transaction, there are 2 cases if transaction recovery mechanism obtain the payment result successfully:

* The transaction is still in its validity period. Its status would be changed to **SUCCESS** and we do the relevant logic.

* The transaction is closed. In this case, we **will not change** the transaction status and do refund to the payer directly.

***

## Request

###### Endpoint

```text
https://queryapi.lianlianpay.com/exchangeorderquery.htm
```

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|no_order|Required|String(32)|Merchant transaction No.|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|

###### Sample Request

```curl
curl https://queryapi.lianlianpay.com/exchangeorderquery.htm \
-H "Content-type: application/json;charset=utf-8" \
-d '{
    	"oid_partner": "201103171000000000",
    	"no_order": "2013051500001",
    	"sign_type ": "RSA",
    	"sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
    }'
```

***

## Response

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
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: YYYYMMddHHmmss, E.g. 20170801225714|
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|result_pay|Required|String| Payment result. <br> SUCCESS  <br> PROCESSING <br> WAITING <br> REFUND <br> FAILURE|
|memo|Optional|String|The remark of transaction, this field may include the failed reason of the transaction|
|currency_order|Required|String(3)|The currency used in the transaction. Refer to [supported currencies](supported-currencies.md) |
|money_order|Required|String(12)|Merchant transaction amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in the currency of ```currency_order```|


The following parameters are returned only when ```result_pay=SUCCESS```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|settle_date|Optional|String(8)| Format YYYYMMDD. |
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|
|pay_type|Optional|String(1)| The pay type used in current transaction.|
|currency_settle|Required|String(3)|The settle currency of transaction|
|money_settle|Required|String(12)|The settle amount of transaction|
|currency_pay|Required|String(3)| The currency of which the payer used. Refer to [supported currencies](supported-currencies.md)|
|money_pay|Required|String(12)|The amount of which the payer payed|
|exchange_rate|Required|String|The exchange rate of current transaction|
|no_agree|Optional|String(16)| A token which represents the key payment information, refer to [Card binding document](card-bind-overview.md) for more details |

###### Sample Response

Below is a sample whose ```result_pay=SUCCESS```:

```json
{
	"oid_partner": "201103171000000000",
	"dt_order": "20130515094013",
	"no_order": "2013051500001",
	"oid_paybill": "2013051613121201",
	"money_order": "31.68",
	"currency_order": "USD",
	"money_pay": "210.97",
	"currency_pay": "CNY",
	"money_sellte": "31.47",
	"currency_sellte": "USD",
	"exchange_rate": "6.66",
	"result_pay": "SUCCESS",
	"settle_date": "20130516",
	"info_order": "用户13958069593购买了3桶羽毛球",
	"pay_type": "D",
	"bank_code": "01020000",
	"ret_code": "0000",
	"ret_msg": "交易成功",
	"sign_type": "RSA",
	"sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
}
```