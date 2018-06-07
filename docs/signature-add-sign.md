# Add Sign

In this tutorial, we assume you have complete the [Key configuration](signature-key-configuration.md) part. 

We use a sample from [card bin query API](card-bin-query-api.md) to show how to do the add sign step. Let's say we have following unhandled data:

```json
{
    "card_no": "403392******1234",
    "oid_partner": "201408071000001543",  
    "sign_type": "RSA"
}
```

We will do 2 steps:

1. [Generate signature source](#1.-generate-signature-source)

2. [Encrypt and merge](#2.-encrypt-and-merge)

***

## 1. Generate signature source

Put all needed the parameters with format ```{key}={value}``` and connect them with ```&``` character together, note:
 
* The signature source need to be ascended in order of the first letter. 

* Parameters whose value is ```null``` should be removed.

```html
card_no=403392******1234&oid_partner=201408071000001543&sign_type=RSA
```

***

## 2. Encrypt and merge

Use your private key to encrypt the generated signature source, here we use ```Java``` as an example:

```java
    /**
     * Encrypt function
     * @param prikeyvalue: Your private key
     * @param sign_str: Generated signature source
     * @return
     */
    public static String sign(String prikeyvalue, String sign_str)
    {
        try
        {
            PKCS8EncodedKeySpec priPKCS8 = new PKCS8EncodedKeySpec(
                    Base64Utils.getBytesBASE64(prikeyvalue));
            KeyFactory keyf = KeyFactory.getInstance("RSA");
            PrivateKey myprikey = keyf.generatePrivate(priPKCS8);
            java.security.Signature signet = java.security.Signature
                    .getInstance("MD5withRSA");
            signet.initSign(myprikey);
            signet.update(sign_str.getBytes("UTF-8"));
            byte[] signed = signet.sign(); 
            return new String(org.apache.commons.codec.binary.Base64.encodeBase64(signed));
        } catch (java.lang.Exception e)
        {
            e.printStackTrace();
        }
        return null;
    }
```

Once we have the generated ```Sign```, merged it together with the original data to create a new ```json```:

```json
{
  "card_no": "403392******1234",
  "oid_partner": "201408071000001543",
  "sign": "nOfov/bBLyl0cBTYR5m8pKcn1X5+OUuiNmCR3QlnDWGoMNmVopdJUYUXlr7xBR2FZ+kJGjHYHqg8K3NTXQPHN5UY4brNhuC8Xv6kDaJ8Pc4XsO4os7VUd53cqew+CX9IzBSkQ9u8tldSTfqNyX1Stp3+zBNwD7IC3JBKKtmHdb4=",
  "sign_type": "RSA"
}
```

That's it, we now have a request body for [card bin query API](card-bin-query-api.md).