# Instant Payment Apply API

This API is used to transfer money from you merchant account to the bank account you designated in almost real time. 
***

## Request

###### Endpoint

```html
https://instantpay.lianlianpay.com/paymentapi/payment.htm
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|api_version|Required|String(6)|1.1|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|flag_card|Required|String(1)|Flag of the card type.<br>1.Personal debit card<br>2.Business bank settlement accounts|
|money_order|Required|String(12)|Merchant order amount in CNY, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected.|
|no_order|Required|String(32)|Original merchant order No. |
|dt_order|Required|String(14)|Original merchant order date. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|card_no|Required|String|User's card number|
|acct_name|Required|String|The name of the card holder, in Chinese.|	
|info_order|Required|String(255)|```info_order``` will be sent back in asynchronous notification for parameters transmission.|
|memo|Required|String(24)|Note which maybe shown in the bank statement. Special characters will be filtered. The regular expression for single character filtering is ```[^a-zA-Z0-9\\u4e00-\\u9fa5-_]```|
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|
|bank_code|Required for business bank settlement accounts.|String(18)|The bank short code of used card. |





###### Sample Request

The request message before being encrypted.

```html
   {
        "api_version":"1.1",
		"oid_partner":"201103171000000000",
        "card_no":"621626*********0018",
        "dt_order":"20170317165929",
        "flag_card":"0",
        "info_order":"test测试10.00?",
		"memo":"代付",
		"money_order":"0.02",
        "no_order":"2013051500001",
        "acct_name":"张三",
        "notify_url":"http://10.20.41.35:8080/tradepayapi/receiveNotify.htm",
        "sign_type ":"RSA",
        "sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
    }
	```
Encrypted request message. Please use LLP public key and [demo](#demo) to do the encryption.
```curl	
	curl https://instantpay.lianlianpay.com/paymentapi/payment.htm\
-H "Content-type:application/json;charset=utf-8" \
-d '{
       "oid_partner":"201408071000001541",
	   "pay_load":"lianpay1_0_1$QexRqaELwVLHBxL1o3TZQ2/g7Q+uKfJvhGVSnLnZEGwAg9Dd7ZODPPUkj+lIFlaQuROdnE1xPJVQ\r\n3EhxFS4l/QdBgG3cTbD+es/VxjJdHLSfp0+m7EIpZAaCIZkVTKkpAPZBN2ldl8kQTucEuBmQOEpm\r\nGXb2FmC2+jB+ugIiYgU=$bH02K28N5yjpHpwp38LdD52ZZV5PH4WTz71BcGI6M8JW6d3CWLeCuyMcrtF2rwSxUG/v6fbieat3\r\nanrg3Ip1RANcfNJfLIldtcwlvzaquUaHiHmwSuYPkrX6CYCGeqaYqwyi8kfE6onqw8HCSfRpLFjn\r\n7eopwZ9Clfi2eioCmas=$NUhENU5CQ1M=$SqN7bDTdUupZU+NnuxGEtv17+LLrbEukE6BCZ+rNPkloelbkvUs1mS9TTt73lIGmFaA2lhoGKUJy\r\nxLSI8o5XM4f+6L8PravXDMS907bx554fGWfaaF72wlizUfbrTN+yB0CMAbD7Ux4iNG8fKkMWKdkC\r\nT9EY0IJFrUCWtqUh7TkWeRtG4pSwBz6l/DHwG+hhuxOLQIj7BMK8yddnD6mEv/sRo5RzZEbcpZtU\r\nE6WGUHEY0y9sa/za1B/3JI2mZ8dcd0IDN5ir78UrDkP1mY7syViYESRGML8aUpoVjFeyV6PMg6T8\r\nd8ZeZWtaM0F3y7djirY2AghYy0ZzPis6cqi8fl6oCXBN9I36wo+7xIMMtARw8f0k74nHKKs23Fpy\r\nwALaJ1XIbBbdgB7QTfGnKL6bVj2UXb92yUjNTOH6BD5D2lals5Z9bS2n+mEKgkJqEnI80szyPuQg\r\n3NfcxZ+Xz6Cw0NrEgslwkHVslODgXEbTqY+gpjFFaGhXaXGDB1UyfngDElZwnj9WaWsQjJaa8ymN\r\n0Ry2nhEC7SxjJZOa4Dibs2g1K7/xbPsvcjpE1A5uTZnr2/2cSfgIJ17uEL2N5Is4DJl4mlWV7FUO\r\nTNBExS/ALvFLX8isLjVrlAPHlNyRBu+rPcyRFeCFgcAtqVpH5XKNphw2PLbZaiZsuoWaaYHu89gl\r\nU7kbO7OutLMsBPn7PbBl$P+c/1ObkTmCXWcGAXog3/Ax9u6+nO9Jyqwpc0AM/DOc="
	   }'
```



***

## Response

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. Refer to [Return Codes](return-codes.md)|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|no_order|Optional|String(32)|Original merchant order No. |
|confirm_code|Optional|String(6)|Confirm code for [instant payment confirm API](instant-payment-confirm-API.md). Returned when ```ret_code``` is ```4002```,```4003``` or ```4004```. |

The following parameters are returned only when ```ret_code=0000```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. Returned when the transaction object is created successfully. |

###### Sample Response


* Returned when the transaction object is created successfully.

```json
{   
    "no_order": "2016081814228",   
    "oid_partner": "201606020000150062",   
    "oid_paybill": "2016081847411984",   
    "ret_code": "0000",   
    "ret_msg": "交易成功",   
    "sign":"gMMBfHBkD6gFDfV0DgA9yGYubY1nejBnVpEh0UTAgYPIYVb5F7TPdoh8R3WnYPFn3RFG4M4oRJT26wdbXtJj84z5N1mzqIOxBzQ6IZ5gXz2tFq1dWy9ro0GDG9DmxPpTj5BidOzE4 85/caI81EEtQnvrva4z14NHFL3vEC2QJoo=",   
    "sign_type": "RSA" 
    } 
```

* Failure example.

```json
{
   "oid_partner":"201408071000001541",
   "ret_code":"4005",
   "ret_msg":"商户未开通权限",
   "sign":"M/EH4khKDPUIMnDqEid7mh90Acj0pguMVLiq17+ubl9M8YtOqG79X0q+b6TzuyiHVDOlXlUM8dvJQjgJ2qJFcGXe+1DakX0T71DLOQZjE3BEPcCWGQN0PcBpAs3EE1LBaI5p8FmBd3bLGMybYpTrt7/DC+BrukvJLgW7pMjHtHw=",
   "sign_type":"RSA"
}
```

* When confirmation is needed.

```json
{  
   "no_order":"20190510112458100750606813", 
   "confirm_code":"646872", 
   "ret_code":"4002", 
   "ret_msg":"疑似重复提交订单",  
   "sign_type":"RSA",  
   "sign":"fCtHOFJKJjv7hTyttL9mOugxmOo4jb+wvQODi1ldm/gVVRgbBCK+jKAgsH3COyL20w0vzYLq+fJA8toywInQ9VlHvc4VBYn6G1jbPnnBJXagfl6YpxdhFCJ6/o5vSr816vVMsz0/jwVjGhr2cDtTjwUvU9U6N8cfC5XPb/qiM2A=", 
   "oid_partner":"201801030001351894"}

```
## demo


[Java SDK](https://github.com/LianLianPay/LLP-InstantPay-Java-old)

[PHP SDK](https://github.com/LianLianPay/LLP-InstantPay-PHP)

[.NET SDK](https://github.com/LianLianPay/LLP-InstantPay-dotnet)
