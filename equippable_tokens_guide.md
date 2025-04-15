# Equippable Tokens Guide

## Introduction

Equippable tokens are an advanced feature of the RMRK NFT system that allows for dynamic and composable NFTs. This guide will explain how to work with equippable tokens, including creating equippable assets, managing equipment, and interacting with the equipping system.

## Creating Equippable Assets

To create an equippable asset, you need to use the `_addAssetEntry` function in the `RMRKEquippable` contract. This function allows you to define the properties of the asset, including its equippable group and associated catalog.

```solidity
function _addAssetEntry(
    uint64 id,
    uint64 equippableGroupId,
    address catalogAddress,
    string memory metadataURI,
    uint64[] memory partIds
) internal virtual
```

- `id`: Unique identifier for the asset
- `equippableGroupId`: ID of the equippable group this asset belongs to
- `catalogAddress`: Address of the catalog contract associated with this asset
- `metadataURI`: URI pointing to the asset's metadata
- `partIds`: Array of part IDs included in this asset

Example usage:

```solidity
_addAssetEntry(
    1, // Asset ID
    1, // Equippable Group ID
    address(0x123...), // Catalog Address
    "ipfs://...", // Metadata URI
    [1, 2, 3] // Part IDs
);
```

## Managing Equipment

### Equipping an Asset

To equip a child token into a parent token, use the `equip` function:

```solidity
function equip(IntakeEquip memory data) public
```

The `IntakeEquip` struct contains the following information:

```solidity
struct IntakeEquip {
    uint256 tokenId;
    uint256 childIndex;
    uint64 assetId;
    uint64 slotPartId;
    uint64 childAssetId;
}
```

Example usage:

```solidity
IntakeEquip memory equipData = IntakeEquip({
    tokenId: 1, // Parent token ID
    childIndex: 0, // Index of the child in the parent's active children array
    assetId: 1, // Parent asset ID
    slotPartId: 1, // Slot part ID in the catalog
    childAssetId: 1 // Child asset ID
});

equip(equipData);
```

### Unequipping an Asset

To unequip a child token from a parent token, use the `unequip` function:

```solidity
function unequip(
    uint256 tokenId,
    uint64 assetId,
    uint64 slotPartId
) public
```

Example usage:

```solidity
unequip(1, 1, 1); // Unequip from token ID 1, asset ID 1, slot part ID 1
```

## Checking Equipment Status

### Is Child Equipped

To check if a child token is equipped into a parent token, use the `isChildEquipped` function:

```solidity
function isChildEquipped(
    uint256 tokenId,
    address childAddress,
    uint256 childId
) public view returns (bool isEquipped)
```

Example usage:

```solidity
bool isEquipped = isChildEquipped(1, address(0x456...), 2);
```

### Get Equipment Information

To retrieve information about the equipment in a specific slot, use the `getEquipment` function:

```solidity
function getEquipment(
    uint256 tokenId,
    address targetCatalogAddress,
    uint64 slotPartId
) public view returns (Equipment memory equipment)
```

Example usage:

```solidity
Equipment memory equipment = getEquipment(1, address(0x123...), 1);
```

## Working with Catalogs

Catalogs are an essential part of the equippable system. They define the valid slots and parts that can be used for equipping assets.

### Checking Equippability

To verify if a token can be equipped with a specific asset into a slot, use the `canTokenBeEquippedWithAssetIntoSlot` function:

```solidity
function canTokenBeEquippedWithAssetIntoSlot(
    address parent,
    uint256 tokenId,
    uint64 assetId,
    uint64 slotId
) public view returns (bool canBeEquipped)
```

Example usage:

```solidity
bool canBeEquipped = canTokenBeEquippedWithAssetIntoSlot(
    address(0x789...),
    1,
    1,
    1
);
```

### Getting Asset and Equippable Data

To retrieve detailed information about an asset, including its equippable data, use the `getAssetAndEquippableData` function:

```solidity
function getAssetAndEquippableData(
    uint256 tokenId,
    uint64 assetId
) public view returns (
    string memory metadataURI,
    uint64 equippableGroupId,
    address catalogAddress,
    uint64[] memory partIds
)
```

Example usage:

```solidity
(
    string memory metadataURI,
    uint64 equippableGroupId,
    address catalogAddress,
    uint64[] memory partIds
) = getAssetAndEquippableData(1, 1);
```

## Best Practices

1. Always check if a token can be equipped before attempting to equip it.
2. Use appropriate access control mechanisms to ensure only authorized users can equip and unequip assets.
3. Keep your catalogs well-organized and updated to maintain a consistent equipping system.
4. Consider using events to track equipping and unequipping actions for better traceability.

By following this guide, you should now have a good understanding of how to work with equippable tokens in the RMRK system. Remember to refer to the specific contract implementations and interfaces (IERC6220, RMRKEquippable) for more detailed information on available functions and their exact behaviors.