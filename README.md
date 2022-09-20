# freeCodeCamp-blockchain
Metamask seed: lecture entire zone coin lottery riot regret muffin pilot glove night pig

![2022-09-19 18_00_25-Rinkeby Transaction Hash (Txhash) Details _ Etherscan](https://user-images.githubusercontent.com/67793141/191144667-d0899259-3742-4cc2-b8b6-0dc471e5fbec.png)


Transaction fee = gas price * usage = 0.000000002500000008 * 21000 = 0.0000525


Blockchain miners need to find the nonce to solve the equation to get X amount of 0s at the beginning

Asymmetric encryption is used to verify transactions
Private Key -> sign -> send -> recipient use sender’s public key to verify

Higher on-chain activity leads to higher gas fee
Can set max gas fee

Base gas fee is burned, the rest goes to the miner, different EIP version has different gas model

Need to learn more about different levels of Ethereum and what do different most popular crypto does

Each contract also has its own address

Whenver you deploy/ modify a contract, it counts as a transaction since you modify the chain to include the contract

Solidity variable initial value follows “zero state”: 
- bool: false
- int: 0
- string: “”
- array: empty
Ref: https://docs.soliditylang.org/en/develop/control-structures.html#default-value

View and pure function do not cost gas
View: only read from the chain
Pure: more restrictive then view, not even reading from chain
=> If these function gets called from inside a regular function still gets gas

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.8;
 
contract SimpleStorage {
    uint256 favoriteNumber;
 
    // takes gas
    function store(uint256 _favoriteNumber) public {
        favoriteNumber = _favoriteNumber;
    }
 
    // takes no gas
    function viewFunc() public view returns(uint256) {
        return favoriteNumber;
    }
 
    // takes no gas
    function pureFunc() public pure returns(uint8) {
        return 1;
    }
}
```
=> Gas only spent when chain is modified

Storage: permanent on chain
Memory: modifiable, last the duration of the function
Calldata: cannot be modified, last the duration of the function

To interact with a contract, you need ADDRESS + ABI (Application Binary Interface)
