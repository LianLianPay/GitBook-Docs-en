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
|user_info_mail|Optional|String||
|user_info_bind_phone|Required|String|Merchant user phone number|
|user_info_dt_register|Required|String(14)|Format: YYYYMMDDHHMMSS|
|goods_name|Required|String| Product name. Can be same with ```name_goods```|

***

## API Parameters(Server to server)

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|frms_client_chnl|Required|String| Your client side type. 10, Mobile application <br> 13, Web application <br> 16, H5 application |
|frms_ip_addr|Required|String|End-user's IP address|

***

## Real-name Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|user_info_full_name|Required|String| Your user's name|
|user_info_id_type|Required|String|0, ID Card or Business License <br> 1, Household Register <br> 2, Passport <br> 3, Certificate of Military Office <br> 4, Certificate of Soldier <br> 5, Mainland Travel Permit for Hong Kong and Macau Residents <br> 6, Mainland Travel Permit for Taiwan Residents <br> 7, Temporary ID Card <br> 8, Residence Permit for Foreigners <br> 9, Police ID Card <br> X, Other Certificates <br> By default is 0, currently only 0 is supported |
|user_info_id_no|Required|String||
|user_info_identify_state|Required|String|1, ID-based <br> 0, Non-ID-based|
|user_info_identify_type|Required|String|1, Bank Card Verification <br> 2, On-spot Verification <br> 3, ID Card Remote Verification <br> 4, Other Types of Verification |

***

## Delivery Parameters

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|delivery_addr_full|Required|String|Address full name includes county (district)|
|delivery_addr_province|Required|String|Province code |
|delivery_addr_city|Required|String|City code|
|delivery_full_name|Required|String|Recipient name |
|delivery_phone|Required|String|Mobile phone number of recipient|
|logistics_mode|Required|String|1, Post <br> 2, Ordinary Express<br> 3, Express delivery <br> 4, Logistics freight company <br> 5, Logistics and distribution company <br> 6, International express <br> 7, Shipping express <br> 8, Ocean Shipping <br>  9, Package Deliver & Pick up |
|delivery_cycle|Required|String|12h, Within 12 hours <br> 24h, Within 24 hours <br> 48h, Within 48 hours <br> 72h, Within 72 hours <br> other, 3 days later |

***

## Virtual risk control parameters 

Virtual risk control parameters are divided into the following 6 categories:
* Games, no game card.Required when frms_ware_category is 1003.
* Personal telephone recharge.
* Anonymous ticketing, such as tickets, car tickets, etc. 
* Car rental with a little money. 
* Refueling cards
* Pay for public utilities

## Games

***

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|game_id_belongs|Required|String|The current type of transaction.<br>1 - recharge or pay for this account.<br>2 - recharge or pay for another person's account. |
|game_id|Required when game_id_belongs is 2|String|The repaid game account ID. |

***

## Personal telephone recharge

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|frms_charge_phone|Required|String|The recharged phone no. |

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

## Car rental with small amount money

|Name|Required|Type|Description|
|:---|:---|:---|:---|
|taxiguest_phone|Required|String|The passenger contact telephone number.  |
|taxi_no_province|Required|String|The provincial code for the ride location.Refer to [?省市编码表待写](?.md)| |
|taxi_no_city|Required|String|The city code for the ride location.Refer to [?省市编码表待写](?.md)|
|bytaxi_time|Required|String|Ride time. |
|taxi_mileage|Required|String|Ride length(km). |
|taxidriver_phone|Required|String|The driver contact phone. |

