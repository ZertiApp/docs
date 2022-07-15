---
layout: default
title: Collection.sol
parent: Zert
nav_order: 4
---

# Collection.sol
{: .no_toc }

Creates a collection of certificaties.

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

* ### CollectionUpdate
    * __Params:__
        * __uint8 indexed eventType:__
            - __0:__ Collection creation.
            - __1:__ Collection modification.
            - __2:__ Collection deletion.
        * __address indexed entity:__ Entity who created/modified/deleted a collection.
        * __uint256 indexed id:__ Collections´s id.
        * __string data:__ URI of the collection data.

---

# Methods

### getData(_id)

__Params:__
* __\_id:__ ID to be queried.

__Returns:__
*  String with the data from collection under `_id`.

View function that gets the data from a specific collection.

__#external view__

---

### getEntity(_id)

__Params:__
* __\_id:__ ID to be queried.

__Returns:__
*  Address of the entity that created collection under `_id`.

Get the creator(entity) of a specific collection.

__#external view__

---

### _createCollection(_account, _data)

__Params:__
* __\_account:__ Address of the entity creating the collection.
* __\_data:__ String with data of the collection.


Creates a collection.  
Emits a {CollectionUpdate-0} event

__#internal__

---

### modifyCollection(_id, _newData)

Calls _modifyCollection(`msg.sender`,`_id`,`_newData`) (See below)

Modifies a collection data.

__#external__

---

### _modifyCollection(_account, _id, _data)

__Params:__
* __\_account:__ Address of the entity modifying the collection.
* __\_id:__ ID of the collection to be modified.
* __\_newData:__ String with new data of the collection.

Emits a {CollectionUpdate-1} event

__#internal__

---

### deleteCollection(_id, _newData)

Calls _deleteCollection(`msg.sender`,`_id`,`_newData`) (See below)

Deletes a collection.

__#external__

---

### _deleteCollection(_account, _id, _data)

__Params:__
* __\_account:__ Address of the entity deleting the collection.
* __\_id:__ ID of the collection to be deleted.

Emits a {CollectionUpdate-2} event

__#internal__
