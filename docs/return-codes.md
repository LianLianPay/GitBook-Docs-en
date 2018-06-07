# Return Codes

> Currently all the ```ret_msg``` are written in Chinese.

|Ret_code|Ret_msg|Description|
|:---|:---|:---|
|0000|交易成功|Request is handled successfully|
|0106|签约银行卡信息查询失败|Failed to query the bind bank card info|
|1000|支付授权令牌失效或错误|Invalid token. Token expires in half an hour, in this case, you need to redo the request to obtain a new token|
|1001|商户请求签名错误|Invalid Signature|
|1002|第三方单号重复|Duplicated ```no_order```|
|1003|正在支付中,请稍后|Transaction is processing, contact our support if you encountered this issue|
|1004|商户请求参数校验错误（具体参数名）|The parameter returned in parenthesis is invalid |
|1005|支付处理失败|For a variety of reasons, the payment failed, including the failure of binding cards , insufficient money, the restrictions of the banks, etc |
|1006|支付单创建失败|Payment order creation failed|
|1007|网络链接繁忙|Internet connection failed|
|1008|商户请求IP错误|Invalid IP address|
|1009|暂停商户支付服务，请联系连连银通客服|The merchant payment service was suspended for a variety of reasons, contact our support if you encountered this issue|
|1012|请求报文解析失败|Failed to parse the request body|
|1014|日累计金额或笔数超限|Exceed the cumulative amount or number of transactions limits|
|1016|交易金额超限|The amount of the transaction is beyond the limit|
|1019|单笔金额超限|The amount of a single transaction is beyond the limit|
|1100|无效卡号|Invalid ```card_no```|
|1101|卡已挂失|A missing card is used|
|1102|无此发卡方|The card issuer does NOT exist|
|1103|银行卡有效期或CVV2错误|Invalid expired date or CVV2|
|1104|卡密码错误|Incorrect card password|
|1105|卡号在黑名单中|The card is in the blacklist or the card has been restricted|
|1106|允许的输入PIN次数超限|The time of inputting PIN exceed the limit |
|1107|您的银行卡暂不支持在线支付业务|Your bank card does not support online payment services|
|1108|您输入的证件号、姓名或手机号有误|Incorrect ```id_no```, ```acct_name``` or ```mobile_bind```|
|1109|卡号和证件号不符|Used ```card_no``` is inconsistent with the ```id_no```|
|1110|卡状态异常|Abnormal card status|
|1111|交易异常，支付失败|Payment failed due to abnormal issues|
|1112|证件号有误|Incorrect ```id_no```|
|1113|持卡人姓名有误|Incorrect ```acct_name```|
|1114|手机号有误|Incorrect ```mobile_bind```|
|1115|该卡未预留手机号|The user's card is not bound to any phone number|
|1200|用户选择的银行暂不支持，请重新选择其他银行进行支付/签约|Unsupported bank, please choose other banks to complete the payment.
|1201|银行卡变更失败|Card changing failed|
|1202|银行卡查询失败|Card querying failed|
|1805|暂不支持该银行|Unsupported bank|
|1900|短信码校验错误|SMS verification failed|
|1901|短信码已失效|SMS expired|
|1902|该卡交易金额超出免密限额|The trade amount exceed the amount limit of no payment verification|
|2003|支付单创建|Order created|
|2004|签约处理中|Card bind action is processing|
|2005|原交易已在进行处理，请勿重复提交|The original transaction is processing, please do NOT submit it again|
|2006|交易已过期|The transaction is closed|
|2007|交易已支付成功|The original transaction has been paid successfully|
|2008|交易处理中|The transaction is processing|
|3001|非法商户|Invalid ```oid_partner```|
|3002|用户生成错误|User account creation failed|
|3003|签约信息不存在|The used ```no_agree``` does NOT exit|
|3004|用户解约失败|Card unbinding failed|
|3005|查询跨度不得高于3个月|The period of querying is not allowed to exceed 3 months|
|3006|用户不存在	|This user account does not exist|
|3007|[user_id]查询不存在|This ```user_id``` does not exist|
|3008|查询截止日期错误|The end date of querying is incorrect|
|3009|重复签约|Duplicate bind card action|
|3010|商户支付权限不足|The used ```oid_partner``` does NOT have proper payment access|
|3011|用户签约信息不存在|The used ```no_agree``` of user does NOT exist|
|4000|解约失败，请联系发卡行|Card unbinding failed. Please contact your card issuer|
|5001|卡bin校验失败|Card bin identification failed|
|5002|原始交易不存在|Original transaction does NOT exist|
|5003|退款金额错误|Refund amount is inconsistent with the original transaction amount|
|5004|商户状态异常，无法退款|Merchant status is abnormal, refund can NOT be processed|
|5005|退款失败，请重试|Refund failed, please try again|
|5006|商户账户余额不足|Insufficient balance of merchant account|
|5007|累计退款金额大于原交易金额|Accumulated refund amount exceeds the original trade amount|
|5008|原交易未成功|The original transaction is not paid successfully|
|5501|大额行号查询失败|Prcptcd code querying failed|
|5502|信用卡不支持提现|Withdraw is unsupported for credit card|
|5503|商户提现权限不足|Your merchant account does NOT have access to withdraw|
|5504|商户操作权限不足|Your merchant account does NOT have access to proceed operation|
|5505|该银行未进行配置，请联系客服|This bank hasn't been configured. Please contact our supports|
|5506|商户备注解析失败|```memo``` parsing failed|
|5601|支付方式和卡类型不匹配|```pay_type``` does NOT match the used card type|
|6001|卡余额不足|Insufficient fund in bank card|
|6002|该卡号未成功进行首次验证|The ```card_no``` has NOT completed the first verification|
|6601|获取银行请求地址失败，请稍后重试| Obtaining the endpoint of banks failed, please try again later|
|7001|用户已存在|The used ```user_id``` exists already |
|7017|金额不正确|Incorrect ```money_order```|
|7018|新增银行卡用户信息不一致|The user info of the new added bank card is inconsistent|
|7119|分账信息有误（分期付定期扣款）|Incorrect ```shareing_data```|
|7777|需要补全签约信息|Need to complete contract information|
|8000|用户信息不存在|No such user information|
|8001|文件异常|Invalid file|
|8002|生成文件异常|File generation failed|
|8003|解析文件异常|An exception is thrown in parsing file|
|8006|汇兑异常|An exception is thrown in exchanging funds|
|8010|付款人信息不存在|The payer information does not exist|
|8099|实名认证失败|Real-name authorization failed|
|8101|无此扣款计划信息|No such repayment plan exists|
|8102|当前扣款计划已执行，请勿重复执行|The selected repayment date has been executed|
|8103|还款计划通知短信发送失败|Repayment plan notification SMS failed to send|
|8104|扣款失败|Funds deduction failed|
|8105|变更还款次数超过限制|The number to change repayment plan exceeds the limit|
|8106|该签约协议号未授权该扣款协议号进行扣款|The repayment plan has not been authorized by the used ```no_agree```|
|8107|扣款计划校验失败|The verification of repayment plan failed|
|8108|变更计划失败|Changing repayment plan failed|
|8109|计划保存失败|Failed to save repayment plan|
|8110|验证码错误|Invalid verification code|
|8111|用户查询失败|User querying failed|
|8112|变更计划状态失败|Failed to change the repayment state|
|8113|分期计划不存在|No such installment plan exists|
|8114|扣款方式不符|Incorrect ```pay_type```|
|8115|还款卡授权失败|The repayment card failed to authorize the repayment plan|
|8116|定时任务执行失败|Scheduled task execution failed|
|8117|幂等检查异常|An exception is thrown in idempotent check|
|8888|短信已下发，需要再次验证|SMS code sent, another verification is required|
|8901|没有记录|No such record exists|
|8911|没有风控记录|No such payment risk record exists|
|9000|银行维护中，请稍后再试|Selected bank is under maintenance, please try again later|
|9002|报文解析异常|An exception is thrown in parsing requests|
|9091|创建支付失败|Failed to create ```oid_paybill```|
|9092|支付单号与订单号不一致|```oid_paybill``` is inconsistent with the ```no_order```|
|9093|无对应的支付单信息|No corresponding payment information exists|
|9094|请求银行扣款失败|Failed to deduct money from bank side|
|9104|对公账户余额不足|Insufficient balance of business account|
|9700|短信验证码错误|Incorrect SMS verification code|
|9701|短信验证码和手机不匹配|SMS verification code and mobile phone numbers do not match|
|9702|验证码错误次数超过最大次数,请重新获取进行验证|The time of SMS verification exceeds the limits, please re-obtain SMS verification code|
|9703|短信验证码失效,请重新获取|SMS verifaction code expired, please re-obtain it|
|9704|短信发送异常,请稍后重试|Failed to send SMS verification code, please try again later|
|9901|请求报文非法|Invalid request body|
|9902|接口调用异常|An exception is thrown when the interface is invoked|
|9903|请求无效，请稍后重试|Invalid request, please try again later or contact our support|
|9907|银行编码与卡不一致|The ```bank_code``` is inconsistent with the ```card_no```|
|9901|风险等级过高|Payment is suspended due to high risk level|
|9911|金额超过指定额度|The ```money_order``` has exceeded the specified amount|
|9912|该卡暂不支持|The used card is unsupported|
|9913|该卡已签约成功|This card has already been bound successfully|
|9920|退款失败|Failed to refund|
|9921|退款查询失败|Failed to query the refund|
|9970|银行系统日切处理中|There are maintenance in bank system, please try again later|
|9983|商户订单号重复|Duplicate ```no_order```|
|9990|银行交易出错，{银行返回的错误信息}，请稍后重试|Failed to proceed transaction from bank side, please try again later or contact our support|
|9991|银行交易出错，请联系客服，客服热线客服热线：4000188888|Failed to proceed transaction from bank side, please contact our support|
|9999|系统错误|Internal system error|
