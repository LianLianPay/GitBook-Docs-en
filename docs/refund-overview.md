# Refund 

In this part, you will learn about the interactions with our refund services.

***

## Introduction of Money Back Operations

There are 2 major money back operations, one is called **Refund** and another is **Chargeback**.

* **Refund** is an operation initiated by you to return the whole or part of the transaction amount to your customer. You may request our [Refund Apply API](refund-apply-api.md) directly in this case.

Typically, the refund operation only supports those transactions which occurs within 6 months. The refund request behaves as a ticket to our gateways, that are banks. For the status of a refund transaction, LianLian return the result of acceptance of our gateways but **NOT** the result of whether the funds is returned to your customer's account. The specific funds arrival time is based on capability of refund process of banks.

* **Chargeback** is an operation initiated by your customer from his/her card issuer, that is transaction dispute. It usually happens when the card information is used without their authority or the cardholder wants a refund but got no approval or they are not satisfied with the service/goods. 

**Refund** services take no part in this case, LianLian will cooperate with our gateways and contact with you to make sure each dispute is handled in a reasonable way. Consult our support team for more details.   

***

## Refund APIs

* [Refund Apply API](refund-apply-api.md)

* [Refund Result Query API](refund-result-query-api.md)