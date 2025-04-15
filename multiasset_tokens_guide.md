# Multi-Asset Tokens Guide

## Introduction

Multi-asset tokens are a powerful feature of the RMRK standard that allows a single token to have multiple assets associated with it. This guide will help you understand how to work with multi-asset tokens, including creating and managing multiple assets for a single token, and how to leverage this feature in various use cases.

## Understanding Multi-Asset Tokens

In traditional NFT standards, each token represents a single asset. However, with RMRK's multi-asset functionality, a single token can have multiple assets associated with it. This opens up new possibilities for creating more complex and dynamic NFTs.

Key concepts:
- A token can have multiple assets
- Assets can be added, removed, or prioritized
- Assets have their own metadata
- Users can accept or reject pending assets

## Implementing Multi-Asset Tokens

To implement multi-asset tokens, you'll need to use the `RMRKMultiAsset` contract. This contract extends the basic ERC721 functionality with multi-asset capabilities.

### Key Functions

1. `acceptAsset`: Allows the token owner to accept a pending asset.
2. `rejectAsset`: Allows the token owner to reject a pending asset.
3. `rejectAllAssets`: Rejects all pending assets for a token.
4. `setPriority`: Sets the priority order of active assets for a token.
5. `getActiveAssets`: Retrieves the IDs of active assets for a token.
6. `getPendingAssets`: Retrieves the IDs of pending assets for a token.

### Adding Assets to a Token

To add an asset to a token, you'll need to implement a function in your contract that calls the internal `_addAssetToToken` function. Here's an example:

```solidity
function addAssetToToken(uint256 tokenId, uint64 assetId) public onlyOwner {
    _addAssetToToken(tokenId, assetId, 0);
}
```

This function adds the asset with `assetId` to the token with `tokenId`. The third parameter (0 in this case) is the ID of the asset to replace, or 0 if it's not replacing any existing asset.

### Managing Assets

Token owners can manage their assets using the following functions:

```solidity
// Accept a pending asset
function acceptAsset(uint256 tokenId, uint256 index, uint64 assetId) public;

// Reject a pending asset
function rejectAsset(uint256 tokenId, uint256 index, uint64 assetId) public;

// Reject all pending assets
function rejectAllAssets(uint256 tokenId, uint256 maxRejections) public;

// Set priority of active assets
function setPriority(uint256 tokenId, uint64[] calldata priorities) public;
```

These functions can only be called by the token owner or an approved address.

## Use Cases

Multi-asset tokens can be used in various scenarios:

1. **Customizable avatars**: Each asset could represent a different part of an avatar (e.g., head, body, accessories).
2. **Evolving collectibles**: Assets can be added or changed over time to represent the evolution of a collectible.
3. **Multi-media NFTs**: Different assets could represent different forms of media (e.g., image, video, audio) associated with a single token.
4. **Gaming items**: A single game item token could have multiple assets representing different visual styles or power-ups.

## Best Practices

1. **Asset management**: Implement clear rules for when and how assets can be added, accepted, or rejected.
2. **Metadata handling**: Ensure that each asset has well-defined metadata to describe its properties and purpose.
3. **User experience**: Provide intuitive interfaces for users to manage and interact with multiple assets on a single token.
4. **Gas optimization**: Be mindful of gas costs when dealing with multiple assets, especially when setting priorities for a large number of assets.

## Conclusion

Multi-asset tokens provide a flexible and powerful way to create more complex and dynamic NFTs. By understanding how to implement and manage multi-asset tokens, you can create unique and engaging experiences for your users.

Remember to always test your implementations thoroughly and consider the implications of multi-asset functionality on your overall token ecosystem.