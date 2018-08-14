# online banking

Online banking payment refers to the payment method that users input bank card information on the bank's online banking page and complete the payment through the bank's authentication methods (such as U shield, payment password, etc.), which is mainly applicable to the PC side. 

Online banking payment is divided into ** personal online banking payment ** and ** enterprise online banking payment **. You can configure it according to actual needs. 
***

## payment process

![](./online-banking-flow.svg)

* Generate order information like ```no_order```、```dt_order```、```money_order```after the user choosing which products to pay.
* The user select to use which bank on Lianlian page and then they'll be redirected to corresponding online banking page to pay.
* Users login online banking system and pay online.
* Process the notificaton and update the payment status.

***

## Implement


1. [Request payment](/docs/online-banking-redirect-api)  Redirect your user to Lianlian payment page.

2. [Synchronous Notification(optional)](/docs/sync-notification)  Lianlian will send synchronous notification to the ```url_return``` after the payment is confirmed as successful.

3. [Asynchronous Notification](/docs/aggregate-asyn-notification.md)  After the payment is successful, the result of the payment will be notified to your server. 

***

