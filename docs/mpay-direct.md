# MPAY Payment API 

MPAY Payment API is a direct payment solution for domestic cards in China mainland. We currently support almost all types of the cards from different issuers in China mainland. 

Before you start, the concept of card binding is required to be understood.

***

## What is card binding?

Card binding is generally a step which setup a connection between your customer's card with LianLian by verifying below key payment information:

* Real name. The name of card holders.
* ID number. The ID card number of card holders.
* Phone number. The saved phone number on bank side.
* Card number.

> For credit card, expiration date and CVV/CVC code are additionally required.

These elements are typically collected via pages hosted by LianLian and would be sent to banks to complete verification. A SMS code verification step takes place for verifying user's phone number during the whole process.

Once the verification is done, a special parameter named ```no_agree``` is generated. ```No_agree``` represents all the key payment information mentioned above and can be used in payment request directly. You may use [card bind query API](card-bind-query-api.md) to build your own page to display users' binding card list.

> [Card unbind API](card-bind-unbind-api.md) is available to remove ```no_agree``` if needed.

***


## Dierect APIs

* Below is a typical UML sequence diagram for the interactions with MPAY Payment API .

![](../assests/MPAY_direct_api.svg)




