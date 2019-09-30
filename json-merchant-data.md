![Jeton](images/logo-200.png)

# JSON Merchant Data

#### Version: 0.1
#### Date published: September 28, 2019

## Purpose

This specification describes an extension to the Bitcoin Payment Protocol (BIP70) which leverages the merchant_data field in a payment request to store a standardized JSON object which can be used to deliver additional instructions to a user's wallet.

## Specification

### Extending Existing Payment Protocol

This payment protocol is an extension of [BIP 70](https://github.com/bitcoin/bips/blob/master/bip-0070.mediawiki) and the [Simple Ledger Protocol URI Scheme Specification](https://github.com/simpleledger/slp-specifications/blob/token-documents/slp-uri-scheme.md). Applications which support these protocols are able, with minor modifications, to support the payment protocol described in this specification.

### Merchant Data JSON Object

The BIP 70 specification provides for an optional merchant data field in the PaymentDetails message of a payment request.

| Field  | Description (from BIP70 specification)                                 |
| ------------ | ------------------------------------------|
| merchant_data | Arbitrary data that may be used by the merchant to identify the PaymentRequest. May be omitted if the merchant does not need to associate Payments with PaymentRequest or if they associate each PaymentRequest with a separate payment address. |

This field is unused in the wild yet it is recognized and readable by all existing clients which implement BIP 70. The merchant_data field is protocol buffer ``bytes`` data type. The JSON string representing the object is UTF-8 encoded and converted to the appropriate data type for insertion into the merchant_data field.

#### Merchant Data Object Properties

The default object, representing no additional wallet instructions, is an empty object.

**``exact_amount``** - Integer representing the exact number of satoshis that the payee must send, in addition to the minimum required mining fee, in order for the payment to be accepted as valid. If ``exact_amount`` property is present in the merchant data object, then change outputs are not allowed. Any payment that does not have the exact outputs, in the exact order, specified in the PaymentDetails will be rejected as invalid.

**``anyonecanpay``** - Boolean representing whether all inputs in the submitted payment must be of signature hash type ``SIGHASH_ANYONECANPAY``

**``contract``** - Object representing terms of a script contract

**``fiat_symbol``** - String of the three-letter currency code representing fiat currency in which invoice is meant to be denominated

**``fiat_rate``** - Float representing Fiat/Bitcoin excahnge rate. This can be used to override the exchange rate of a wallet or to show the comparative exchange rate to the buyer

**``fiat_amount``** - Float representing invoice amount denominated in ``fiat_symbol``


**Example Merchant Data Object**
```
 {
  "exact_amount": 25000,
  "anyonecanpay": true,
  "contract": {
                "refereePubKey":"0252d464cde7e046c7a38fdbedceda0038329ec00a87a16400d506c1806a53603d",
                "parties": 
                [
                  {
                    "message":"player1wins",
                    "amount":13000,
                    "index":0,
                    "address":"bitcoincash:qrzel24gusq488qdlqsmu3pq6yfxpj3h659ayr6mrm"
                  },
                  {
                    "message":"player2wins",
                    "amount":12000,
                    "index":1,
                  "address":"bitcoincash:qpaqwcmsdtn0na94m4ztaqvez5lasafxzuyx8e67yd"
                  }
               ]
              }
}
```
