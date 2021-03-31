# Card Bind Apply API

This API sends request to LianLian to initialize card bind request and send out SMS verification code, it needs to be used in conjunction with [Card Bind Verify API](MPAY-card-bind-verify.md) to complete card bind action. 

***

## Request

###### Endpoint

```text
https://mpayapi.lianlianpay.com/v1/bankcardbind
```

###### Parameter

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|user_id|Required|String(32)| The unique identification assigned to the user in the merchant’s system. |
|oid_partner|Required|String(18)| The unique identification assigned to the merchant. E.g. 201304121000001004. |
|sign_type|Required|String(3)|RSA or SHA256withRSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|pay_type|Required|String| 2, Quick payment - Debit card <br> 3, Quick payment - Credit card <br> P, New Authenticate Payment |
|api_version|Required|String(6)|Fixed value, ```2.1```|
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment_risk_item.md)| 
|id_type|Required|String(1)| 0, ID card <br> 2, Passport <br> 3, Military Officer Certificate <br> 4, Hong Kong-Macau laissez-passer <br> 6, Mainland travel permit for Taiwan residents <br> 9, Police Officer card <br> X, other certificates |
|id_no|Required|String| The number of User's ID card. The length need to be either 15 or 18.|
|acct_name|Required|String| The name of payer, in Chinese.|
|card_no|Required|String| User's card number|
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment-risk-item.md)|
|bind_mob|Required|String| User's phone number, currently only support China domestic number.|
|notify_url|Optional|String| Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify. |
|no_order|Optional|String| Card binding application serial number.|
|dt_order|Optional|String| The date when the card binding application is initialized. Format: yyyyMMddHHmmss, E.g. 20170801225714.|

###### Sample Request

The request message before being encrypted.
```json
{
        "acct_name": "李天增", 
		"bind_mob": "18667341234", 
		"card_no": "8888888888888888", 
		"dt_order": "20180828153543", 
		"id_no": "330401198001014616", 
		"id_type": "0", 
		"no_order": "20180828153543", 
		"notify_url": "http://test.yintong.com.cn/help/notify.php", 
		"oid_partner": "201304121000001004", 
		"pay_type": "P", 		 
		"risk_item": "{\"user_info_bind_phone\":\"13958069593\",\"user_info_dt_register\":\"20131030122130\",\"frms_ware_category \":\"1009\",\"request_imei”:211,\"request_imsi”:121121,\"request_ip”:192.168.20.110}", 
		"sign": "tFQhBzzpL9oaBaQAVMPyJ5w4n9hVP9B6cXrLBfq9EsMBPaQS67hcRHAsyUPt89YPzVzE/ILVMx8MJ1s6WSLFz3Oef+aW5cSPBB+edh+PN3L8C3LdMZA9DfU67h1Fa6iq7pVpfoFK4P8Y0rxwBLeKoYoQdpbztW/8oSuFN3k9WHo=",
    	"sign_type": "RSA", 
		"user_id": "20180828153543", 
		"vali_date": "2002"
		}
```
Send encrypted request message. 
```curl
curl https://mpayapi.lianlianpay.com/v1/bankcardbind \
-H "Content-type:application/json;charset=utf-8" \
-d '{ 
        "oid_partner": "201304121000001004", 
        "pay_load": "lianpay1_0_1$IjRo9QTNupI7FEKMQq0Cz+11KlnvAyoyrTQi8C03nqLlBGl2dj3Kbd9SoV+nTuNs8iTsvwNVnNt8\r\nLVv3iCZjBJYeo3oLsLb2JxWHpHltUgWBc/YQsVkmRmRcrIDzycxvZ8OIQxwVVk34ad7anOC0RD7C\r\nap6BlcZPTs1RDe6fEaw=$KytJ6nSrXn7OS8EDQLENbgj2lVuiLD/VWD6hp0Cp5vieHK29tRHOCkW6ph+fq3yHU98knbaQiAtU\r\nipM/tPGWpyuCyWOBP6aayoJDU0RKdJMgqK0MQYQbZPxg7XCQT7Z6YatTiT4ZFCAQd3LfdSm9OX+w\r\nFb0jCS7QsJYiznF86Jk=$QkFOWFlUMUE=$SCjQXBPxHCk6/mQMD1/RnZUfwKukGcgtm+LmJyK6/627+TPCeZ1iG1maCWluSq7C1FsV5DEhknTH\r\n9I5y6U3xyOajSRq5HS42znCo4oDTT8JrLxgBAAy97CdE8Aax9z7iEmDqwhDRQOwUTb7lObmqoD+K\r\nOEC/fAzZGP6dz4iqkbepLB9atXkFYi7+bAX/ZNVgUeBkL7mXHSpQTl1Yrh21qkKBT2OTVu8weUO2\r\n4xjvV+utC4OgSqdFeWahi5ODS5IWTAuSb+kHcvbvX4UZjDQ8w2s7EwgnlpOzOggFeyUPawMjjx45\r\nPzT+TbS6/glIjtLISnUuOCrwu0i640vlLIvBCyPbbni+5vNF9kD7uhJEDEXkO9tDo3vF8HHzB4lQ\r\n8gXN5sehoGXvd2Rk9ZhUCVk70YCkXd4lA0Jf9imQLxc/tshoyTI4Cb9pcMLvfbt4vZd5tbARJqLF\r\n92MagCxKbLKPl7v+esiCIgS6/TqKcVzCdcaQKRUonjowrOqn+d+7xumHURtX9yYOAawq066gUCOb\r\no6PHrT8KxrAoQ2STxEod4c6HjGXseMs9HqzLadJlzgKFCTiXk8O8FrlcaaMehiiZO8+XILCihcI6\r\nzhTgrE77hstkb5CFfBUwHkdNH24WveERZlluoHUncWwuYof1d28HMjb9dQyFrJR0LIIQYE0V5eOP\r\nhoJXpnPEx8otm+0iqo0cy9Vx257jqb7ezvYMCxkQlbtFuy7Wpko9JsfDcYnwFKElgm1pSnGD9Ga/\r\nSFPT62tA2ndFRfSy32PS/TcPnZYEvM4TwBVTD2srnNG4NxLmS7fnqU0y06q4XSb0lFQh0XSfFZR4\r\ni6bFsHw6ZWcrlBIgOM1/Lmj5SoKBM9K8R9IcYB/eXZSVhPRReCk/9g7H0Id7EEFqPmRfuA/XqsH4\r\nowg1KkxN3kQ/XzF1JSeLoGjIyUESLn+C2Sv4+at+7aCX$XmrO6P26GhZqmR9gE75isXSkmq1dgvW40MAIlwVm8e0=" 
		}'
```

***

## Response

###### Parameter

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. Refer to [return codes](return-codes.md)<br>8888, need to verify card binding request.|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |

The following parameters are returned only when ```ret_code=8888```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|sign_type|Required|String(3)|RSA or SHA256withRSA|
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|token|Optional|String| Used for [MPAY Card Bind Verify API](MPAY-card-bind-verify.md)|
|dt_order|Optional|String| The date when the card binding application is initialized. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|no_order|Optional|String| Card binding application serial number.|

###### Sample Response

```json
{
	"ret_code":"8888", 
	"ret_msg":"短信已下发，需要再次验证", 
	"token":"D096DBA0E3E0CC8F1D504C06E71D292D", 
	"oid_partner":"201103171000000000", 
	"user_id ":"20130515094013", 
	"sign_type":"RSA", 
	"sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
	}
```
