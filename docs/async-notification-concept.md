# Asynchronous Notification

Asynchronous notification is a HTTP request which is sent from LianLian to notify you about the status of a specific transaction. 

In this section, you will learn about the common concept and features of asynchronous notification.

***

## The Features of Asynchronous Notification

Generally, a asynchronous notification has below features:

* It is written in ```InputStream```. Here are several examples about the way to get it:

```text
file_get_contents("php://input"); // php

request.getInputStream(); // Java

Request.inputStream; // c#
```

* Its ```Content-type``` is ```text/json;charset=utf-8```.

* It is sent from below IP addresses, which we recommend you to add in whitelist:

```text
115.236.102.36，101.71.136.20，112.17.38.116，115.238.30.68，118.184.186.141，118.184.156.45
218.4.207.155，218.4.207.156
112.80.55.210，112.80.55.211
223.112.79.242，223.112.79.243
```  

> The HTTP protocol of asynchronous notification is set according to the protocol of ```notify_url``` which you set in the original request. The recommended version of ```TLS``` is not lower than ```1.2``` if you are using ```HTTPS``` protocol.

***

##  Handle Asynchronous Notification

For all the asynchronous notifications, your system need to do 2 steps before executing delivery logic:

* **Verification**, that is, verify the signature and the source of the HTTP request you just obtained to make sure its authenticity. You may check whether it is sent from whitelisted IP addresses and do the [signature validation](signature-verify-sign.md).

* **Send response**. For asynchronous notifications which passed the verification step, you need to send below ```json``` object to inform LianLian that you have received our notification. Otherwise we would repeatedly send asynchronous notification until we receive the expected response or the count of total re-send operation has exceeded its limitation.

```json
{
    "ret_code":"0000",
    "ret_msg":"ok"
}
```

Assuming there is no issues from internet layer, if we haven't received your response within ```5``` seconds, asynchronous notification mechanism would mark the result as FAILURE and send it again. 

Resending logic: 30 times in total with an interval of 2 minutes, until your server has correctly handled the notification. But if we never got the expected response after 30 times, the resending action would be stopped. In this case,  you will have to do the query actions by yourselves. 

However, there are few cases like internet traffic jam, network delay, packet loss or other causes which results in an unexpected phenomenon, that your server has received 2 same asynchronous notification for a same transaction. In case of it, your system **MUST** have the capability to handle duplicated asynchronous notifications, respond them with the expected response of LianLian but proceed your successful logic only **ONCE**. The financial risks which caused by repeated asynchronous notification borne by yourselves.


