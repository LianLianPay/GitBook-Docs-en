# Aggregate Web Redirect API

This redirect API is suitable for aggregate payment on pc websites. 

***

###### Endpoint

```html
https://wallet.lianlianpay.com/aggregateweb/gateway.htm
```

The way to call this API is limited to HTML ```<form/>``` post request from client-side. Below ```<meta>```  is needed in the head of your HTML.
 
```html
<meta http-equiv="content-type" content="text/html;charset=UTF-8">
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|version|Required|String|Fixed value, ```1.1```|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|timestamp|Required|String(14)|The time when request is initialized. Format: YYYYMMDDHHMMSS, E.g. 20170801225714. The time difference between your server and LianLian server(UTC +8) should be no more than 30 mins|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|busi_partner|Required|String(6)|Fixed value. Virtual products, ```101001```; Physical products, ```109001```|
|no_order|Required|String(32)|Merchant order No.|
|dt_order|Required|String(14)|Merchant order date. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|name_goods|Required|String(40)|Product name. E.g. Pen|
|info_order|Optional|String(255)|```info_order``` will be sent back in synchronous or asynchronous notification for parameters transmission|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|
|url_return|Optional|String(128)|Online url, your customer will be redirected to ```url_return``` once they finished their payment|
|userreq_ip|Required|String(32)|The IP address of your customer, used for anti-fraud purpose. Replace "." with "_", E.g. 122_11_37_211|
|url_order|Optional|String(255)|Online url of products|
|valid_order|Optional|Int|The valid period of ```no_order```, in minute. The status of corresponding transaction will be set to "Closed" once its ```valid_order``` run out. Default: 10080 (7 days). |
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment_risk_item.md)| 
|col_oidpartner|Optional|String(18)| ```oid_partner``` of recipient, which is mainly used in LianLian E-wallet. The value is set as ```oid_partner``` to which ```user_id``` belongs by default. |
|col_userid|Optional|String(32)| ```user_id``` of recipient, which is mainly used in LianLian E-wallet. Note there is only one recipient, ```col_userid``` and ```col_oidparnter``` can not be used in a same request |
|shareing_data|Optional|String(1024)| Refer to [Sharing data introduction](#sharing-data-introduction) |
|secured_partner|Optional|String(18)|```oid_partner``` of guarantor, which is mainly used in guaranteed transactions. Refer to LianLian E-wallet documentation for more details|
|buyer_confirm_valid|Optional|Int| |
|seller_send_valid|Optional|Int| |
|id_no|Optional|String| Only ID card is supported, the length need to be either 15 or 18. Required for Verified Payment|
|acct_name|Optional|String|The name of payer, in Chinese. Required for Verified Payment |
|bank_code|Optional|String|Specify which online banking should be used instead of displaying online banking selection page. ```bank_code``` to be used with ```pay_type```. |
|pay_type|Optional|String| 1, online banking payment(debit card) <br> 8, online banking payment(credit card) <br> 9, business online banking payment <br> ```pay_type``` to be used with ```bank_code```.|

###### Sample Request

```html
<form action="ENDPOINT" method="post"> 
    <input type="text" name="version" value="1.0">
    <input type="text" name="oid_partner" value="201304121000001004">
    <input type="text" name="user_id" value="111111">
    <input type="text" name="busi_partner" value="101001">
    <input type="text" name="no_order" value="2013099129111111">
    <input type="text" name="dt_order" value="20130224120224">
    <input type="text" name="name_goods" value="test item">
    <input type="text" name="info_order" value="some info">
    <input type="text" name="money_order" value="49.95">
    <input type="text" name="notify_url" value="http://domain:port/notify">
    <input type="text" name="url_return" value="http://domain:port/return">
    <input type="text" name="userreq_ip" value="*_*_*_*">
    <input type="text" name="risk_item" value='{"user_info_bind_phone":"13958069593","user_info_dt_register":"20131030122130","frms_ware_category":"1009","request_imei":211,"request_imsi":121121,"request_ip":"192.168.20.110"}'>
    <input type="text" name="url_order" value="http://domain:port/orderUrl">
    <input type="text" name="sign_type" value="RSA">
    <input type="text" name="sign" value="YOUR-RSA-SIGN-RESULT">
    <input type="submit" value="checkout">
</form>
```

###### Sharing Data Introduction

Parameter ```shareing_data``` is mainly used for revenue sharing. Max recipients: 3. 

```html
Sharing_account^Sharing_account_type^Sharing_amount^Sharing_description|Sharing_account^Sharing_account_type^Sharing_amount^Sharing_description|Sharing_account^Sharing_account_type^Sharing_amount^Sharing_description
```
* ```Sharing_account```, can either be ```user_id``` or ```oid_partner``` .
* ```Sharing_account_type```, fixed value. 0 for LianLian E-wallet merchant, 1 for regular merchant.
* ```Sharing_amount```, revenue sharing amount, 2 decimal places are expected, in CNY.
* ```Sharing_description```, a short description for revenue sharing, no "^" nor "|", length: 85.

***

## Aggregate Payment Synchronous Notification

Payment synchronous notification, a HTTP POST request, will be sent to ```url_return``` whenever the payment is confirmed as successful. 

> Synchronous notification sends only once, we recommend you to use [asynchronous notification](aggregate-asyn-notification.md) or [payment result query API](aggregate-payment-result-query.md) to obtain payment result.

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|no_order|Required|String(32)|Merchant order No.|
|dt_order|Required|String(14)|Merchant order date. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|result_pay|Required|String| Payment result. E.g. SUCCESS|
|settle_date|Optional|String(8)| Format YYYYMMDD. Returns when payment is successful|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|
|pay_type|Optional|String| The payment method used in this transaction. <br> 0, balance payment <br> 1, online banking payment (debit card) <br> 8, online banking payment (credit card) <br> 9, business online banking payment <br> 2, express payment (debit card) <br> 3, express payment (credit card)<br> D, verified payment <br> I, WeChat Payment <br> L, Alipay Payment| 
|bank_code|Optional|String| Short codes of banks |

There is no need to send response for synchronous notification.