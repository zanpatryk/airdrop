## Foundry

**Foundry is a blazing fast, portable, and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

- **Forge**: Ethereum testing framework (like Truffle, Hardhat, and DappTools).
- **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions, and getting chain data.
- **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
- **Chisel**: Fast, utilitarian, and verbose Solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```

---

## Chapter: `MerkleAirdrop` Contract Explanation

This Solidity contract, `MerkleAirdrop`, is designed to facilitate a token airdrop based on a Merkle Tree. It utilizes several features from OpenZeppelin libraries, including `SafeERC20` for safe token transfers and `MerkleProof` for verifying proof of claims.

### Key Features:

- **Merkle Tree Verification**: The contract verifies if an airdrop recipient is eligible by checking their inclusion in a Merkle Tree. This approach ensures efficient verification of the recipients' claims while keeping the proof size manageable.

- **Signature Verification**: The contract also includes EIP-712 compliant signature verification to ensure that the claim was signed by the correct recipient.

- **Claim Tracking**: A mapping (`s_claimed`) is used to track which addresses have already claimed their tokens, preventing double claims.

### Contract Details:

1. **Storage Variables**:

   - `i_merkleRoot`: The root of the Merkle Tree, which serves as the basis for verifying claims.
   - `i_airdropToken`: The ERC20 token that will be airdropped.
   - `s_claimed`: A mapping that tracks whether an address has already claimed its tokens.

2. **Errors**:

   - `MerkleAirdrop__InvalidProof`: Thrown when the provided Merkle proof is invalid.
   - `MerkleAirdrop__AlreadyClaimed`: Thrown when a user attempts to claim tokens multiple times.
   - `MerkleAirdrop__InvalidSignature`: Thrown when the EIP-712 signature is invalid.

3. **Functions**:

   - `claim`: Allows users to claim their tokens by providing the necessary Merkle proof and EIP-712 signature. If the proof and signature are valid, the user is marked as having claimed, and tokens are transferred to their address.
   - `getDigest`: Generates the EIP-712 compliant digest used for signature verification.
   - `getMerkleRoot`: Returns the stored Merkle root.
   - `getAirdropToken`: Returns the token used for the airdrop.
   - `_isValidSignature`: Verifies that the provided signature corresponds to the user claiming the airdrop.

4. **Workflow**:
   - When a user claims their airdrop, they must provide:
     - Their address and the amount they are claiming.
     - A Merkle proof showing that they are in the Merkle Tree.
     - An EIP-712 signature proving they authorized the claim.
   - The contract verifies the proof using `MerkleProof.verify` and the signature using `_isValidSignature`. If everything is correct, the user is marked as having claimed their tokens, and the airdrop is transferred to them.
