# Card Bind

Card Bind is a process to record the used card of your customer so that they can select their saved card and finish the payment directly. This process requires your customers to input their below key payment information and then take a SMS verification step.

* **Card number**. The used card number for debit card. If a credit card is used, **expiration date** and **CVV/CVC code** are additionally required.

* **ID card number**. The ID card number of customer.

* **Phone number**. The phone number which is saved in the card issuer of customer.

* **Real name**. Customer's full name.

Whenever a card bind action is successful, LianLian generates a specific parameter ```no_agree```, which represents the payment information of the used card. However it can not be used directly as a token to deduct funds. By default, you can use it in [Exchange Payment Apply API](exchange-payment-apply-api.md) to replace **card_no**, **acct_name**, **id_no** and **bind_mob**.

> If you wish to use ```no_agree``` to deduct your customer's funds, consult our support team to get its requirements and restrictions.

Typically, a user identification, which is ```user_id``` in request, can only be bound to a natural person. Let's say a ```user_id``` has been bound to a person named Bob, it then belongs to Bob and can't be bound to any other person as long as the ```no_agree``` is not unbound. Meanwhile, this ```user_id``` is still available for other Bob's cards to perform card bind action.

Based on the above features, ```no_agree``` can be used to setup a card selection page on your own client side. Your customer will then have the capability to choose their used card and do payments accordingly in their later on payments as long as they have their card bound.

LianLian provide below APIs to help you establishing this feature:

* [Card Bind Apply API](card-bind-apply-api.md)

* [Card Bind Verify API](card-bind-verify-api.md)

* [Card Bind Query API](card-bind-query-api.md)

* [Card Unbind API](card-bind-unbind-api.md)