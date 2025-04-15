---
title: Getting Started with RMRK
description: A step-by-step guide to integrate RMRK modules into your projects
---

# Getting Started with RMRK

Welcome to the RMRK (Remark) ecosystem! This guide will walk you through the process of integrating RMRK modules into your projects, setting up your environment, and performing common operations like minting tokens and managing assets.

## Table of Contents

1. [Introduction to RMRK](#introduction-to-rmrk)
2. [Setting Up Your Environment](#setting-up-your-environment)
3. [Integrating RMRK Modules](#integrating-rmrk-modules)
4. [Minting Tokens](#minting-tokens)
5. [Managing Assets](#managing-assets)
6. [Advanced Features](#advanced-features)

## Introduction to RMRK

RMRK is a set of NFT standards that provide advanced functionality for non-fungible tokens. The RMRK ecosystem includes modules for multi-asset NFTs, nestable NFTs, and equippable NFTs. These modules allow for more complex and feature-rich NFT implementations compared to traditional ERC721 tokens.

## Setting Up Your Environment

Before you start integrating RMRK modules, make sure you have the following prerequisites:

1. Node.js (version 12 or higher)
2. npm or yarn package manager
3. Solidity development environment (e.g., Truffle, Hardhat)

To set up your project, create a new directory and initialize a new npm project:

```bash
mkdir my-rmrk-project
cd my-rmrk-project
npm init -y
```

Install the required dependencies:

```bash
npm install @openzeppelin/contracts
```

## Integrating RMRK Modules

RMRK provides several abstract contracts that you can use as a starting point for your implementation. Depending on your needs, you can choose from:

- `RMRKAbstractEquippable`
- `RMRKAbstractMultiAsset`
- `RMRKAbstractNestable`

To use these modules, create a new Solidity file for your contract and import the desired RMRK abstract contract:

```solidity
// SPDX-License-Identifier: Apache-2.0

pragma solidity ^0.8.21;

import "@openzeppelin/contracts/token/ERC721/extensions/IERC721Metadata.sol";
import "@openzeppelin/contracts/interfaces/IERC2981.sol";
import "@openzeppelin/contracts/utils/introspection/IERC165.sol";
import "./RMRKAbstractEquippable.sol";

contract MyRMRKToken is RMRKAbstractEquippable {
    constructor(
        string memory name,
        string memory symbol,
        string memory collectionMetadata,
        string memory tokenURI,
        uint256 maxSupply
    ) RMRKImplementationBase(name, symbol, collectionMetadata, tokenURI, maxSupply) {
        // Additional initialization if needed
    }

    // Implement required functions and add custom logic
}
```

## Minting Tokens

To mint tokens using your RMRK-based contract, you can use the `_mint` function provided by the OpenZeppelin ERC721 implementation. Here's an example of how to create a public minting function:

```solidity
function mint(address to) public {
    uint256 tokenId = _totalSupply + 1;
    _mint(to, tokenId);
}
```

## Managing Assets

RMRK allows you to add multiple assets to a single token. Here's how you can add assets to your tokens:

```solidity
function addAssetToToken(uint256 tokenId, uint64 assetId, uint64 replacesAssetWithId) public override onlyOwnerOrContributor {
    super.addAssetToToken(tokenId, assetId, replacesAssetWithId);
}

function addAssetEntry(string memory metadataURI) public override onlyOwnerOrContributor returns (uint256 assetId) {
    return super.addAssetEntry(metadataURI);
}
```

To use these functions, first add an asset entry, then add the asset to a specific token:

```solidity
uint256 assetId = addAssetEntry("ipfs://QmAssetMetadataURI");
addAssetToToken(tokenId, uint64(assetId), 0);
```

## Advanced Features

RMRK offers advanced features like equippable assets and nestable tokens. To use these features, refer to the specific abstract implementations:

- For equippable assets, use `RMRKAbstractEquippable` and implement functions like `addEquippableAssetEntry` and `setValidParentForEquippableGroup`.
- For nestable tokens, use `RMRKAbstractNestable` and implement nested token operations.

Here's an example of adding an equippable asset:

```solidity
function addEquippableAsset(
    uint64 equippableGroupId,
    address catalogAddress,
    string memory metadataURI,
    uint64[] memory partIds
) public onlyOwnerOrContributor returns (uint256) {
    return addEquippableAssetEntry(equippableGroupId, catalogAddress, metadataURI, partIds);
}
```

Remember to implement all necessary functions from the abstract contracts and add any custom logic required for your specific use case.

By following this guide, you should now have a basic understanding of how to integrate RMRK modules into your project, mint tokens, and manage assets. For more advanced usage and detailed information on specific modules, refer to the individual module documentation and the RMRK specification.