# Payment Risk

In order to identify and prevent fraud transactions, risk parameters, as known as ```risk_item```, is required to be provided when payment order is submitted. 

Risk parameters is related to below keys:
 
* The ```frms_ware_category``` of your ```oid_partner```, each of them has its own requirement of risk parameters. You may contact [tech_support@yintong.com.cn](mailto:tech_support@yintong.com.cn) for more details.
    
> Generally, ```frms_ware_code``` is a code with 4 digits which represents your industry category. Each ```oid_partner``` has its own ```frms_ware_category``` which is assigned by LianLian when it is created.

* Your implementation. If you have implemented LianLian payment products on your server side directly, [API parameters](#api-parameters) is required.

* The products your have integrated. For merchants who have integrated Verified Payment, [real-name parameters](#real-name-parameters) is required.

All risk parameters ought to be set in a json format and assigned to ```risk_item```, E.g.

```json
{
    "before_risk_item":"...",
    "risk_item":"{\"frms_ware_category\":\"1002\",\"user_info_mercht_userno\":\"...\",\"user_info_mercht_userlogin\":\"\",\"user_info_mail\":\"\",\"user_info_bind_phone\":\"...\",\"user_info_mercht_usertype\":\"\",\"user_info_dt_registe\":\"20180206143300\",\"user_info_register_ip\":\"\",\"user_info_full_name\":\"...\",\"user_info_id_type\":\"0\",\"user_info_id_no\":\"...\",\"user_info_identify_state\":\"0\",\"user_info_identify_type\":\"4\"}",
    "after_risk_item":"..."
}
```

***

## Basic Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|frms_ware_category|Required|String||
|user_info_mercht_userno|Required|String| Merchant user No. Value can be same with ```user_id``` |
|goods_count|Required|String|Quantity of goods|
|user_info_bind_phone|Required|String|Merchant user phone number. Required when merchants hold this info.|
|user_info_dt_register|Required|String(14)|User registration time at merchant side. Format: YYYYMMddHHmmss|
|goods_name|Required|String| Product name. Can be same with ```name_goods```|

***

## API Parameters(Server to server)

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|frms_client_chnl|Required|String| Your client side type. 10, Mobile application <br> 13, Web application <br> 16, H5 application |
|frms_ip_addr|Required|String|End-user's IP address|
|frms_imei|Required|String(40)|IMEI（International Mobile Equipment Identity）. Required for moblie application.|
|frms_mac_addr|Required|String(40)|MAC Address. Required for web application.|

***

## Real-name Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|user_info_full_name|Required|String| Your user's name|
|user_info_id_no|Required|String||
|user_info_identify_state|Required|String|1, ID-based <br> 0, Non-ID-based|
|user_info_identify_type|Required|String|1, Bank Card Verification <br> 2, On-spot Verification <br> 3, ID Card Remote Verification <br> 4, Other Types of Verification |

***

### Real-name businessman and traveller(air ticket, train ticket)
In case there are several goods for one order，please send following parameters in a list.  key=airline_list

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|airline_code|Required|String||
|passenger_country|Required|String|Country code of consumer  e.g.US |
|passenger_idcard_type|Required|String|0, ID Card or Business License <br> 1, Household Register <br> 2, Passport <br> 3, Certificate of Military Office <br> 4, Certificate of Soldier <br> 5, Mainland Travel Permit for Hong Kong and Macau Residents <br> 6, Mainland Travel Permit for Taiwan Residents <br> 7, Temporary ID Card <br> 8, Residence Permit for Foreigners <br> 9, Police ID Card <br> X, Other Certificates <br> By default is 0, currently only 0 is supported |
|passenger_idcard|Required|String|ID card NO|
|passenger_name|Required|String|First name of the passenger|
|passenger_phone|Required|String|Allow merchant to mask off the last four digits with the symbol *. |
|station_from_province|Required|String|Three-letter designator of the take-off airport.  |
|airport_to|Required|String|Three-letter designator of the landing airport.|
|station_from_city|Required|String|The city code of the city where train leaves. |
|station_from_province|Required|String|The province code of the city where train leaves. |
|station_to_province|Required|String|The province code of the city where train arrive to. |
|station_to_city|Required|String|The city code of the city where train arrive to. |
|dt_flight|Required|String|Flight date / travel date.Format: YYYYMMdd|

***

### hotel reservation

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|hotel_com_name|Required|String|Full name of the hotel|
|hotel_phone|Required|String|The telephone number of the person who checked in. |
|dt_arrive|Required|String(14)|Date of arrival.Format:YYYYMMDDH24MISS  |
|dt_leave|Required|String|Date of departure.Format:YYYYMMDDH24MISS|


***

### Tourist products
In case there are several goods for one order，please send following parameters in a list.  key=airline_list

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|book_phone|Required|String|Contact number when booking tickets|
|cinema_addr_province|Required|String|The province code where the consumer is located.|
|cinema_addr_city|Required|String(14)|The city code where the consumer is located.  |
|dt_play|Required|String|Activity start time.Format:YYYYMMDDH24MISS|

***

## Delivery Parameters for physical products

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|delivery_addr_full|Required|String|Address full name includes county (district)|
|delivery_addr_province|Required|String|Province code |
|delivery_addr_city|Required|String|City code|
|delivery_phone|Required|String|Mobile phone number of recipient|
|logistics_mode|Required|String|1, Post <br> 2, Ordinary Express<br> 3, Express delivery <br> 4, Logistics freight company <br> 5, Logistics and distribution company <br> 6, International express <br> 7, Shipping express <br> 8, Ocean Shipping <br>  9, Package Deliver & Pick up |
|delivery_cycle|Required|String|12h, Within 12 hours <br> 24h, Within 24 hours <br> 48h, Within 48 hours <br> 72h, Within 72 hours <br> other, 3 days later |

***

## Virtual products risk control parameters 


## Games

***

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|game_id_belongs|Required|String|The current type of transaction.<br>1 - recharge or pay for this account.<br>2 - recharge or pay for another person's account. |
|game_id|Required when game_id_belongs is 2|String|The repaid game account ID. |


***

## Anonymous ticketing

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|cinema_name|Required|String|The company name.The name of the cinema or the name of the ticketing company etc.  |
|book_phone|Required|String|The contact number. |
|cinema_addr_province|Required|String|The provincial code for the location of the consumer.|
|cinema_addr_city|Required|String|The city code for the location of the consumer.|
|dt_play|Required|String|Effective date. Refers to the time when the movie starts, the stage plays and so on. The format is YYYYMMDDHHMISS and use 24-hour clock.  |
|goods_count|Required|String|The Quantity of goods.|

***



