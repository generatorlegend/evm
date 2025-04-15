# RMRK Utils Usage Guide

## Introduction

RMRK utility contracts provide powerful tools to enhance the functionality of RMRK implementations and improve user experience. This guide focuses on two main utility contracts: `RMRKCollectionUtils` and `RMRKEquipRenderUtils`.

## RMRKCollectionUtils

The `RMRKCollectionUtils` contract offers various utility functions for RMRK contracts, particularly for managing collection data and metadata.

### Key Functions

#### getCollectionData

Retrieves comprehensive data about a collection:

```solidity
function getCollectionData(address collection) public view returns (CollectionData memory data)
```

This function returns a `CollectionData` struct containing:

- totalSupply
- maxSupply
- royaltyPercentage
- royaltyRecipient
- owner
- name
- symbol
- collectionMetadata

#### getInterfaceSupport

Checks which interfaces a collection supports:

```solidity
function getInterfaceSupport(address collection) public view returns (
    bool supports721,
    bool supportsMultiAsset,
    bool supportsNesting,
    bool supportsEquippable,
    bool supportsSoulbound,
    bool supportsRoyalties
)
```

#### getPaginatedMintedIds

Retrieves a list of existing token IDs within a specified range:

```solidity
function getPaginatedMintedIds(
    address targetEquippable,
    uint256 pageStart,
    uint256 pageSize
) public view returns (uint256[] memory page)
```

#### refreshCollectionTokensMetadata and refreshTokenMetadata

Trigger events to refresh metadata for a range of tokens or a single token:

```solidity
function refreshCollectionTokensMetadata(
    address collectionAddress,
    uint256 fromTokenId,
    uint256 toTokenId
) public

function refreshTokenMetadata(
    address collectionAddress,
    uint256 tokenId
) public
```

## RMRKEquipRenderUtils

The `RMRKEquipRenderUtils` contract provides utility functions for composing and rendering equipped assets in RMRK implementations.

### Key Functions

#### getExtendedEquippableActiveAssets

Retrieves detailed information about active assets of a token:

```solidity
function getExtendedEquippableActiveAssets(
    address target,
    uint256 tokenId
) public view returns (ExtendedEquippableActiveAsset[] memory activeAssets)
```

#### getExtendedPendingAssets

Retrieves detailed information about pending assets of a token:

```solidity
function getExtendedPendingAssets(
    address target,
    uint256 tokenId
) public view returns (ExtendedPendingAsset[] memory pendingAssets)
```

#### composeEquippables

Composes equippable assets for a given token:

```solidity
function composeEquippables(
    address target,
    uint256 tokenId,
    uint64 assetId
) public view returns (
    string memory metadataURI,
    uint64 equippableGroupId,
    address catalogAddress,
    FixedPart[] memory fixedParts,
    EquippedSlotPart[] memory slotParts
)
```

#### getAllEquippableSlotsFromParent

Retrieves information about equippable slots for a child token:

```solidity
function getAllEquippableSlotsFromParent(
    address targetChild,
    uint256 childId,
    bool onlyEquipped
) public view returns (uint256 childIndex, EquippableData[] memory equippableData)
```

#### getChildrenWithTopMetadata

Retrieves information about child tokens with their top asset metadata:

```solidity
function getChildrenWithTopMetadata(
    address parentAddress,
    uint256 parentId
) public view returns (ChildWithTopAssetMetadata[] memory childrenWithMetadata)
```

## Usage Examples

Here are some examples of how to use these utility functions in your RMRK implementation:

1. Retrieving collection data:

```solidity
RMRKCollectionUtils utils = new RMRKCollectionUtils();
address collectionAddress = 0x...;
RMRKCollectionUtils.CollectionData memory data = utils.getCollectionData(collectionAddress);
console.log("Collection name:", data.name);
console.log("Total supply:", data.totalSupply);
```

2. Composing equippables for a token:

```solidity
RMRKEquipRenderUtils equipUtils = new RMRKEquipRenderUtils();
address tokenAddress = 0x...;
uint256 tokenId = 1;
uint64 assetId = 1;

(
    string memory metadataURI,
    uint64 equippableGroupId,
    address catalogAddress,
    RMRKEquipRenderUtils.FixedPart[] memory fixedParts,
    RMRKEquipRenderUtils.EquippedSlotPart[] memory slotParts
) = equipUtils.composeEquippables(tokenAddress, tokenId, assetId);

console.log("Metadata URI:", metadataURI);
console.log("Number of fixed parts:", fixedParts.length);
console.log("Number of equipped slot parts:", slotParts.length);
```

3. Refreshing token metadata:

```solidity
RMRKCollectionUtils utils = new RMRKCollectionUtils();
address collectionAddress = 0x...;
uint256 tokenId = 1;

utils.refreshTokenMetadata(collectionAddress, tokenId);
```

By leveraging these utility functions, developers can easily access and manage complex RMRK data structures, compose equippable assets, and handle metadata updates, ultimately creating more robust and user-friendly RMRK implementations.