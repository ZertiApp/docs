---
layout: default
title: VoteFactory.sol
nav_order: 5
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

    
    _Emited at fallback function._
    
* ### EntityAdded
    * __Params:__
        * __address indexed \_newEntity__ -> New validated entity
        * __address \_caledBy__ -> Contract(VoteProxy) that called the function.

    _Emited at Vote finish, from a Vote clone, if result if 1._

* ### ProxyCreated
    * __Params:__
        * __address indexed proxy__ -> Address of the newly created vote clone/instance.
    
    _Emited at Vote cloning._

---
# __Methods:__

### createVote(uint256 votingCost, uint256 minVotes, uint256 timeToVote)

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

```solidity
function createVote(
    uint256 _votingCost,
    uint256 _minVotes,
    uint256 _timeToVote
) public payable {
    address _voteProxy = this.deployMinimal(voteImpl);

    Vote(_voteProxy).initialize(
        _votingCost,
        _minVotes,
        _timeToVote,
        msg.sender
    );

    postulations[msg.sender] = true;
    clones[_voteProxy] = true;
}
```
---

### rePostulationAllowance()

Allows entity to repostulate. If entity sends especified amount of ethers to the function, it is allowed to iniciate a new vote.
Entity should not be validated(lost at previous votation).

__Reverts on:__
* Call by an already validated entity.
* msg.value != reAlowanceValue

```solidity
function rePostulationAllowance() public payable {
    if(postulations[msg.sender] && entities[msg.sender])
        revert AlreadySelectedAsEntity(msg.sender);
    if(msg.value != reAlowanceValue)
        revert InvalidInput(msg.value);
    
    postulations[msg.sender] = false;
}
```
---
## _View and info-retrieving methods_
### getImplAddr()

Get the current implementation Address
__Returns:__
* address of the Vote contract from which clones are created.

```solidity
function getImplAddr() external view returns (address) {
    return voteImpl;
}
```

---
### getAdmin()

Get the current administrator Address
__Returns:__ 
* address of contract admin.

```solidity
function getAdmin() external view returns (address) {
    return admin;
}
```
---
### isEntity(address \_addr)

Check if a given address is a validated entity.

__Params:__
* __\_addr__ address of the entity to be queried.

__Returns:__ 
* Boolean value stating if address is a validated entity.  

```solidity
function isEntity(address _addr) external view returns (bool) {
    return entities[_addr];
}
```
___
### getPostulated(address \_addr)

Check if a given address has postulated.

__Params:__
* __\_addr__ address of the entity to be queried.

__Returns:__
* Boolean value stating if address has postulated.

```solidity
function hasPostulated(address _addr) external view returns (bool) {
    return postulations[_addr];
}
```
---
## _Admin methods_  
### changeImpl(address \_newVoteImpl)

__Params:__ 
* __\_newVoteImpl__ address of the new Vote implementation

Only callable by Admin.  
Changes the address from which vote contracts are cloned.

__Reverts on:__
* Call by everyone if not Admin.


```solidity
function changeImpl(address _newVoteImpl) public onlyAdmin {
    voteImpl = _newVoteImpl;
}
```
---
### changeAlowanceValue(uint256 \_newValue)

__Params:__
* __\_newValue__ new amount to pay when repostulating.

Only callable by Admin.  
_newValue should be parsed as GWEI.  
Changes the value of reAlowanceValue.

__Reverts on:__
* call by everyone if not Admin.
* '_newValue' equal to cero or equal to previous value.



```solidity
function changeAlowanceValue(uint256 _newValue) external payable onlyAdmin {
    if(_newValue == reAlowanceValue || _newValue == 0)
        revert InvalidInput(_newValue);
    reAlowanceValue = _newValue;
}
```
