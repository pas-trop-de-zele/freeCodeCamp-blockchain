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
```solidity
using PriceConverter for uint256;
// Allows calling PriceConverter methods on uint256 type
uint256 num;
num.PriceConverterMethod();
```

Prior to 0.8, math is unchecked, meaning `uint8 num = 255; num += 1 => 0 // wrap over`
After 0.8, it will throw error, can do `unchecked` to be more has efficient if the num will never be over limit

`constant` and `immutable` can help save gas
- `constant` variable is set at compile-time
- `immutable` is set at run time in constructor

Use `modifier` to enhance function
```
function sayHi(string memory name) {
  return string.concat("Hello ", name)
}

modifier sayHi(string memory name) {
 require(bytes(name).length > 0, "Name is empty");
 _; // Execute the rest of the function
)
```

`receive` and `fallback` trigger when ETH is sent but not match any function signature, in other words, just plainly send ETH to contract would trigger either
```
receive external payable {
 // Function gets call when someone sends ETH
}

receive external payable {
 // Function gets call when someone sends ETH with data
}

// Explainer from: https://solidity-by-example.org/fallback/
// Ether is sent to contract
//      is msg.data empty?
//          /   \ 
//         yes  no
//         /     \
//    receive()?  fallback() 
//     /   \ 
//   yes   no
//  /        \
//receive()  fallback()
```

A use case for `receive` would be to wrap it around another function
```
function fund public payable() {...}

receive external payable() {
 fund();
}
```

