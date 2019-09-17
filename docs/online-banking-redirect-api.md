# Request payment

The way to call this API is limited to HTML <form/> post request from client-side.The user will be redirected to Lianlian payment page.

***

## Request Parameters and sample

Below <meta> is needed in the head of your HTML：

```html
<meta http-equiv="content-type" content="text/html;charset=UTF-8">
```

###### Endpoint

```html
https://payment.lianlianpay.com/payment/bankgateway.htm
```

###### Request Parameters
|Name|Required|Type|Description|
|:---|:---|:---|:---|
|version|Required|String(6)|Fixed value, 1.0|
|charset_name|Required|String(18)|Encoding format of merchat website,support UTF-8(default),GBK,GB2312 and GB18030.|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|timestamp|Required|String(14)|The time when request is initialized. Format: yyyyMMddHHmmss, E.g. 20170801225714. The time difference between your server and LianLian server(UTC +8) should be no more than 30 mins|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|busi_partner|Required|String(6)|Fixed value. Virtual products, 101001; Physical products, 109001|
|no_order|Required|String(32)|Merchant order No.|
|dt_order|Required|String(14)|Merchant order date. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|name_goods|Required|String(40)|Product name. E.g. Pen|
|info_order|Optional|String(255)|```info_order``` will be sent back in asynchronous notification for parameters transmission|
|money_order|Required|String(12)|Merchant order amount, 2 decimal places are expected, in CNY|
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|
|url_return|Optional|String(128)|Online url, your customer will be redirected to ```url_return``` once they finished their payment|
|userreq_ip|Optional|String(32)|The IP address of your customer, used for anti-fraud purpose. Replace "." with "_", E.g. 122_11_37_211|
|url_order|Optional|String(255)|Online url of products|
|valid_order|Optional|Int|The valid period of ```no_order```, in minute. The status of corresponding transaction will be set to "Closed" once its ```valid_order``` run out. Default: 10080 (7 days). |
|bank_code|Optional|String(18)|The bank short code of used card.When it's NOT null,the user will be redirected to corresponding online banking page from merchant page.|
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment_risk_item.md)| 
|pay_type|Optional|String| The payment method used in this transaction. <br> 1, online banking payment (debit card) <br> 8, online banking payment (credit card) <br> 9, business online banking payment <br> |


###### Sample Request

```html
<form action="https://payment.lianlianpay.com/payment/bankgateway.htm" method="post"> 
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