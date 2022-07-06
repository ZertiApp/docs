---
layout: default
title: Vote.sol
nav_order: 5
parent: Voting System
---

# Vote.sol
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

For each Vote contract, users are able to create a "Voting Pool" and to determine if the entity in question should be considered as such. Each vote has its time limit,  minimum votes and voting cost required. It is important to consider that results are given by majority and that each clone of the Vote contract acts independentely. Stores an IPFS URI that contains entity info(see Data)

Each entity postulation has it own Vote.sol instance.

* Implements the DAO Voting System.
* Implements the Voting-Pool Reward System.
* Stores data in a IPFS link.
* Users(voters) interact with cloned instances(See VoteFactory.sol) of this contract.
* Can be upgraded via VoteFactory(See VoteFactory.sol).
* Calls _addEntity(See VoteFactory.sol) if result is 1(True/In favour).
* Stores votes in tree-like structure based on vote(0- Votes Against ; 1-Votes in Favour)
* Implements a time-lock function, restricting function calls when block.timestamp at call exceeds timeToVote
* At vote finish, distributes the pool according to majoritarian vote.

![ZertiVotingPoolPattern](/static/img/VotingPoolPattern.png)
__Voting Pool:__ Stake-ish MATIC (ethers) pool formed by the tokens sent by Users. It is distributed based on majoritarian vote, generating a reward system for people in order to incentivate entity verification.
![ZertiDistributePoolPattern](/static/img/DistributePoolPattern.png)
When a vote finishes, the pool is distributed based on majoritarian vote. This system's sole objective is to incentivate entity verification for the community.

# __Events__

* ### UserVoted
    * __Params:__
        * __address indexed userAddr__ --> address of the voter
        * __uint8 vote__ --> (0- Vote Against ; 1-Vote in Favour)

    _Emmited at each sendVote() call (See sendVote)._

* ### VoteFinished
    * __Params:__
        * __address indexed entity__ --> address of the entity being voted
        * __uint8 vote__ --> (0- Vote lost, entity is not validated ; 1- Vote win, entity is now validated and can emit certificates)

    _emited at voteFinalization()(See voteFinalization)._

    
---

# __Methods__

## __Main Functions:__

### initialize(uint256 \_votingCost, uint256 \_minVotes, uint256 \_timeToVote, address \_sender)

__Params:__
* __votingCost:__ should be N usd, info gathered in the front-end
* __minVotes:__ minimal votes to win votation
* __timeToVote:__ days to vote, should be at least N days.
* __\_sender:__ entity to be verified.

__Reverts on:__
* Already initialized at call
* Call by everyone, if not VoteFactory.


init function, given that cloned contracts cant have constructors. Only callable by VoteFactory contract. Only callable once(It is like a constructor)

---
### sendVote(uint8 \_userVote)

__Params:__
* __\_userVote:__ users vote obtained in front-end. 1 - In favor vote; 0 - Opposing vote.

__Reverts on:__
* msg.value not equal to votingCost.
* msg.sender already voted.
* Not initialized
* block.timestamp at call exceeds set time.
* Voting finished.

Main voting function.  
Users should send correct amount of ethers to vote. Determined at init. and obtainable at getVotingCost().  
Receives ether and stores user's vote and address in tree structure.  
Sets that user has voted.  
Emits a {UserVoted} event.

---
## __View and Info-Retrieving functions:__
### getWhoBeingVoted()
Get entity that is being voted.

__Returns:__
* address of entity being voted

---
### getData()
Get IPFS Link to entity info.
(See Metadata-Standards)

__Returns:__
* string, IPFS Link.


---
### getTotalDeposit()
Get contract balance

__Returns:__
* uint256, balance of the contract(IN GWEI).


---
### getUserVoted(address _addr)
Check if address has already voted

__Params:__
* __\_addr__ address to check.

__Returns:__
* boolean value stating if selected address has already voted.


---
### getInit()
Check if contract is initialized

__Returns:__
* boolean value stating if contract is initiliazed


---
### getVotesAgainst()
Check against votes

__Returns:__
* uint256, number of votes against.


---
### getVotesInFavour()
Check in favour votes

__Returns:__
* uint256, number of votes in favour.


---
### getVotingCost()
get voting cost, if 0, voting closed

__Returns:__
* uint256, neccesary ethers to stake/vote(IN GWEI)


---
### getEndTime()
Get endTime timestamp(In Unix Time!)

__Returns:__
* uint256, block.timestamp at init + timeToVote days.


---
## __Internal Functions:__
### voteFinalization()

Sets vote result and calls distributeVotePool() function

internal and should only be called by an Admin contract.
Emits a {VoteFinished} event; and calls distributePool().

__Reverts On:__
* Voting not finished

### distributePool()
Transfers ether to vote winners.
Distributes reward system pool between winners(majority).  

__Reverts On:__
* Voting not finished
