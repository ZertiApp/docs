---
layout: default
title: VoteFactory.sol
nav_order: 2
parent: Voting System
---

# VoteFactory.sol
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


We will be calling acaemic institutes and/or enterprises wishing to emit certificates entities. When one of such postulates as one, it interacts with the VoteFactory contract, which uses an upgradeable EIP1167 implementation, creating a clone of the Vote contract.

* Implements EIP1167 standard to clone instances of the Vote.sol contract.  
* Stores all deployed clones at clones mapping.  
* Stores all validated entities at entities mapping.  
* Entities interact wit this contract when postulating/repostulating.
* Badge contracts interact with IVF to check if an address is an entity.
* Devs can interact with the View Methods to retrieve information about the current state of the contract.
* Admin should interact with VoteFactory to change contract state. 

![ZertiProxyPattern](/static/img/ProxyPattern.png)

# __Events__

* ### EthReceived
    * __Params:__
        * __address indexed \_sender__
        * __uint256 \_amount__

    
    -Emited at fallback function.
    
* ### EntityAdded
    * __Params:__
        * __address indexed \_newEntity__ -> New validated entity
        * __address \_caledBy__ -> Contract(VoteProxy) that called the function.

    - Emited at Vote finish, from a Vote clone, if result if 1.

* ### ProxyCreated
    * __Params:__
        * __address indexed proxy__ -> Address of the newly created vote clone/instance.

    - Emited at Vote cloning.

---
# __Methods:__

### createVote(votingCost, minVotes, timeToVote)

__Params:__
* __votingCost:__ should be N usd, info gathered in the front-end
* __minVotes:__ minimal votes to win votation
* __timeToVote:__ days to vote, should be at least N days.

__Returns:__
* A boolean value indicating whether the operation succeeded.

Can be called by everyone.
Creates an instance of the vote proxy contract and emits a {ProxyCreated} event, then initializes the Clone/Proxy.
Entities call this function at postulation.

__Reverts on:__
* Invalid input: _votingCost == 0 or _minVotes < 2 or _timeToVote < 2
* msg.sender already postulated

__#external__

---

### rePostulationAllowance()

Allows entity to repostulate. If entity sends especified amount of ethers to the function, it is allowed to iniciate a new vote.
Entity should not be validated(lost at previous votation).

__Reverts on:__
* Call by an already validated entity.
* msg.value != reAlowanceValue

__#external__

---
## __View and info-retrieving functions:__
### getImplAddr()

Get the current implementation Address
__Returns:__
* address of the Vote contract from which clones are created.

__#external view__

---
### getAdmin()

Get the current administrator Address
__Returns:__ 
* address of contract admin.

__#external view__

---
### isEntity(\_addr)

Check if a given address is a validated entity.

__Params:__
* __\_addr__ address of the entity to be queried.

__Returns:__ 
* Boolean value stating if address is a validated entity. 

__#external view__
___
### getPostulated(\_addr)

Check if a given address has postulated.

__Params:__
* __\_addr__ address of the entity to be queried.

__Returns:__
* Boolean value stating if address has postulated.

__#external view__

---  
### changeImpl(\_newVoteImpl)

__Params:__ 
* __\_newVoteImpl__ address of the new Vote implementation

Only callable by Admin.  
Changes the address from which vote contracts are cloned.

__Reverts on:__
* Call by everyone if not Admin.


__#external(adm)__

---
### changeAlowanceValue(\_newValue)

__Params:__
* __\_newValue__ new amount to pay when repostulating.

Only callable by Admin.  
_newValue should be parsed as GWEI.  
Changes the value of reAlowanceValue.

__Reverts on:__
* call by everyone if not Admin.
* '_newValue' equal to cero or equal to previous value.

__#external(adm)__
