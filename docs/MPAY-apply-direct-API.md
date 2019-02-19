# MPAY Apply Direct API

This API is used to apply card Payment, create corresponding transaction object in LianLian's system and send SMS verification code.

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
|sign_type|Required|String(3)|RSA or SHA256withRSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|busi_partner|Required|String(6)|Fixed value. Virtual products, ```101001```; Physical products, ```109001```|
|no_order|Required|String(32)|Merchant transaction No.|
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|name_goods|Required|String(40)|Product name. E.g. Pen|
|info_order|Optional|String(255)|```info_order``` will be sent back in synchronous or asynchronous notification for parameters transmission|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected|
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|
|valid_order|Optional|Int|The valid period of ```no_order```, in minute. The status of corresponding transaction will be set to "Closed" once its ```valid_order``` run out. Default: 10080 (7 days). |
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment-risk-item.md)| 
|pay_type|Required|String| 2, Quick payment - Debit card <br> 3, Quick payment - Credit card <br> P, New Authenticate Payment |
|vali_date|Optional|String| The expire date of credit card. Required for credit card|
|card_no|Required|String|User's card number|
|cvv2|Optional|String|The CVV/CVC2 of credit card. Required for credit card|
|bank_code|Optional|String(18)|The bank short code of used card.|
|acct_name|Required|String|The name of payer, in Chinese|
|bind_mob|Required|String| User's phone number, currently only support China domestic number.|
|id_type|Required|String(1)|0, ID card <br> 2, Passport <br> 3, Military Officer Certificate <br> 4, Hong Kong-Macau laissez-passer <br> 6, Mainland travel permit for Taiwan residents <br> 9, Police Officer card <br> X, other certificates |
|id_no|Required|String| The number of User's ID card. The length need to be either 15 or 18|
|no_agree|Optional|String(32)| A token which represents the key payment information|


> ```id_no```, ```bind_mob,```, ```id_type```, ```card_no``` are required if ```no_agree``` is NOT present.

###### Sample Request

