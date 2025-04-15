# Nestable Tokens Guide

## Introduction

Nestable tokens, implemented through the RMRK standard (ERC-7401), allow for the creation of parent-child relationships between tokens. This guide will explain how to work with nestable tokens, manage nested token structures, and leverage this feature for complex token hierarchies.

## Understanding Nestable Tokens

Nestable tokens extend the functionality of standard ERC-721 tokens by allowing tokens to own other tokens. This creates a hierarchical structure where:

- A token can be a parent to multiple child tokens
- A token can be a child of another token
- Tokens can be transferred between parents or to standalone ownership

## Key Concepts

### Direct Ownership

The `DirectOwner` struct represents the immediate owner of a token:

```solidity
struct DirectOwner {
    uint256 tokenId;
    address ownerAddress;
}
```

- If `tokenId` is 0, the owner is an externally owned account (EOA)
- If `tokenId` is non-zero, the owner is another token

### Child Management

Child tokens are managed using two arrays:

1. Active children: Accepted child tokens
2. Pending children: Proposed child tokens awaiting acceptance

Child tokens are represented by the `Child` struct:

```solidity
struct Child {
    uint256 tokenId;
    address contractAddress;
}
```

## Working with Nestable Tokens

### Creating Parent-Child Relationships

To establish a parent-child relationship:

1. Use the `addChild` function to propose a child token:

```solidity
function addChild(uint256 parentId, uint256 childId, bytes memory data) external;
```

2. Accept the proposed child using `acceptChild`:

```solidity
function acceptChild(uint256 parentId, uint256 childIndex, address childAddress, uint256 childId) external;
```

### Managing Nested Tokens

#### Transferring Child Tokens

To transfer a child token:

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
) external;
```

This function allows you to transfer a child token to another parent token or to standalone ownership.

#### Rejecting Pending Children

To reject all pending children of a token:

```solidity
function rejectAllChildren(uint256 tokenId, uint256 maxRejections) external;
```

#### Burning Nested Tokens

When burning a parent token, all its child tokens are recursively burned:

```solidity
function burn(uint256 tokenId, uint256 maxRecursiveBurns) external returns (uint256 burnedChildren);
```

### Querying Nested Token Information

- Get active children: `childrenOf(uint256 parentId)`
- Get pending children: `pendingChildrenOf(uint256 parentId)`
- Get a specific active child: `childOf(uint256 parentId, uint256 index)`
- Get a specific pending child: `pendingChildOf(uint256 parentId, uint256 index)`

### Ownership and Approval

- Check token ownership: `ownerOf(uint256 tokenId)`
- Get direct owner (immediate parent or EOA): `directOwnerOf(uint256 tokenId)`
- Approve token management: `approve(address to, uint256 tokenId)`
- Check if approved: `getApproved(uint256 tokenId)`

## Leveraging Nestable Tokens for Complex Structures

Nestable tokens enable the creation of complex token hierarchies, useful for various applications:

1. Composable NFTs: Create NFTs with modular components
2. Game item inventories: Represent in-game inventories and item ownership
3. Organizational structures: Model hierarchical relationships in DAOs or other organizations
4. Real estate: Represent property ownership with nested sub-properties

## Best Practices

1. Always check ownership and approval before performing operations on tokens
2. Use `safeTransferFrom` instead of `transferFrom` when transferring to smart contracts
3. Implement proper access control for sensitive operations
4. Be mindful of gas costs when working with deeply nested structures
5. Use events to track important state changes in your contract

## Conclusion

Nestable tokens provide a powerful way to create complex token relationships and hierarchies. By understanding the core concepts and utilizing the provided functions, you can leverage this feature to build sophisticated token-based applications.