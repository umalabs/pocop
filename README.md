# Proof of Chain of Possession (POCOP) Tokens

## Abstract

This document introduces a POCOP mechanism based on a nested, chained HMACs construction to provide chained authenticity and integrity protection.

## Introduction

Bearer tokens are vulnerable at rest and in transit when an attacker is able to intercept a token to illegally access private information. In order to mitigate some of the risk associated with bearer tokens, proof-of-chain-of-possession may be used to authenticate the token. Chain-of-possession is a chronological tamper-resistant record of all the possessors of tokens and the changes that have been made.

## Concept

**Chained message checksum (integrity protection)**

MAC = HMAC(...HMAC(HMAC(K<sub><i>1</i></sub>, m<sub><i>1</i></sub>), m<sub><i>2</i></sub>,), ...m<sub><i>n</i></sub>)

**Chained proof-of-possession (authenticity)**

MAC = HMAC(K<sub><i>n</i></sub>, ...HMAC(K<sub><i>2</i></sub>, HMAC(K<sub><i>1</i></sub>, m<sub><i>1</i></sub>)))


**Chained authenticity and integrity protection**

MAC = HMAC(K<sub><i>n</i></sub>, ...HMAC(HMAC(K<sub><i>2</i></sub>, HMAC(HMAC(K<sub><i>1</i></sub>, m<sub><i>1</i></sub>), m<sub><i>2</i></sub>)), ...m<sub><i>n</i></sub>))

Example of client chaining with RS_1 and RS_2:

MAC = HMAC(K<sub><i>RS_2</i></sub>, HMAC(HMAC(K<sub><i>RS_1</i></sub>, HMAC(HMAC(K<sub><i>client</i></sub>, m<sub><i>client</i></sub>), m<sub><i>RS_1</i></sub>)), m<sub><i>RS_2</i></sub>))

broken down into individual MACs

MAC = HMAC(K<sub><i>client</i></sub>, m<sub><i>client</i></sub>)

MAC = HMAC(K<sub><i>RS_1</i></sub>, HMAC(MAC, m<sub><i>RS_1</i></sub>))

MAC = HMAC(K<sub><i>RS_2</i></sub>, HMAC(MAC, m<sub><i>RS_2</i></sub>))

These nested, chained HMACs constructions applied on tokens, claims, tickets or cookies may be used to implement both new authorization protocols and to enhance existing ones.

## Acknowledgment

Credits go to [WG - User-Managed Access][1].

[1]: https://kantarainitiative.org/confluence/display/uma/Home