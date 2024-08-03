# 09 - King

The vulnerability of this contract is a bit tricky. As the description says: 


The contract below represents a very simple game: whoever sends it an amount of ether that is larger than the current prize becomes the new king. On such an event, the overthrown king gets paid the new prize, making a bit of ether in the process! As ponzi as it gets xD.

Such a fun game. Your goal is to break it.

When you submit the instance back to the level, the level is going to reclaim kingship. You will beat the level if you can avoid such a self proclamation.


The thing with this contract, is that the owner can claim kinghship whenever he wants, and we have to prevent that from happening. The answer to do that is in this line of the contract: `payable(king).transfer(msg.value);`

Keep in mind that transfer is a function that sends a certain amount of ether to an address, and more importantly, this function may revert if it doesn't get to fullfill the transfer successfully, which will make the whole program execution stop. Therefore, we have to make the tranfer fail. How can we do that?

To make the transfer fail, we are going to create a smart contract that doesn't accept transfers, but that can send ether to become the king. Thus, when the owner tries to reclaim kingship, the call to the transfer function will revert and both the king and prize won't be updated.

The contract I've created to get this done is this:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract breakKing {
    address owner;
    uint oops;
    address payable addr;

    constructor(address ownerAddr, address payable target) {
        owner = ownerAddr;
        addr = target;
        oops = 0;
    }

    modifier onlyOwner(){
        require(msg.sender == owner);
        _;
    }

    function transferEth() public payable onlyOwner returns(bool) {
        (bool sent,) = addr.call{value: msg.value}("");
        return sent;
    }
}
```

You just need to initialize this assigning your address and the address of your King contract instance, then you call `transferEth()` by sending an enough amount of ether to become king. After doing this, you will become the king forever :)