The request message before being encrypted.
```json
{
    	"acct_name": "李天增", 
		"bind_mob": "18667341234", 
		"busi_partner": "101001", 
		"card_no": "8888888888888888", 
		"cvv2": "222", 
		"dt_order": "20180828153543", 
		"id_no": "330401198001014616", 
		"money_order": "55.44", 
		"name_goods": "点读机", 
		"no_order": "20180828153543", 
		"notify_url": "http://test.yintong.com.cn/help/notify.php", 
		"oid_partner": "201304121000001004", 
		"risk_item": "{\"user_info_bind_phone\":\"13958069593\",\"user_info_dt_register\":\"20131030122130\",\"frms_ware_category \":\"1009\",\"request_imei”:211,\"request_imsi”:121121,\"request_ip”:192.168.20.110}", 
		"sign": "HTFW/Tzk4xzGOZGCFD9LwisGg6Wsnn/THQbcLPTz3rqQmdsVXmVcnoyNrPTUh46GD5uqqWbd8KG8Gf26UI0g4S3Oqwb6vDkJb95+jRqNVWeyiDSaxsOqCry1xUs6PR2Qpqm4WPVYaiHOawH3NC3Gav9KcZ25QwIAaq0OMi8K5BQ=", "sign_type": "RSA", "user_id": "20180828153543", "vali_date": "2002", "valid_order": "10080"
		K8sRWnwfZD605I5JqqormozxCjQBf0tmH+yDo0lG5ovgYBSO7Fwv21vBoyr+Aq7dwLVmGbEzjJgcqXYYLeXrexNKpNepXCAE+CDBHgbOLujBPpNIiEf+sr1ABXN69HjDvKaz0SS8qt6aMNHda2HhCZapU="
    }
```
Send encrypted request message. 
```curl
curl https://mpayapi.lianlianpay.com/v1/bankcardprepay\
-H "Content-type:application/json;charset=utf-8" \
-d '{
        "oid_partner": "201304121000001004", 
		"pay_load": "lianpay1_0_1$IjRo9QTNupI7FEKMQq0Cz+11KlnvAyoyrTQi8C03nqLlBGl2dj3Kbd9SoV+nTuNs8iTsvwNVnNt8\r\nLVv3iCZjBJYeo3oLsLb2JxWHpHltUgWBc/YQsVkmRmRcrIDzycxvZ8OIQxwVVk34ad7anOC0RD7C\r\nap6BlcZPTs1RDe6fEaw=$KytJ6nSrXn7OS8EDQLENbgj2lVuiLD/VWD6hp0Cp5vieHK29tRHOCkW6ph+fq3yHU98knbaQiAtU\r\nipM/tPGWpyuCyWOBP6aayoJDU0RKdJMgqK0MQYQbZPxg7XCQT7Z6YatTiT4ZFCAQd3LfdSm9OX+w\r\nFb0jCS7QsJYiznF86Jk=$QkFOWFlUMUE=$SCjQXBPxHCk6/mQMD1/RnZUfwKukGcgtm+LmJyK6/627+TPCeZ1iG1maCWluSq7C1FsV5DEhknTH\r\n9I5y6U3xyOajSRq5HS42znCo4oDTT8JrLxgBAAy97CdE8Aax9z7iEmDqwhDRQOwUTb7lObmqoD+K\r\nOEC/fAzZGP6dz4iqkbepLB9atXkFYi7+bAX/ZNVgUeBkL7mXHSpQTl1Yrh21qkKBT2OTVu8weUO2\r\n4xjvV+utC4OgSqdFeWahi5ODS5IWTAuSb+kHcvbvX4UZjDQ8w2s7EwgnlpOzOggFeyUPawMjjx45\r\nPzT+TbS6/glIjtLISnUuOCrwu0i640vlLIvBCyPbbni+5vNF9kD7uhJEDEXkO9tDo3vF8HHzB4lQ\r\n8gXN5sehoGXvd2Rk9ZhUCVk70YCkXd4lA0Jf9imQLxc/tshoyTI4Cb9pcMLvfbt4vZd5tbARJqLF\r\n92MagCxKbLKPl7v+esiCIgS6/TqKcVzCdcaQKRUonjowrOqn+d+7xumHURtX9yYOAawq066gUCOb\r\no6PHrT8KxrAoQ2STxEod4c6HjGXseMs9HqzLadJlzgKFCTiXk8O8FrlcaaMehiiZO8+XILCihcI6\r\nzhTgrE77hstkb5CFfBUwHkdNH24WveERZlluoHUncWwuYof1d28HMjb9dQyFrJR0LIIQYE0V5eOP\r\nhoJXpnPEx8otm+0iqo0cy9Vx257jqb7ezvYMCxkQlbtFuy7Wpko9JsfDcYnwFKElgm1pSnGD9Ga/\r\nSFPT62tA2ndFRfSy32PS/TcPnZYEvM4TwBVTD2srnNG4NxLmS7fnqU0y06q4XSb0lFQh0XSfFZR4\r\ni6bFsHw6ZWcrlBIgOM1/Lmj5SoKBM9K8R9IcYB/eXZSVhPRReCk/9g7H0Id7EEFqPmRfuA/XqsH4\r\nowg1KkxN3kQ/XzF1JSeLoGjIyUESLn+C2Sv4+at+7aCX$XmrO6P26GhZqmR9gE75isXSkmq1dgvW40MAIlwVm8e0="    
		}'
```

***

## Response

The response of this API is possible to include the payment result directly when your account has **No SMS Verification Flow** setup, for more details, please contact our technical support.

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. Refer to [return codes](return-codes.md)<br>8888, need to verify the payment request<br>0000, request successfully |
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |

The following parameters are returned only when ```ret_code=8888```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|Fixed value, RSA or SHA256WITHRSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|no_order|Required|String(32)|Merchant transaction No.|
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. Returned when the transaction object is created successfully. |
|money_order|Required|String(12)|Merchant transaction amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in the currency of ```currency_order```|
|token|Optional|String| It'll lose effectiveness in 30 minutes. Won't return if **No SMS Verification Flow** is setup.  Used for [MPAY Verify Direct API](MPAY-verify-direct-api.md)|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|

The following parameters are returned only when ```ret_code=0000```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|Fixed value, RSA or SHA256WITHRSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|no_order|Required|String(32)|Merchant transaction No.|
|dt_order|Required|String(14)|The date when the transaction is initialized. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. Returned when the transaction object is created successfully. |
|money_order|Required|String(12)|Merchant transaction amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in the currency of ```currency_order```|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|

The following parameters are returned only when ```result_pay=SUCCESS```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|settle_date|Optional|String(8)| Format YYYYMMdd. |
|no_agree|Optional|String(16)| A token which represents the key payment information|


###### Sample Response

Below is a sample response without **No SMS Verification Flow** when ```ret_code=0000```.

```json
{
	"ret_code":"0000", 
	"ret_msg":交易成功", 
	"token":"D096DBA0E3E0CC8F1D504C06E71D292D", 
	"oid_partner":"201103171000000000", 
	"dt_order":"20130515094013", 
	"no_order":"2013051500001", 
	"oid_paybill":"2013051613121201", 
	"money_order":"210.97", 
	"sign_type":"RSA", 
	"sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
	}
```
