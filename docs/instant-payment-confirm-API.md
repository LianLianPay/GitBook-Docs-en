# Instant payment confirm API

This API is used to cinfirm the money transfer application.

## Request

###### Endpoint

```html
https://instantpay.lianlianpay.com/paymentapi/confirmPayment.htm
```

###### Request Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|api_version|Required|String(6)|1.1|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|
|confirm_code|Required|String(6)|Confirm code.  |
|no_order|Required|String(32)|Original merchant order No. |
|notify_url|Required|String(128)|Online url where asynchronous notification should be sent, E.g. http://www.lianlianpay.com/help/notify|



######  sample request

The request message before being encrypted.
```json
{
    "api_version":"1.0",
    "confirm_code":"137007",
    "no_order":"20170208165924",
    "notify_url":"http://192.168.110.13:8080/test/tradepayapi/receiveNotify.htm",
    "oid_partner":"201606221000921503",
    "sign":"CLVVqaS39aW9EurgJbljVR3gmti5emt4GJdi0f0edJjvr8k52HBB60cEu7UmiMxtqgm/gVA40OG42ANY96ZiEoiJnAPdBANTK3pJyYq/anw56wrGLLW/HIFiTslnvIpwqtQoSjBaz8h+J2aSXzFPywG6WXFsLyAb/NUJSCSb6dU=",
    "sign_type":"RSA"
}
```
Encrypted request message. Please use LLP public key and [demo](#demo) to do the encryption.
```curl
curl https://instantpay.lianlianpay.com/paymentapi/confirmPayment.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{
        "oid_partner":"201606221000921503",
        "pay_load":"lianpay1_0_1$WJlWl6plHu1NJppMZJl0UhBo8xzwBA/0SlbrHwNbnejDLfr8SMqzv+tXRwX+9eGMX6nzsyO8kDR2\r\nReZ/w22hJhQauaJiKrDcaoAdw1++SX1H25rg+sX1D0oh+eYXzLBXCnTXAG61xHBAIDbOJK3bmjrp\r\nM2/VjEkY8+/zHnHO3t0=$icV1y+ZLWywLksIWy725MTrfAPasC7BNpdqyHzGUTWWYY2/YaeP9qVDOJ7xR4wPbWa27WjRCMANa\r\nH+7R2UcM7pD4z3Ns3LWAx7SGVMTC53ojJkEHWlES3vZH9iitCNuczsSx81gf+FazQSO9d3RJr5Ez\r\nK+MJrbDCMLw8b2VpGGs=$RUIyMkM0NEs=$tfXbWbTWnzseVnnu/9Rl0Pz0EzAHUs7teGYC25c8ZZ05V7EkCXVOwmhGSQRzDmlwVNl/drQxseVa\r\nHhyi1XKbfK8b94Gaq/bbQvsoKZ5XEczqNNVljJD+gwjTFaDn23Ac5YoPyXnKucAWpm8kFWOGe2/O\r\nWPtypAl5RuDmVM6q3Hn60QppwJMB5ajpUo1+udG7UofRdz9BGp/zso5AMUkgtEWwL8ilHTH7lxZa\r\n87eRYXCo/1cVoavsj8aVKPjbH7xcl7+sklx9nQSVZWxIoor78JlNQpjoGPaugEbUo3lW69/OixoI\r\npddgMjdzdmbxJWNryW7Y6RJPmbyEemvfRb3dgq9deecgO12wkH/lgbueRWMqpGN4hkSoJS9bYPhP\r\nGL01zbYxrQxWgaJR/y7Ue0TtNSX2sF+UfuYY3guutvUlqM4PWAFzSrIt/B64pX2HNWsEwRA07WuO\r\nmFZPuzb6YG4cWOqUmzwK/hcb8+OcA1L5zJTEA/WRmKDs1fM9heP/c4tiHQ==$7btIQ57N4wnhOoCEDPwZdp+sfHRx/LSFdrmiJVFbd+4="
}'
```
## Response

###### Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|Return code, whether the request is handled successfully or not. Refer to [Return Codes](return-codes.md)|
|ret_msg|Required|String(100)|Return message, description of ```ret_code```, in Chinese |
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value, refer to [signature document](signature.md)|

The following parameters are returned only when ```ret_code=0000```:

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098. Returned when the transaction object is created successfully. |
|no_order|Optional|String(32)|Original merchant order No. |

###### Sample Response


Returned when the transaction object is created successfully.

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

