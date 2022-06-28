---
weight: 5
title: Account Opening
summary: Open brokerage accounts, enable crypto and stock trading, and manage the ongoing user experience with Alpaca Broker API
---

# Customer Account Opening

If you are a fully-disclosed broker-dealer, an RIA, or a trading app setup, you
can open your end customer’s account using Account API. The POST method allows
you to submit all KYC information to Alpaca. There are slight differences
between setups.

![account-opening-flow](account-flow2.png)

## **Trading/Investing App and RIA**

In this use case, Alpaca is responsible for the account approval step, while you
can own the user experiences for collecting the end-customer information. We
require you to collect a set of the information required for our approval
process.

Upon the POST request, the account status starts from `SUBMITTED` status. Alpaca
system will run the automatic KYC process asynchronously and update the KYC
result as the account status. You can receive such updates in the
[Event API]({{< relref "/api-references/broker-api/events/_index.md" >}}) stream.

If all KYC information is verified without problems, the account status will be
`APPROVED` and shortly transition to `ACTIVE`. In some cases, if the final approval
is pending, the account status becomes `APPROVAL_PENDING` which will transition to
`APPROVED` once it is approved. In the case of some action is required, the status
becomes `ACTION_REQUIRED` and you will receive the reason for this. In most cases,
you will need to collect additional information from the end user. One example
would be that the residential address is not verified, so a copy of a document
such as a utility bill needs to be uploaded. You can use
[Document API]({{< relref "/api-references/broker-api/documents/_index.md" >}}) to upload
additional documents when requested.

## **Fully-Disclosed Broker-Dealer**

As a reminder, in this setup, you are required to have a proper broker-dealer
license in your local jurisdiction and you are the broker on the record. Alpaca
relies on your KYC process to open customers' accounts.

In this case, as soon as a `POST` request is made and all fields are validated,
then the account status starts from `APPROVED` status, meaning you have approved
the account opening. Therefore, you need to complete your KYC for the account
before making the `POST` request. The status will shortly become `ACTIVE` which
indicates it’s ready for funding and trading.

## **Non-Disclosed Broker-Dealer**

In a non-disclosed setup, you will use [Account API]({{< relref
"/api-references/broker-api/accounts/accounts.md" >}}) to create a sub account that
represents one of your customer’s accounts. The supplied field for the `POST`
request will be minimal as Alpaca does not need to collect such information. The
account is named as anonymous. The account status will become `ACTIVE`
immediately.

## **Omnibus Broker-Dealer**

In an omnibus setup, you will not request any new account opening. Your trading
accounts will be set up by Alpaca when the go-live is approved. That said, you
may want to simulate this structure using [Account API]({{< relref
"/api-references/broker-api/accounts/accounts.md" >}}) and you can open as many
accounts as you want in the sandbox environment even if you are an omnibus.

## **Account Type**

Alpaca currently opens all accounts as margin accounts. We support individual
taxable accounts and business accounts. Other types of accounts such as cash,
IRA, and custodial accounts are on our roadmap.

Even though all accounts at Alpaca are margin accounts, you have the ability to set
accounts to be cash accounts (100% buying power) to disable margin trading for your
users through account configurations [here]({{< relref "../../../api-references/broker-api/trading/trading-configurations.md">}}).

&nbsp;
