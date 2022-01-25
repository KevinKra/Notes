# Solidity

```
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

The first line tells you that the source code is licensed under the GPL version 3.0. **Machine-readable license specifiers are important in a setting where publishing the source code is the default.**

**Pragmas are common instructions for compilers about how to treat the source code.**

A contract in the sense of Solidity is a **collection of code (its functions) and data (its state) that resides at a specific address on the Ethereum blockchain.**

`uint` (unsigned integer of 256 bits). You can think of it as a single slot in a database that you can query and alter by calling functions of the code that manages the database.

To access a member (like a state variable) of the current contract, you do not typically add the `this.` prefix, **you just access it directly via its name.**

> Be careful with using Unicode text, as similar looking (or even identical) characters can have different code points and as such are encoded as a different byte array.

## Subcurrency

```
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

contract Coin {
    // The keyword "public" makes variables
    // accessible from other contracts
    address public minter;
    mapping (address => uint) public balances;

    // Events allow clients to react to specific
    // contract changes you declare
    event Sent(address from, address to, uint amount);

    // Constructor code is only run when the contract
    // is created
    constructor() {
        minter = msg.sender;
    }

    // Sends an amount of newly created coins to an address
    // Can only be called by the contract creator
    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        balances[receiver] += amount;
    }

    // Errors allow you to provide information about
    // why an operation failed. They are returned
    // to the caller of the function.
    error InsufficientBalance(uint requested, uint available);

    // Sends an amount of existing coins
    // from any caller to an address
    function send(address receiver, uint amount) public {
        if (amount > balances[msg.sender])
            revert InsufficientBalance({
                requested: amount,
                available: balances[msg.sender]
            });

        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```

The line `address public minter`; declares a state variable of type `address`. **The `address` type is a 160-bit value that does _not_ allow any arithmetic operations.** It is suitable for storing addresses of contracts, or a hash of the public half of a keypair belonging to external accounts.

The keyword `public` automatically generates a function that allows you to access the current value of the state variable from _outside_ of the contract. Without this keyword, other contracts have no way to access the variable.

The next line, `mapping (address => uint) public balances;` also creates a public state variable, but it is a more complex datatype. **The mapping type maps addresses to unsigned integers.**

#### Mapping

**Mappings can be seen as hash tables which are virtually initialised such that every possible key exists from the start** and is mapped to a value whose byte-representation is all zeros. However, **it is neither possible to obtain a list of all keys of a mapping, nor a list of all values.** Record what you added to the mapping, or use it in a context where this is not needed. Or even better, keep a list, or use a more suitable data type.

The line event `Sent(address from, address to, uint amount);` declares an **“event”**, which is emitted in the last line of the function `send`. **Ethereum clients such as web applications can listen for these events emitted on the blockchain without much cost.** As soon as it is emitted, the listener receives the arguments `from`, `to` and `amount`, which makes it possible to track transactions.

The constructor is a special function that **is executed during the creation of the contract and cannot be called afterwards.**

```
// Constructor code is only run when the contract
// is created
constructor() {
    minter = msg.sender;
}
```

**The `msg` variable (together with `tx` and `block`) is a special global variable that contains properties which allow access to the blockchain.**

**`msg.sender` is always the address where the current (external) function call came from.**

### Mint and Send

> The functions that make up the contract, and that users and contracts can call are `mint` and `send`.

```
// Sends an amount of newly created coins to an address
// Can only be called by the contract creator
function mint(address receiver, uint amount) public {
    require(msg.sender == minter);
    balances[receiver] += amount;
}
```

**The `mint` function sends an amount of newly created coins to another address.** The `require` function call defines conditions that reverts all changes if not met. In this example, `require(msg.sender == minter);` ensures that only the creator of the contract can call mint.

- In general, the creator can mint as many tokens as they like, but at some point, this will lead to a phenomenon called “overflow”.

  - Note that because of the default Checked arithmetic, the transaction would revert if the expression `balances[receiver] += amount`; overflows, i.e., when `balances[receiver] + amount` in arbitrary precision arithmetic is larger than the maximum value of `uint (2\*\*256 - 1)`.

- Errors are used together with the **`revert` statement**.

```
if (amount > balances[msg.sender])
    revert InsufficientBalance({
        requested: amount,
        available: balances[msg.sender]
    });
...
```

- The **`revert` statement** unconditionally aborts and reverts all changes similar to the require function, but **it also allows you to provide the name of an error and additional data which will be supplied to the caller (and eventually to the front-end application or block explorer) so that a failure can more easily be debugged or reacted upon.**

#### Note

> If you use this contract to send coins to an address, you _will not_ see anything when you look at that address on a blockchain explorer, because the record that you sent coins and the changed balances are only stored in the data storage of this particular coin contract.

> By using events, you can create a “blockchain explorer” that tracks transactions and balances of your new coin, but you have to inspect the coin contract address and not the addresses of the coin owners.

## BlockChain Basics

> **A blockchain is a globally shared, transactional database.**

Everyone can read entries in the database just by participating in the network. If you want to change something in the database, you have to create a so-called transaction which has to be accepted by all others. The word transaction implies that the change you want to make (assume you want to change two values at the same time) is **either not done at all or completely applied.** Furthermore, **while your transaction is being applied to the database, no other transaction can alter it.**

- a transaction is always **cryptographically signed by the sender (creator).** This makes it straightforward to guard access to specific modifications of the database. In the example of the electronic currency, a simple check ensures that only the person holding the keys to the account can transfer money from it.

### Blocks

One major obstacle to overcome is what (in Bitcoin terms) is called a **“_double-spend attack_”: What happens if two transactions exist in the network that both want to empty an account? Only one of the transactions can be valid, typically the one that is accepted first. The problem is that “first” is not an objective term in a peer-to-peer network.**

The abstract answer to this is that you do not have to care. A globally accepted order of the transactions will be selected for you, solving the conflict. **The transactions will be bundled into what is called a “block” and then they will be executed and distributed among all participating nodes. If two transactions contradict each other, the one that ends up being second will be rejected and not become part of the block.**

**These blocks form a linear sequence in time and that is where the word “blockchain” derives from.** Blocks are added to the chain in rather regular intervals - _for Ethereum this is roughly every 17 seconds._

As part of the **“order selection mechanism” (which is called “mining”)** it may happen that blocks are reverted from time to time, but only at the “tip” of the chain.

**The more blocks are added on top of a particular block, the less likely this block will be reverted.** So it might be that your transactions are reverted and even removed from the blockchain, but the longer you wait, the less likely it will be.

## Questions

- The first line of a script in Solidity requires what?
- What is a **pragma**?
- What is a contract in Solidity?
- What is a `uint`?
- If you want to access a member of a current contract, do you need to use the `this.` prefix?
- What bit value is the `address` type?
- Does the `address` type allow any arithmetic operations?
- What does the `public` keyword allow?
- Would other contracts be able to access the state variable of a contract without the `public` keyword in `address public minter`?
- In this code: `mapping (address => uint) public balances;`, what does the `mapping` type map?
- Can a Mapping be seen a hash table?
- Is every possible key that can exist in a map initialised at the start?
- Is it possible to obtain a list of all keys of a mapping?
- Is it possible to obtain a list of all values of a mapping?
- What listens for "events" that are emitted from functions?
- The `constructor` is a special function that is executed at what point during a contract's lifecycle?
- Can a `constructor` be called again after a contract has been created?
- is `msg` a special global variable?
- `msg.sender` is always what?
- What does the `mint` function do?
- What happens if the `require` function called conditions are not met?
- A creator can mint as many tokens as they like, but eventually they may experience overflow. **What is overflow?**
- What does the `revert` statement does what, just like the `require` statement?
- What does the `revert` statement do that the `require` statement doesn't?
- What is a blockchain?
- A transaction always has what from the creator of the transaction?
- What is a **double-spend attack**?
- A block is a bundling of what?
- A block is distributed to what?
- If two transactions contradict each other, will the first or second transaction become part of the block?
- Blocks form a linear sequence in time, what is this called?
- Blocks are added at regular intervals, what is the _rough_ interval for Ethereum?
- The **order selection mechanism** is commonly called what?
- Can blocks be reverted occasionally? At what part of the chain is this possible?
- The more blocks added on top of a particular block reduce the chances of what?
