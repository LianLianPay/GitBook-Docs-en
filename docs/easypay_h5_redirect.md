# Easy Payment H5 Redirect API

###### Endpoint

```html
https://wap.lianlianpay.com/travlepay.htm
```

The way to call this API is limited to HTML ```<form/>``` post request from client-side. All of the required parameters should be embedded into a ```<input>``` tag whose name is ```req_data```:

```html
<form action="https://wap.lianlianpay.com/travlepay.htm" method="POST">
	<input name="req_data" value='{$value}'>
</form> 
```



###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|version|Required|String|Fixed value, ```1.0```|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|platform|Optional|String(32)| ```platform``` is used for sharing user info between multiple ```oid_partner```, this requires additional settings from LianLian side|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|app_request|Required|String(1)| 1, Android <br> 2, iOS <br> 3, H5|
|timestamp|Required|String(14)|The time when request is initialized. Format: YYYYMMDDHHMMSS, E.g. 20170801225714. The time difference between your server and LianLian server(UTC +8) should be no more than 30 mins|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|bg_color|Optional|String(6)|The background color of payment pages. Range: ```000000``` ~ ```ffffff```. By default is ```ff5001```|
|font_color|Optional|String(6)|The font color of payment pages. Range: ```000000``` ~ ```ffffff```|
|syschnotify_flag|Optional|String(1)| 0, Redirect only when the return button is clicked <br> 1, Redirect automatically <br> How the user is redirected after completing payments, by default is 0|
|busi_partner|Required|String(6)|Fixed value. Virtual products, ```101001```; Physical products, ```109001```|
|no_order|Required|String(32)|Merchant order No.|
|dt_order|Required|String(14)|Merchant order date. Format: YYYYMMDDHHMMSS, E.g. 20170801225714|
|name_goods|Required|String(40)|Product name. E.g. Pen|
|info_order|Optional|String(255)|```info_order``` will be sent back in synchronous or asynchronous notification for parameters transmission|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|
|url_return|Optional|String(128)|Online url, your customer will be redirected to ```url_return``` once they finished their payment|
|back_url|Optional|String(128)| The url where user is redirected when they need to change cards. For [payment info preset](easypay.md#payment-info-preset) only|
|no_agree|Optional|String(32)| A token which represents the key payment information, refer to [Binding Card](easypay.md) for more details|
|valid_order|Optional|Int|The valid period of ```no_order```, in minute. The status of corresponding transaction will be set to "Closed" once its ```valid_order``` run out. Default: 10080 (7 days). |
|pay_type|Required|String| M, regular payments <br> F, authorization, used for credit card only. An error throws out if the used card is not a credit card|
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment_risk_item.md)| 


> ```id_no```, ```acct_name```, ```id_type```, ```card_no``` are ignored if ```no_agree``` is present.

###### Sample Request

```html
<form action="https://wap.lianlianpay.com/travlepay.htm" method="post"> 
	<input name="req_data" value='{
		"oid_partner":"201103171000000000",
		"dt_order":"20130515094013",
		"no_order":"2013051500001",
		"busi_partner":"101001",
		"name_goods":"羽毛球",
		"info_order":"用户13958069593购买羽毛球3桶",
		"money_order":"210.97",
		"notify_url":"http://.../notify_url.shtml",
		"url_return":"http://.../return_url.htm",
		"back_url":"http://.../back_url.htm",
		"risk_item":"{\"frms_ware_category\":\"2009\",\"user_info_mercht_userno\":\"123456\",\"user_info_dt_register\":\"20141015165530\",\"user_info_full_name\":\"张三\",\"user_info_id_no\":\"3306821990012121221\",\"user_info_identify_type\":\"1\",\"user_info_identify_state\":\"1\"}",
		"valid_order":"30",
		"sign_type ":"RSA",
		"sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
		}'/>
</form>
```

***

## Synchronous Notification

Payment synchronous notification, a HTTP POST request, will be sent to ```url_return``` whenever the payment or the authorization is confirmed as successful. No synchronous notification is sent for failed transactions or transactions with exception. 

> Synchronous notification sends only once, we recommend you to use [asynchronous notification](asyn_notification.md) or [payment result query API](easypay_payment_result_query.md) to obtain payment result.

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

There is no need to send response for synchronous notification.

###### Sample Synchronous Notification

```html
{url_return}?res_data={
	"oid_partner":"201103171000000000",
	"dt_order":"20130515094013",
	"no_order":"2013051500001",
	"oid_paybill":"2013051613121201",
	"money_order":"210.97",
	"result_pay":"SUCCESS",
	"settle_date":"20130516",
	"sign_type":"RSA", 
	"sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
}
```