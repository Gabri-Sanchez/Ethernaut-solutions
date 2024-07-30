# Delegation
This contract takes advantage of the delegateCall function.
When it is not properly implemented, it can be a way for bad actors to execute code.

In this specific case, we  take advantage of `delegatecall` from the fallback() function of the contract Delegation. We manage to do that using the hash of the pwn() function as the transaction data. The Delegation contract will try to find a function that is not implemented by getting it from the Delegate contract, which will give us the ownership, and make uss pass the level.

For us to gain control, we just have to do this:

sendTransaction({
    to: contract.address,
    data: "0xdd365b8b"
})
