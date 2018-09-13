# Transaction Report

Transaction reports are generated automatically by ```settle_date``` which is returned in [asynchronous notification](async-notification-concept.md) for successful payments.

LianLian provides 2 ways for you to obtain reports:

* Download it from [merchant dashboard](https://b.lianlianpay.com/trader/login.htm).

* Download it from Sftp servers. In order to obtain Sftp access, you need to contact [tech_support@yintong.com.cn](mailto:tech_support@yintong.com.cn).

***

## The Format and Examples

###### Daily report file format

Daily report is generated and uploaded to Sftp servers at 9:00 am (UTC +8) every day. 

**File name format**: Its name consists of prefix, your ```oid_parnter``` and ```settle_date``` of the payment. E.g. JYMX_201301221000001001_20130313.txt.

**File content format**: Only successful or refunded payment are shown in daily reports, failed payments won't be reflected in it. Besides, all transaction details including merchant's settlement and withdraw are recorded in daily reports.

Below is the header of each transaction report files:

```text
no_order | oid_partner | dt_order | busi_partner | oid_paybill | settle_date | currency_order | money_order | currenty_pay | money_pay | currency_settle | money_settle | exchange_rate | transaction_status | dt_update | transaction_fee | pay_product | pay_type | info_order  
```

|Parameter name|Description|
|---|---|
|no_order|Merchant transaction ID, the ```no_order``` in original request|
|oid_partner|Your account ID in LianLian's system, which is ```oid_partner```|
|dt_order|The corresponding time(UTC+8) of ```dt_order``` in original request, format: ```YYYYMMdd HH:MM:SS```. E.g. ```20171216 01:51:25```|
|busi_partner| The ```busi_partner``` in original request|
|oid_paybill|The unique transaction ID in LianLian's system, that is ```oid_paybill```|
|settle_date|The date of settlement|
|currency_order|The currency used in the transaction. Refer to [supported currencies](supported-currencies.md) |
|money_order|Merchant transaction amount, range: 0.01 ~ 100,000,000.00, 2 decimal places are expected, in the currency of ```currency_order```|
|currency_pay|The currency of which the payer used. Refer to [supported currencies](supported-currencies.md)|
|money_pay|The amount of which the payer payed|
|currency_settle|The settle currency of transaction|
|money_settle|The settle amount of transaction|
|exchange_rate|The exchange rate of current transaction|
|transaction_status|Possible value, 0 or 5. <br> 0 - Successful <br> 5 - Refunded|
|dt_update| The transaction status update time. |
|transaction_fee| The fee of each transaction, 2 decimal places, in CNY|
|pay_product|The used payment product of transaction|
|pay_type|The pay_type of current transaction|
|info_order| The ```info_order``` in original request|

Daily reports contain all of your business transaction records, you can use ```busi_partner``` as a filter if needed. 

An order which is refunded has 2 records in reports, one with positive ```money_order``` + ```fee``` and another one with negative ```money_order``` + ```fee```. The ```oid_paybill``` of the refund records is the original ```no_order```, and its ```payment_status``` is set to 5.

***

###### Monthly report file format

Monthly report will be available by 9:00 am (UTC +8) on the 1st of next month.

**File name format**: Its name consists of prefix, your ```oid_parnter``` and current month. E.g. JYMX_201301221000001001_201304.txt.

**File content format**: The content format is same with [daily report file format](#daily-report-file-format) but with longer time range.

***

###### Daily summary report file format

Summary report is generated everyday which records an overview of your daily revenue.

**File name format**: Its name consists of prefix, your ```oid_parnter``` and ```settle_date``` of the payment. E.g. JYMXSUM_201301221000001001_20130313.txt.

**File content format**:

```html
total_transaction_count | successful_transaction_count | successful_transaction_amount_summary(2 decimal places, in CNY) | refunded_transaction_count | refunded_transaction_amount_summary(2 decimal places, in CNY, negative value) | canceled_transaction_count | canceled_transaction_amount_summary(2 decimal places, in CNY, negative value)
```

E.g.

```html
8|8|551.23|0||0|
```