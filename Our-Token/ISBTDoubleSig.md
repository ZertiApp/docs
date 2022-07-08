---
layout: default
title: ISBT Double Sig
parent: Our Token
nav_order: 3
---

# ISBT

Interface of the ISBTDoubleSig contract.


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

# __Events__

* ### TokenTransfer
    * __Params:__
        * __address indexed \_from__
        * __address indexed \_to__ 
        * __uint256 id__ 

    - Emitted when `_id` token is transferred from `_from` to `_to`.  
    - If `_from` is the cero address, a token is being minted. 
    - If `_to` is the cero address, a token is being burned.


* ### TokenClaimed
    * __Params:__
        * __address indexed \_newOwner__
        * __uint256 id__ 

    - Emitted when `_newOwner` claims or rejects pending `_tokenId`.  
    - If `_newOwner` is the cero address, token was rejected.

---

# __Methods__

### uri()

__Returns:__
* String of the URI.

Get URI.  

__#external view__

---

### uriOf(_id)

__Params:__
* __\_id:__ uint256 of the ID to be queried.

__Returns:__
* String of `_id` URI.

Get URI of a ID.  

__#external view__

---

### ownerOf(_id)

__Params:__
* __\_id:__ uint256 of the ID to be queried.

__Returns:__
* Address of the owner of `_id`.

get ownerOf a token, given an ID.  

__#external view__

---

### amountOf(_id)

__Params:__
* __\_id:__ uint256 of the ID to be queried.

__Returns:__
* number of tokens under `_id`.

Get amount of tokens under ID(Semi-Fungible).  

__#external view__

---

### tokensFrom(_from)

__Params:__
* __\_from:__ addres to be queried.

__Returns:__
* uint256[] array of IDs, all tokens owned by `_from`.

Get tokens owned by a given address.

__#external view__

---

### pendingFrom(_from)

__Params:__
* __\_from:__ address to be queried.

__Returns:__
* uint256[] array of IDs, all tokens pending to be claimed by `_from`.

Get tokens pending to be claimed by a given address.

__#external view__

---

### transfer(_id, _to)

Transfers `_id` token from `_from` to `_to`.

__Requirements:__
- `_from` cannot be the zero address.
- `_to` cannot be the zero address.
- `_to` MUST NOT have a pending token under `_id`.
- `_from` Must be the minter(owner before assigning `_id` as pending to `_to`) of `_id`.
- `_to` cannot own a token under `_id` at call.
     
Emits a {TokenTransfer} event.

__Returns:__
* a boolean value indicating whether the operation succeeded.

__#external__

---

### transferBatch(_id, _to[])

Transfers `_id` tokens from `_from` to every address at `_to[]`.   
Calls _transfer(`_id`, `_from`, `_to[i]`) len(_to) times.  
Emits a {TokenTransfer} event _to[].length times. 

__Requirements:__
* See {transfer}  

__Returns:__
* a boolean value indicating whether the operation succeeded.

__#external__

