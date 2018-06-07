# Preparation

In this section you will learn about the required steps before integration and the process of how project goes live. 

***

## Content

* [Create your account](#create-your-account)

* [Config your account](#config-your-account)

* [Setup signature function on your server](#setup-signature-function-on-your-server)

* [Project Go Live](#project-go-live)

***

## Create your account

All the interactions with LianLian requires you to have your own account in our system, before account creation, we will apply following flows with your company entity:

* **Compliance**. The business of your company must comply the restrictions and policies of LianLianPay, Inc. 

* **Due Diligence**. According to the national policies as well as the regulations from Union Pay organizations, LianLian requires you to provide relevant documentations to complete the due diligence flow. 

You can consult our business development colleague about these 2 process, they will help you to complete your account creation flow. A merchant identification will be assigned to your company once it is finished, which is named as ```oid_partner```, is required for usage later on.

***

## Config your account

There are 2 kinds of access verification performed in each HTTP request which is sent to LianLian, one is **product access verification** and another is **request source access verification**. **Product access verification** checks whether your ```oid_partner``` has the correspond product access of the API which your request is sent to. **Request source access verification** apply a review about whether your request is sent from whitelisted source. 

Typically, product accesses shall properly be configured as long as ```oid_partner``` is created(Our business development colleague will communicate with you about the product configuration before account creation). If you have experienced  a tip like ```You don't have proper product access```, you can double check with our business development colleague. 

For **request source verification**, you need to send us the information of your environment depending on your integration type:

* If you are using direct API integration, server IP address information required.

* If you have integrated our redirect API, source domain name is required.

***

## Setup signature function on your server

Signature is required for data transmission between you and LianLian. You may refer to [signature](signature.md) to setup the logic on your server.

***

## Project Go Live

By default, each configured product under your merchant account has an amount limitation which is 50/transaction, 50/day, 50/month (in CNY) for each customer.

If you have completed the integration and would like to run our service in your production environment, you need to inform our colleagues to start the flow of project go live and proceed with below 2 actions:

* Make a successful test transaction and send it to our technical support colleague to review the integration of your project.

* Prepare a environment where our colleague from due diligence team can perform payment test.

> Each product is required to take a go live progress, if you have multiple payment products integrated you will have to do the flow accordingly. Once the review request is approved, you will be able to use it :)。 This process usually takes 1 ~ 2 working days.
