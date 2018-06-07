# Verify Sign

Verify sign is similar to [Add Sign](signature-add-sign.md), now we use a sample notification to show the logic. Let's say we have obtained an asynchronous notification and get its body like this:

> We assume you have downloaded the public key which is provided by LianLian. 

```json
{
    "bank_code": "01020000",
    "dt_order": "20180518102511",
    "money_order": "0.01",
    "no_order": "1526610305612",
    "oid_partner": "201103171000000000",
    "oid_paybill": "2018051845781369",
    "pay_type": "P",
    "result_pay": "SUCCESS",
    "settle_date": "20180518",
    "sign": "fh5bg2w6fvsUsgt+Qo+gl9s+6okHOL186+Mr+DW8eNQJyEwI98RHIFO6/XApGgcdwXPfKevTPAyymlK1Q0F6DmIkMSGSUL2u1SM7CZCU7mPdD8xDUAin83TnH0WdTO+AJj55HpQDkEpExrmEdGQPbVpuepnUOB0fLFX3cSiFqVU=",
    "sign_type": "RSA"
}
```

A verify sign flow contains 2 step:

1. [Generate signature source](#1.-generate-signature-source)。

2. [Verify it](#2.-verify-it)。


***

## 1. Generate signature source

Take parameter ```sign``` and its value out and then put all the other parameters with format ```{key}={value}``` and connect them with ```&``` character together, note:
 
* The signature source need to be ascended in order of the first letter. 

```html
bank_code=01020000&dt_order=20180518102511&money_order=0.01&no_order=1526610305612&oid_partner=201103171000000000&oid_paybill=2018051845781369&pay_type=P&result_pay=SUCCESS&settle_date=20180518&sign_type=RSA
```

***

## 2. Verify it

Execute the verify function with ```sign``` and the generated signature source as well as the public key provided by LianLian. Here is a sample using ```Java```:

```java
    /**
     * RSA签名验证
     *
     * @param reqObj: The obtained asynchronous notification body
     * @param rsa_public: The public key provided by LianLian
     * @return
     */
    private boolean checkSignRSA(JSONObject reqObj, String rsa_public)
    {
        if (reqObj == null)
        {
            return false;
        }
        String sign = reqObj.getString("sign");
        String sign_src = getInstance().generateSignSrc(reqObj);
        try
        {
            if (TraderRSAUtil.checksign(rsa_public, sign_src, sign))
            {
                return true;
            } else
            {
                return false;
            }
        } catch (Exception e)
        {
            return false;
        }
    }
```

You can continue with your own delivery logic once the verification is successful.