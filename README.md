# Proof of Chain of Possession (POCOP) Tokens

## Abstract

This document introduces a POCOP mechanism based on a nested, chained HMACs construction to provide chained authenticity and integrity protection.

## Introduction

Bearer tokens are vulnerable at rest and in transit when an attacker is able to intercept a token to illegally access private information. In order to mitigate some of the risk associated with bearer tokens, proof-of-chain-of-possession may be used to authenticate the token. Chain-of-possession is a chronological tamper-resistant record of all the possessors of tokens and the changes that have been made.

## Concept

#### Chained MACs with Multiple Messages

The MAC<sub><i>final</i></sub> value is calculated starting with HMAC root key K<sub><i>root</i></sub> and the first message m<sub><i>1</i></sub>, each MAC value being used as the HMAC key for the next message.

MAC<sub><i>final</i></sub> = HMAC(...HMAC(HMAC(K<sub><i>root</i></sub>, m<sub><i>1</i></sub>), m<sub><i>2</i></sub>,), ...m<sub><i>n</i></sub>)

Google Macaroons are based on this construction.

#### Chained MACs with Multiple Keys

The MAC<sub><i>final</i></sub> value is calculated starting with the first HMAC key K<sub><i>1</i></sub> and the root message m<sub><i>root</i></sub>, each MAC value being used as the HMAC message for the next Key.

MAC<sub><i>final</i></sub> = HMAC(K<sub><i>n</i></sub>, ...HMAC(K<sub><i>2</i></sub>, HMAC(K<sub><i>1</i></sub>, m<sub><i>root</i></sub>)))

This construction provides the basis of the POCOP mechanism.

#### Experimental Chaining Construction

MAC<sub><i>final</i></sub> = HMAC(K<sub><i>n</i></sub>, ...HMAC(HMAC(K<sub><i>2</i></sub>, HMAC(HMAC(K<sub><i>1</i></sub>, m<sub><i>1</i></sub>), m<sub><i>2</i></sub>)), ...m<sub><i>n</i></sub>))

Example of client chaining with RS_1 and RS_2:

MAC<sub><i>final</i></sub> = HMAC(K<sub><i>RS_2</i></sub>, HMAC(HMAC(K<sub><i>RS_1</i></sub>, HMAC(HMAC(K<sub><i>client</i></sub>, m<sub><i>client</i></sub>), m<sub><i>RS_1</i></sub>)), m<sub><i>RS_2</i></sub>))

broken down into individual MACs

MAC = HMAC(K<sub><i>client</i></sub>, m<sub><i>client</i></sub>)

MAC = HMAC(K<sub><i>RS_1</i></sub>, HMAC(MAC, m<sub><i>RS_1</i></sub>))

MAC<sub><i>final</i></sub> = HMAC(K<sub><i>RS_2</i></sub>, HMAC(MAC, m<sub><i>RS_2</i></sub>))

This combined construction with multiple messages and multiple keys is applicable to the JWT format.

### Intermediate Conclusion

Nested, chained HMACs constructions applied on tokens, claims, tickets, cookies and macaroons may be used to implement both new authorization protocols and to enhance existing ones.

## POCOP Token Mechanism

The Chained MACs with Multiple Keys construction is used as the basis of the POCOP token mechanism.

The root message of the token must contain:

* The random NONCE to prevent replay attack.

* The claim that identifies who created the token.

* The timestamp of when the token was issued.

The claims can be chained using the Chained MACs with Multiple Messages construction to form a composite [UMA Macaroons][6] mechanism.

## Conclusion

...

## Acknowledgment

Credits go to [WG - User-Managed Access][1] and [Google Research Publications][2].

[1]: https://kantarainitiative.org/confluence/display/uma/Home
[2]: https://research.google/pubs/pub41892/
[3]: https://github.com/umalabs/uma-pocop-tokens#chained-macs-with-multiple-messages
[4]: https://github.com/umalabs/uma-pocop-tokens#chained-macs-with-multiple-keys
[5]: https://github.com/umalabs/uma-pocop-tokens#pocop-mechanism
[6]: https://github.com/umalabs/uma-macaroons