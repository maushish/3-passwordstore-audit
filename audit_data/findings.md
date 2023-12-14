### [S-#] Storing the password on-chain makes it visible to everyone and no longer to private.

**Description:** All data stored on blockchain and can be read directly from the blockchain. The `PasswordStore::s_password` variable is intended to be private variable and only accessd through the `PasswordStore::getPassword` function which is intended to be only called by the owner of the contract.

We show one such method of reading any data below:

**Impact:** Anyone can read the password which is supposed to be private, severly breaking the functionality of the protocol/

**Proof of Concept:**(Proof of Code)
The Below test case shows how anyone can read the password directly from the blockchain.

1. Create a local running chain
```bash
make anvil
```
2. Deploy the contract on chain
```
make deploy
```

3.Run the Storage Tool: We use `1` because that's the storage slot of `s_password` in the contract

```
cast storage <ADDRESS_HERE> 1 --rpc-url http://127.0.0.1:8545
```
you will get an output like this 
`0x6d7950617373776f726400000000000000000000000000000000000000000014`

You can parse that hex into a string with 
```
cast parse-bytes32-string 0x6d7950617373776f726400000000000000000000000000000000000000000014
```
You will get an output  of:
```
myPassword
```

**Recommended Mitigation:** Rethink the whole architecture