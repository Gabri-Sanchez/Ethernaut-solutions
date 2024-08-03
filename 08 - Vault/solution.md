# 08 - Vault

To do this level, we need to call the `unlock()` function by providing the correct value, which is stored in the private variable `password`. Although the variable is private, and its value can't be accessed through any method, the value is still stored onchain, so there is still a way to get the value. We can use `getStorageAt()` to retrieve the value of the `password` variable. As it is the second variable of the contract, it will be int the slot 1, as variables are stored on each slot by the order of their declaration, which starts at 0.

```javascript
const password = await web3.eth.getStorageAt(contract.address, 1);

await contract.unlock();
```