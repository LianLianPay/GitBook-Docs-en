# Payment creation direct API

This API supports payment creation for multipe pay type.

***

## Request Parameters and sample


###### Endpoint

```html
https://payserverapi.lianlianpay.com/v1/paycreatebill
```

###### Request Parameters
|Name|Required|Type|Description|
|:---|:---|:---|:---|
|api_version|Required|String(6)|Fixed value, 1.0|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|timestamp|Required|String(14)|The time when request is initialized. Format: yyyyMMddHHmmss, E.g. 20170801225714. The time difference between your server and LianLian server(UTC +8) should be no more than 30 mins|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|busi_partner|Required|String(6)|Fixed value. Virtual products, 101001; Physical products, 109001|
|no_order|Required|String(32)|Merchant order No.|
|dt_order|Required|String(14)|Merchant order date. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|name_goods|Optional|String(40)|Product name. E.g. Pen|
|info_order|Optional|String(255)|```info_order``` will be sent back in asynchronous notification for parameters transmission|
|money_order|Required|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|
|url_return|Optional|String(128)|Online url, your customer will be redirected to ```url_return``` once they finished their payment|
|userreq_ip|Optional|String(32)|The IP address of your customer, used for anti-fraud purpose. Replace "." with "_", E.g. 122_11_37_211|
|back_url|Optional|String(128)|The URL for user to go back and modify card NO.|
|shareing_data|Optional|String(128)|Refer to [Sharing data introduction](#sharing-data-introduction)|
|risk_item|Required|String| This parameter is used for payment risk control, all required parameters should be included in the value of ```risk_item``` in json format, refer to [Payment Risk](payment_risk_item.md)| 
|flag_pay_product|Required|String| The payment method used in this transaction.<br> 0, bank card payment(low transaction costs limit) <br> 1, bank card payment(high transaction costs limit) <br> 2, online banking payment <br> 12, mobile online banking <br> |
|flag_chnl|Required|Int|client type <br>0：SDK-android <br>1：SDK-ios<br>2：WEB <br>3：WAP|
|id_type|Optional|String(1)| 0, ID card|
|id_no|Optional|String| The number of User's ID card. The length need to be either 15 or 18|
|acct_name|Optional|String|The name of payer, in Chinese|
|card_no|Optional|String|User's card number|
|valid_order|Optional|Int|The valid period of ```no_order```, in minute. The status of corresponding transaction will be set to "Closed" once its ```valid_order``` run out. Default: 10080 (7 days). |
|bind_mob|Optional|String(18)|Mobile phone number binding on bank card|
|no_agree|Optional|String(32)| A token which represents the key payment information, refer to [Binding Card](easypay.md) for more details|
|bank_code|Optional|String|Specify which online banking should be used instead of displaying online banking selection page. ```bank_code``` to be used with ```pay_type```. |
|card_type|Optional|String|The payment method used in this transaction. <br> 1, online banking payment (debit card) <br> 8, online banking payment (credit card) <br> 9, business online banking payment <br>|



###### Sample Request

```json
{
  "api_version": "1.0",
  "sign_type": "RSA",
  "sign": "9K9+wUL3NuRdFy60rOXDWqcmtTD7k8PMII8HUb9czISzXp8ff1sPH1jCVxM=",
  "time_stamp": "20170824102755",
  "platform": "201707041000000000",
  "oid_partner": "201707041000000000",
  "user_id": "userid000001",
  "busi_partner": "101001",
  "no_order": "4d80a9b351f8b167a661c84dc013e927",
  "dt_order": "20170824102755",
  "name_goods": "话费充值",
  "money_order": "598.35",
  "notify_url": "https://www.*****.com/notify.do",
  "risk_item": "{\"user_info_bind_phone\":\"13958069593\",\"user_info_dt_register\":\"20131030122130\",\"risk_state\":\"1\",\"frms_ware_category \":\"1009\"}",
  "flag_pay_product": "1",
  "flag_chnl": "2",
  "bank_code ": "0100000",
}

```

###### Response Parameter

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|0000, Refer to [return codes](return_code.md)|
|ret_msg|Required|String(100)|交易成功|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|dt_order|Optional|String(14)|Merchant order date. Format: yyyyMMddHHmmss, E.g. 20170801225714|
|no_order|Optional|String(32)|Merchant order No.||user_id|Required|String(32)|The unique identification assigned to the user in the merchant’s system|
|money_order|Optional|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|token|Optional|String| It'll lose effectiveness in 30 minutes. Won't return if **No SMS Verification Flow** is setup.  Used for [Easypay Verify Direct API](easypay-verify-direct-api.md)|
|sign_type|Optional|String(3)|RSA |
|sign|Optional|String|Signature value|
|gateway_url|Optional|String|Payment page|

###### Sample Response

```json
{
  "ret_code": "0000",
  "ret_msg": "交易成功",
  "sign_type": "RSA",
  "sign": "9K9+wUL3NuRdFy60rOXDWqcmtTD7k8PMII8HUb9czISzXp8ff1sPH1jCVxM=",
  "no_order": "4d80a9b351f8b167a661c84dc013e927",
  "dt_order": "20170824102755",
  "money_order": "598.35",
  "oid_paybill": "2017122889360863",
  "gateway_url": "https://paymentweb.lianlianpay.com/paymentweb/payinit?token=4a1123e3f123138df813ab81de8",
  "token": "4a1123e3f123138df813ab81de8"
}
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
