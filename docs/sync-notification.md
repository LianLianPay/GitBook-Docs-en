# Synchronous Notification

Lianlian will send synchronous notification to the ```url_return``` which is send in payment request after the payment is confirmed as successful.

Characteristics: 

* Lianlian wouldn't send any synchronous notification if the payment is failed or abnormal.

* Synchronous notification sends only once.In extreme cases, if your user turns off the payment page without redirecting to ```url_return```, the synchronization notification wouldn't send out.

We recommend you to use [asynchronous notification](/docs/aggregate-asyn-notification.md)； 

> Some banks, such as ICBC, do not support redirection to ```url_return```. In this particular case, there will be no synchronous notification. 

***

## Request Parameters and sample

Payment synchronous notification, a HTTP POST request in a form format, will be sent to ```url_return``` whenever the payment is confirmed as successful.

###### Request Parameters

<param type='docs' category='request' list='oid_partner, sign_type, sign, dt_order, no_order_notification, oid_paybill, money_order, result_pay_sync_notification, settle_date'></param>

###### Sample Request

```html
<form action="${url_return}" method="post"> 
	<input type="text" name="oid_partner" value="201304121000001004">
	<input type="text" name="sign_type" value="">
	<input type="text" name="sign" value="4a1123e3f123138df813ab81de8">
	<input type="text" name="dt_order" value="20130224120224">
	<input type="text" name="no_order" value="2013099129111111">
	<input type="text" name="oid_paybill" value="2013099129232322">
	<input type="text" name="money_order" value="0.01">
	<input type="text" name="result_pay" value="SUCCESS">
	<input type="text" name="settle_date" value="20140109">
	<input type="text" name="info_order" value="Remarks">
	<input type="text" name="pay_type" value="1">
	<input type="text" name="bank_code" value="01020000">
</form>
```



