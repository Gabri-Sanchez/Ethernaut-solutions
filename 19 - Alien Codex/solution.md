# 19 - Alien Codex

The array codex (whose length is stored in slot 0) is a dynamic array. Its values are stored in the slot that results of "sha3(the number of the slot) + index", so we will use this to our advantage. As owner is stored in slot 0, and we can make a write on codex affect slot 0: we have to make sha3(1) + index equal to 0. We'll get this by making the sum result to cause an overflow that ends up returning 0. But first, we are going to see what we need before.

We'll have to make the length of codex big enough for us to get to the entry that causes the overflow, so that the rewriting of the owner can be done without issues. We'll do it just by calling retract function, which can cause an underflow on codex's length, giving us the perfect situation to now rewrite the owner of the contract.

Which entry do we need to rewrite? Let's see

We want the result of sha3(1) + index = 0. With a bit of math, we get that index = 0 - sha3(1). Now, we just need to do the calculations in uint256 numbers, and we'll get that the index is 35707666377435648211887908874984608119992236509074197713628505308453184860938 (The result should be this)

This number, coupled with your address encoded as a 32 byte value, will give you the ownership of the contract ;)