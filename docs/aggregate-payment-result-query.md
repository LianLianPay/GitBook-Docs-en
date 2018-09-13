# Aggregate Payment Result Query API

You can call this API from server-side to obtain the information on a particular transaction.

***

###### Endpoint

```html
https://wallet.lianlianpay.com/llwalletapi/orderquery.htm
```

###### Request Parameters

  <table>
   <tr>
      <td>Name</td>
      <td>Required</td>
      <td>Type(length)</td>
      <td>Description and Example</td>
   </tr>
   <tr>
      <td colspan="4">Basic Parameters</td>
   </tr>
   <tr>
      <td>oid_partner</td>
      <td>Required</td>
      <td>String(18)</td>
      <td>A unique 18 digit number to identify a contracted merchant account. . E.g. 201304121000001004</td>
   </tr>
   <tr>
      <td>sign_type</td>
      <td>Required</td>
      <td>String(3)</td>
      <td>RSA</td>
   </tr>
   <tr>
      <td>sign</td>
      <td>Required</td>
      <td>String</td>
      <td>Signature value</td>
   </tr>
   <tr>
      <td colspan="4">Business Parameters</td>

   </tr>
   <tr>
      <td>no_order</td>
      <td>Optional</td>
      <td>String(32)</td>
      <td>Merchant order No.It's an either-or choice between no_order and oid_paybill.</td>
   </tr>
   <tr>
      <td>dt_order</td>
      <td>Optional</td>
      <td>String(14)</td>
      <td>Merchant order date. Format: YYYYMMddHHmmss, E.g. 20170801225714</td>
   </tr>
   <tr>
      <td>oid_paybill</td>
      <td>Optional</td>
      <td>String(14)</td>
      <td>The unique trade number in lianlian Pay system.</td>
   </tr>

   <tr>
      <td>query_version</td>
      <td>Optional</td>
      <td>String(6)</td>
      <td>The query API version.The default is 1.0</td>
   </tr>
   <tr>
      <td>user_id</td>
      <td>Optional</td>
      <td>String(32)</td>
      <td>The unique identification assigned to the user in the merchant’s system</td>
   </tr>

</table>

###### Sample Request

```html
curl https://wallet.lianlianpay.com/llwalletapi/orderquery.htm \
-H "Content-type:application/json;charset=utf-8" \
-d '{
        "oid_partner":"201103171000000000",
        "dt_order":"20130515094013", 
        "no_order":"2013051500001", 
        "sign_type ":"RSA",
        "sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk=" 
    }'
```

***

##  Response

###### Parameter

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|ret_code|Required|String(4)|0000, Refer to [return codes](return_code.md)|
|ret_msg|Required|String(100)|交易成功|
|oid_partner|Required|String(18)|The unique identification assigned to the merchant. E.g. 201304121000001004|
|sign_type|Required|String(3)|RSA |
|sign|Required|String|Signature value|
|no_order|Optional|String(32)|Merchant order No.|
|dt_order|Optional|String(14)|Merchant order date. Format: YYYYMMddHHmmss, E.g. 20170801225714|
|oid_paybill|Optional|String(18)|Unique transaction No. in LianLian system. E.g. 2011030900001098|
|money_order|Optional|String(12)|Merchant order amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in CNY|
|result_pay|Optional|String| Payment result. E.g. SUCCESS|
|settle_date|Optional|String(8)| Format YYYYMMdd. Returns when payment is successful|
|info_order|Optional|String(255)| Returns when ```info_order``` is sent in API requests|
|pay_type|Optional|String| The payment method used in this transaction. <br> 0, balance payment <br> 1, online banking payment (debit card) <br> 8, online banking payment (credit card) <br> 9, business online banking payment <br> 2, express payment (debit card) <br> 3, express payment (credit card)<br> D, verified payment <br> I, WeChat Payment <br> L, Alipay Payment| 
|bank_code|Optional|String| Short code of banks|
|bank_name|Optional|String| Name of banks|
|memo|Optional|String| Payment information from bank side |

###### Sample Response

```json
{
    "oid_partner":"201103171000000000",
    "dt_order":"20130515094013",
    "no_order":"2013051500001",
    "result_pay":"SUCCESS",
    "oid_paybill":"2013051500001",
    "money_order":"49.65",
    "settle_date":"20130515",
    "info_order":"用户13958069593购买了3桶羽毛球",
    "pay_type":"2",
    "bank_code":"01020000",
    "sign_type":"RSA",
    "sign":"ZPZULntRpJwFmGNIVKwjLEF2Tze7bqs60rxQ22CqT5J1UlvGo575QK9z/+p+7E9cOoRoWzqR6xHZ6WVv3dloyGKDR0btvrdqPgUAoeaX/YOWzTh00vwcQ+HBtXE+vPTfAqjCTxiiSJEOY7ATCF1q7iP3sfQxhS0nDUug1LP3OLk="
}
```