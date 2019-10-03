![Jeton](images/logo-200.png)

# JSON Merchant Data Contract Protocol

#### Version: 0.1
#### Date published: October 3, 2019

## Purpose

This specification describes an extension to the JSON Merchant Data specification and defines a structure for the representation of advanced multi-party script contracts. The purpose of this specification is to create a standard allowing parties to independently verify contract terms before committing to funding and to be able to produce the necessary unlocking script (scriptSig) to spend a contract output

## Specification

### Extending Existing Payment Protocol

This payment protocol is an extension of [JSON Merchant Data](https://github.com/jeton-tech/payment-protocol-extensions/blob/master/json-merchant-data.md) which it, itself, an extension of [BIP 70](https://github.com/bitcoin/bips/blob/master/bip-0070.mediawiki) and the [Simple Ledger Protocol URI Scheme Specification](https://github.com/simpleledger/slp-specifications/blob/token-documents/slp-uri-scheme.md). Applications which support these protocols are able, with minor modifications, to support the payment protocol described in this specification.

### Multi-Party Contracts

Bitcoin locking scripts (scriptPubKey)m which would otherwise be considered non-standard, such as advanced script contracts between multiple parties, must be formatted as [Pay-To-Script-Hash (P2SH)](https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki). The BIP70 payment protocol supports paying to P2SH outputs, however, the scriptPubKey itself does not allow a user to see the full scriptPubKey data. This means that a party to such a contract needs some other way to independently and trustlessly verify the contract terms.

By utilizing a ``contract`` field in the JSON Merchant Data of a BIP 70 Payment Request, a user's wallet can both verify that the proposed P2SH output is funding a particular contract, as well as be able, at a future date, to create the subscript required as part of the scriptSig for spending the P2SH output. 
