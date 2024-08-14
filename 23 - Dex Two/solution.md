# 23 - Dex Two

With this second dex, we are allowed to exchange more than the two initial tokens that come with the initialization of the level. Knowing this, we can create our own ERC20 token, and take advantage of the way the swap function works to exchange a few of our tokens for a lot of the ones we want to get.

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol";

contract SAD is ERC20, ERC20Permit {
    constructor() ERC20("T", "TOKEN") ERC20Permit("TOKEN") {
        _mint(msg.sender, 1000000000000000000);
    }
}


```