# Instant Payment asynchronous notification

This asynchronous notification is sent out to the ```notify_url``` of the original request when a transaction is confirmed as ```SUCCESS``` or ```FAILURE``` in our system. 

> Refer to [asynchronous notification](async-notification-concept.md) for the introduction of its concept and its requirements.


***

## Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|dt_order|Required|String(14)|Merchant order date. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|no_order|Required|String(32)|Merchant order No.|
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests or the payment result is ```FAILURE```.|
|result_pay|Required|String| Payment result. E.g. SUCCESS|
|settle_date|Optional|String(8)| Format YYYYMMDD. Returns when payment is successful|

###### Sample asynchronous notification

```html
curl ${notify_url} \
-H "Content-type:text/json;charset=utf-8" \
-d '{
      "oid_partner": "201103171000000000",
      "dt_order": "20130515094013",
      "no_order": "2013051500001",
      "oid_paybill": "2013051613121201",
      "money_order": "210.97",
      "result_pay": "SUCCESS",
      "settle_date": "20130516",
      "info_order": "withdrawal",
      "sign_type": "RSA",
      "sign": "ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
    }'
```

***


