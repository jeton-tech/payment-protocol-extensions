![Jeton](images/logo-200.png)

# JSON Merchant Data Contract Protocol

#### Version: 0.1
#### Date published: October 3, 2019

## Purpose

This specification describes an extension to the JSON Merchant Data specification and defines a structure for the representation of advanced multi-party script contracts. The purpose of this specification is to create a standard allowing parties to independently verify contract terms before committing to funding and to be able to produce the necessary unlocking script (scriptSig) to spend a contract output

### Multi-Party Contracts

Bitcoin locking scripts (scriptPubKey) which would otherwise be considered non-standard, such as advanced script contracts between multiple parties, must be formatted as [Pay-To-Script-Hash (P2SH)](https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki). The BIP70 payment protocol supports paying to P2SH outputs, however, the scriptPubKey itself does not allow a user to see the full scriptPubKey data. This means that a party to such a contract needs some other way to independently and trustlessly verify the contract terms.

By utilizing a ``contract`` field in the JSON Merchant Data of a BIP 70 Payment Request, a user's wallet can both verify that the proposed P2SH output is funding a particular contract, as well as be able, at a future date, to create the subscript required as part of the scriptSig for spending the P2SH output.

## Specification

### Extending Existing Payment Protocol

This payment protocol is an extension of [JSON Merchant Data](https://github.com/jeton-tech/payment-protocol-extensions/blob/master/json-merchant-data.md) which it, itself, an extension of [BIP 70](https://github.com/bitcoin/bips/blob/master/bip-0070.mediawiki) and the [Simple Ledger Protocol URI Scheme Specification](https://github.com/simpleledger/slp-specifications/blob/token-documents/slp-uri-scheme.md). Applications which support these protocols are able, with minor modifications, to support the payment protocol described in this specification.

### JSON Merchant Data Contract Object

The object described in this specification is stored in the ``contract`` property of the JSON Merchant Data object.

#### Contract Object Properties

The contract object is required to contain the following properties:

**``compiler``** - String with standardized name of library used to compile contract. Examples include *spedn*, *cashscript*, and *jeton-lib*

**``version``** - String with identifier of contract version

All other included properties correspond to the required arguments that are passed to the contract ``version`` in the ``compiler`` library. This structure should be defined in a manifest within the compiler code.

#### Example

The original [jeton-lib escrow contract](https://github.com/jeton-tech/jeton-lib/blob/master/lib/escrow/OutputScript.js) (`version` e001) requires that an object is passed to it containing a *refereePubKey* property as well as a *parties* property consisting of an array of objects, each of which has a *message* and *pubKey* (or *address*) property.

A minimum valid example Contract Object for this escrow contract would appear as follows:

```
{
  "compiler": "jeton-lib",
  "version": "e001",
  "refereePubKey":"0252d464cde7e046c7a38fdbedceda0038329ec00a87a16400d506c1806a53603d",
  "parties": 
  [
    {
      "message":"player1wins",
      "address":"bitcoincash:qrzel24gusq488qdlqsmu3pq6yfxpj3h659ayr6mrm"
    },
    {
      "message":"player2wins",
      "address":"bitcoincash:qpaqwcmsdtn0na94m4ztaqvez5lasafxzuyx8e67yd"
    }
 ]
}
```

This object can be passed directly into the constructor for the corresponding escrow contract in ``jeton-lib`` and used to generate the contract scriptPubKey. Arbitrary data is allowed as additional properties throughout the object so long as so doing does not corrupt the contract compilation. Such additional arbitrary data may be useful in specific applications and allows for future extension of this specification.
