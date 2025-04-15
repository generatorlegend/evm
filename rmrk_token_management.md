# RMRK Token Management

## Introduction

This guide covers the management of RMRK tokens, including minting, burning, transferring, and interacting with various RMRK features. RMRK tokens are built on top of the ERC721 standard and introduce additional functionality such as nesting, equipping, and multi-asset support.

## Minting Tokens

### Standard Minting

To mint a new RMRK token, you can use the `_mint` function:

```solidity
function _mint(address to, uint256 tokenId, bytes memory data) internal virtual
```

Example usage:

```solidity
_mint(recipient, newTokenId, "");
```

### Nested Minting

RMRK supports minting tokens directly into other tokens using the `_nestMint` function:

```solidity
function _nestMint(
    address to,
    uint256 tokenId,
    uint256 destinationId,
    bytes memory data
) internal virtual
```

Example usage:

```solidity
_nestMint(parentContract, newTokenId, parentTokenId, "");
```

## Burning Tokens

To burn an RMRK token, use the `burn` function:

```solidity
function burn(uint256 tokenId, uint256 maxChildrenBurns) public virtual
```

This function allows for recursive burning of child tokens. The `maxChildrenBurns` parameter limits the number of child tokens that can be burned to prevent excessive gas consumption.

Example usage:

```solidity
burn(tokenId, 5); // Burn the token and up to 5 levels of nested children
```

## Transferring Tokens

### Standard Transfer

Use the `transferFrom` function to transfer tokens:

```solidity
function transferFrom(address from, address to, uint256 tokenId) public virtual
```

Example usage:

```solidity
transferFrom(currentOwner, newOwner, tokenId);
```

### Nested Transfer

To transfer a token into another token, use the `nestTransferFrom` function:

```solidity
function nestTransferFrom(
    address from,
    address to,
    uint256 tokenId,
    uint256 destinationId,
    bytes memory data
) public virtual
```

Example usage:

```solidity
nestTransferFrom(currentOwner, parentContract, tokenId, parentTokenId, "");
```

## Managing Child Tokens

### Adding a Child Token

To add a child token to a parent token:

```solidity
function addChild(
    uint256 parentId,
    uint256 childId,
    bytes memory data
) public virtual
```

This function is called by the child token's contract to propose itself as a child of the parent token.

### Accepting a Child Token

To accept a proposed child token:

```solidity
function acceptChild(
    uint256 parentId,
    uint256 childIndex,
    address childAddress,
    uint256 childId
) public virtual
```

Example usage:

```solidity
acceptChild(parentTokenId, 0, childContractAddress, childTokenId);
```

### Rejecting Child Tokens

To reject all pending child tokens:

```solidity
function rejectAllChildren(uint256 tokenId, uint256 maxRejections) public virtual
```

Example usage:

```solidity
rejectAllChildren(parentTokenId, 10); // Reject up to 10 pending children
```

### Transferring Child Tokens

To transfer a child token from one parent to another:

```solidity
function transferChild(
    uint256 tokenId,
    address to,
    uint256 destinationId,
    uint256 childIndex,
    address childAddress,
    uint256 childId,
    bool isPending,
    bytes memory data
) public virtual
```

Example usage:

```solidity
transferChild(
    parentTokenId,
    newParentContract,
    newParentTokenId,
    0,
    childContractAddress,
    childTokenId,
    false,
    ""
);
```

## Best Practices

1. Always check token ownership and approval before performing operations.
2. Use `safeTransferFrom` instead of `transferFrom` when transferring to contracts to ensure they can handle ERC721 tokens.
3. When burning tokens, consider the implications for nested child tokens and use an appropriate `maxChildrenBurns` value.
4. Implement access control for sensitive operations like minting and burning.
5. Use events to track important state changes and make it easier for off-chain services to monitor your contract.

## Conclusion

RMRK token management introduces powerful features for creating complex token relationships and interactions. By understanding and correctly implementing these functions, you can create rich, nested token ecosystems with multiple assets and equippable items.