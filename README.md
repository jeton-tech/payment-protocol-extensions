# Payment Protocol Extensions
Extensions of the BIP 70 Payment Protocol


#### [JSON Merchant Data](https://github.com/jeton-tech/payment-protocol-extensions/blob/master/json-merchant-data.md)
This specification describes an extension to the Bitcoin Payment Protocol (BIP70) which leverages the merchant_data field in a payment request to store a standardized JSON object which can be used to deliver additional instructions to a user's wallet.

#### [JSON Merchant Data Contract](https://github.com/jeton-tech/payment-protocol-extensions/blob/master/json-contract.md)
This specification describes an extension to the JSON Merchant Data specification and defines a structure for the representation of advanced multi-party script contracts. The purpose of this specification is to create a standard allowing parties to independently verify contract terms before committing to funding and to be able to produce the necessary unlocking script (scriptSig) to spend a contract output.

#### [Multi-Party Merge Transactions](https://github.com/jeton-tech/payment-protocol-extensions/blob/master/multi-party-merge.md)
This specification describes an extension to the Bitcoin Payment Protocol (BIP70) which leverages the SIGHASH_ANYONECANPAY signature hash flag to combine two or more partial transactions, from separate parties, into a single transaction. This protocol is useful for "blind" funding of complex script contracts by multiple parties.
