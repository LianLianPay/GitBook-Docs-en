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
|frms_is_real_name|Optional|String| 0, Non-real-name <br> 1, Real name |
|user_info_mercht_userno|Required|String| Merchant user No. Value can be same with ```user_id``` |
|user_info_mercht_userlogin|Optional|String| User name in merchant system (phone no., email, etc.) |
|user_info_mail|Optional|String||
|user_info_bind_phone|Required|String|Merchant user phone number|
|user_info_mercht_usertype|Optional|String|1, normal user <br> 2, white listed user (low risk score) <br> By default is 1|
|user_info_dt_register|Required|String(14)|Format: YYYYMMDDHHMMSS|
|user_info_register_ip|Optional|User IP address |
|goods_name|Required|String| Product name. Can be same with ```name_goods```|

***

## API Parameters

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
|normal_delivery_address|Optional|String||
|delivery_full_name|Required|String|Recipient name |
|delivery_phone|Required|String|Mobile phone number of recipient|
|logistics_mode|Required|String|1, Post <br> 2, Ordinary Express<br> 3, Express delivery <br> 4, Logistics freight company <br> 5, Logistics and distribution company <br> 6, International express <br> 7, Shipping express <br> 8, Ocean Shipping <br>  9, Package Deliver & Pick up |
|delivery_cycle|Required|String|12h, Within 12 hours <br> 24h, Within 24 hours <br> 48h, Within 48 hours <br> 72h, Within 72 hours <br> other, 3 days later |

