# Easy Payment Web Redirect API

###### Endpoint

```html
https://payment.lianlianpay.com/payment/traveleasypay.htm
```

The way to call this API is limited to HTML ```<form/>``` post request from client-side. Below ```<meta>```  is needed in the head of your HTML.
 
```html
<meta http-equiv="content-type" content="text/html;charset=UTF-8">
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|version|Required|String|Fixed value, ```1.0```|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|platform|Optional|String(32)| ```platform``` is used for sharing user info between multiple ```oid_partner```, this requires additional settings from LianLian side|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|timestamp|Required|String(14)|The time when request is initialized. Format: YYYYMMddHHmmss, E.g. 20170801225714. The time difference between your server and LianLian server(UTC +8) should be no more than 30 mins|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|busi_partner|Required|String(6)|Fixed value. Virtual products, ```101001```; Physical products, ```109001```|
|no_order|Required|String(32)|Merchant order No.|
|dt_order|Required|String(14)|Merchant order date. Format: YYYYMMddHHmmss, E.g. 20170801225714|
|name_goods|Required|String(40)|Product name. E.g. Pen|
|info_order|Optional|String(255)|```info_order``` will be sent back in synchronous or asynchronous notification for parameters transmission|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|
|url_return|Optional|String(128)|Online url, your customer will be redirected to ```url_return``` once they finished their payment|
|userreq_ip|Required|String(32)|The IP address of your customer, used for anti-fraud purpose. Replace "." with "_", E.g. 122_11_37_211|
|url_order|Optional|String(255)|Online url of products|
|valid_order|Optional|Int|The valid period of ```no_order```, in minute. The status of corresponding transaction will be set to "Closed" once its ```valid_order``` run out. Default: 10080 (7 days). |
|pay_type|Required|String| M, regular payments <br> F, authorization, used for credit card only. An error throws out if the used card is not a credit card|
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment_risk_item.md)| 
|back_url|Optional|String(128)| The url where user is redirected when they need to change cards. For [payment info preset](easypay.md#payment-info-preset) only|
|no_agree|Optional|String(32)| A token which represents the key payment information, refer to [Binding Card](easypay.md) for more details|
|acct_name|Optional|String|The name of payer, in Chinese|
|card_no|Optional|String|User's card number|
|id_type|Optional|String(1)| 0, ID card <br> 2, Passport <br> 3, Military Officer Certificate <br> 4, Hong Kong-Macau laissez-passer <br> 6, Mainland travel permit for Taiwan residents <br> 9, Police Officer card <br> X, other certificates |
|id_no|Optional|String| The number of User's ID card. The length need to be either 15 or 18|



###### Sample Request

```html
<form action="https://payment.lianlianpay.com/payment/traveleasypay.htm" method="post"> 
    <input type="text" name="version" value="1.0">
    <input type="text" name="oid_partner" value="201304121000001004">
    <input type="text" name="user_id" value="111111">
    <input type="text" name="timestamp" value="20180301175839">
    <input type="text" name="busi_partner" value="101001">
    <input type="text" name="no_order" value="2013099129111111">
    <input type="text" name="dt_order" value="20130224120224">
    <input type="text" name="name_goods" value="测试商品">
    <input type="text" name="info_order" value="测试商品信息">
    <input type="text" name="money_order" value="49.95">
    <input type="text" name="notify_url" value="http://domain:port/notify">
    <input type="text" name="url_return" value="http://domain:port/return">
    <input type="text" name="userreq_ip" value="*.*.*.*">
    <input type="text" name="pay_type" value="M">
    <input type="text" name="sign_type" value="RSA">
    <input type="text" name="sign" value="RSA签名结果">
    <input type="submit" value="立即支付">
</form>
```

***

## Synchronous Notification

Payment synchronous notification, a HTTP POST request, will be sent to ```url_return``` whenever the payment or the authorization is confirmed as successful. No synchronous notification is sent for failed transactions or transactions with exception. 

> Synchronous notification sends only once, we recommend you to use [asynchronous notification](asyn_notification.md) or [payment result query API](aggregate_payment_result_query.md) to obtain payment result.

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code. Refer to [return codes](return_code.md)|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|no_order|Required|String(32)|Merchant order No.|
|dt_order|Required|String(14)|Merchant order date. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|oid_paybill|Required|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|settle_date|Optional|String(8)| Format YYYYMMDD. Returns when payment is successful|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|
|pay_type|Optional|String| The payment method used in this transaction. <br> 0, balance payment <br> 1, online banking payment (debit card) <br> 8, online banking payment (credit card) <br> 9, business online banking payment <br> 2, express payment (debit card) <br> 3, express payment (credit card)<br> D, verified payment <br> I, WeChat Payment <br> L, Alipay Payment| 
|bank_code|Optional|String| Short codes of banks |

There is no need to send response for synchronous notification.

###### Sample Synchronous Notification

```html
<form action="<url_return>" method="post"> 
	<input type="text" name="ret_code" value="0000">
	<input type="text" name="ret_msg" value="支付成功">
	<input type="text" name="oid_partner" value="201304121000001004">
	<input type="text" name="sign_type" value="RSA">
	<input type="text" name="sign" value="{sign}">
	<input type="text" name="dt_order" value="20130224120224">
	<input type="text" name="no_order" value="2013099129111111">
	<input type="text" name="oid_paybill" value="2013099129232322">
	<input type="text" name="money_order" value="49.95">
	<input type="text" name="settle_date" value="20140109">
	<input type="text" name="info_order" value="备注">
	<input type="text" name="pay_type" value="M">
	<input type="text" name="bank_code" value="01020000">
</form>
```