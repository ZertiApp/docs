---
layout: default
title: SBTDS.sol
parent: Our Token
nav_order: 1
---


# SBTDS.sol
{: .no_toc }

Main SBTDS contract, used for our badge/certification standard.


{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

# Events

* _See [ISBTDS-Events](https://docs.zerti.com.ar/Our-Token/ISBTDoubleSig.html#events)_

---

# Methods

### uri()

* _See [ISBTDS-uri()](https://docs.zerti.com.ar/Our-Token/ISBTDoubleSig.html#uri)_

__#external view__

--- 

### uriOf(_id)

* _See [ISBTDS-uriOf](https://docs.zerti.com.ar/Our-Token/ISBTDoubleSig.html#uriof_id)_

__#external view__

--- 

### ownerOf(_id)

* _See [ISBTDS-uriOf](https://docs.zerti.com.ar/Our-Token/ISBTDoubleSig.html#ownerof_id)_

__#external view__

--- 

### amountOf(_id)

* _See [ISBTDS-amountOf](https://docs.zerti.com.ar/Our-Token/ISBTDoubleSig.html#amountof_id)_

__#external view__

--- 

### tokensFrom(_from)

* _See [ISBTDS-tokensFrom](https://docs.zerti.com.ar/Our-Token/ISBTDoubleSig.html#tokensfrom_from)_

__#external view__

--- 

### pendingFrom(_from)

* _See [ISBTDS-pendingFrom](https://docs.zerti.com.ar/Our-Token/ISBTDoubleSig.html#pendingfrom_from)_

__#external view__

--- 

### transfer(_id,_to)

_Calls #internal \_transfer(`_from(msg.sender)`, `_id`, `_to`)._

* _See [ISBTDS-transfer](https://docs.zerti.com.ar/Our-Token/ISBTDoubleSig.html#transfer_id-_to)_

__#external__

--- 

### transferBatch(_id,_to[])

_Calls #internal \_transferBatch(`_from(msg.sender)`, `_id`, `_to[]`)._


* _See [ISBTDS-transferBatch](https://docs.zerti.com.ar/Our-Token/ISBTDoubleSig.html#transferbatch_id-_to)_

__#external__

--- 

### _mint(_account,_data)

Mints(creates) a token.
Minter must be an entity to mint.

__Params:__
* __\_account:__ address who will mint the token
* __\_data:__ URI to IPFS with data of the token(see [Token-Metadata](https://docs.zerti.com.ar/Our-Token/Token-Metadata.html))

__#internal__

--- 

### claim(_id)

_Calls #internal \_claim(`_account(msg.sender)`, `_id`)._

Claims pending token id `_id` of address `_account`.

__Requirements:__
* `_account` cannot be the zero address.
* `_account` MUST have a pending token under `id`.
* `_account` MUST NOT own a token under `id`.

Emits a {TokenClaimed} event.

__#external__

---

### reject(_id)

_Calls #internal \_reject(`_account(msg.sender)`, `_id`)._

Rejects pending token id `_id` of address `_account`.

__Requirements:__
* `_account` cannot be the zero address.
* `_account` MUST have a pending token under `id`.
* `_account` MUST NOT own a token under `id`.

Emits a {TokenClaimed} event.

__#external__

---

### burn(_id)

_Calls #internal \_burn(`_account(msg.sender)`, `_id`)._

Destroys `_id` token owned by `_account`.

* Requirements:
* `account` cannot be the zero address.
* `account` must have `_id` token.
* There must be at least one token under `_id`.

Emits a {Transfer} event with `to` set to the zero address.

__#external__

---

# Hooks

## Transfering

### _beforeTokenTransfer(_from, _to, _id, _amount)

Hook that is called before any transfer of tokens. This includes minting and burning.

__Params:__ 
* __\_from:__ Address who will make the token transfer.
* __\_to:__ Address who will receive the token transfer.
* __\_id:__ Token id.
* __\_amount:__ Amount of tokens that will be transfered.

__#internal__

---

### _afterTokenTransfer(_from, _to, _id, _amount)

Hook that is called before any transfer of tokens. This includes minting and burning.

__Params:__ 
* __\_from:__ Address who made the token transfer.
* __\_to:__ Address who received the token transfer.
* __\_id:__ Token id.
* __\_amount:__ Amount of tokens that were transfered.

__#internal__

---

## Claiming

### _beforeTokenClaim(_newOwner, _id)

Hook that is called before any token claim. This includes token rejection.  
If `_newOwner` is the cero address, token has been rejected.

__Params:__ 
* __\_newOwner:__ Address who will claim or reject the token under `_id`.
* __\_id:__ Token id.

__#internal__

---

### _afterTokenClaim(_newOwner, _id)

Hook that is called after any token claim. This includes token rejection.  
If `_newOwner` is the cero address, token has been rejected.

__Params:__ 
* __\_newOwner:__ Address who has claimed or rejected the token under `_id`.
* __\_id:__ Token id.

__#internal__