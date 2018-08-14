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

<param type='docs' category='request' list='version_1.0, charset_name, oid_partner, user_id, timestamp, sign_type, sign, busi_partner, no_order, dt_order, name_goods, info_order, money_order, notify_url, url_return, userreq_ip, url_order, valid_order, bank_code, pay_type, sharing_data, risk_item'></param>

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