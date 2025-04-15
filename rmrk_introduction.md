# Introduction to RMRK Standard

## Overview

The RMRK (pronounced "remark") standard is an innovative approach to NFT functionality, offering advanced features and flexibility beyond traditional NFT implementations. RMRK introduces a set of modular, composable, and extensible NFT legos that can be combined to create powerful and versatile digital assets.

## Core Concepts

RMRK is built on three main modules:

1. Equippable
2. MultiAsset
3. Nestable

These modules work together to provide a rich set of features for NFT creators and collectors.

### Equippable

The Equippable module allows NFTs to be equipped with other NFTs, creating a dynamic and interactive relationship between digital assets. This feature enables the creation of customizable avatars, upgradeable items, and complex game assets.

Key concepts:
- Equipment: NFTs that can be attached to other NFTs
- Slots: Predefined positions where equipment can be attached
- Catalogs: Collections of parts that can be used as equipment

### MultiAsset

The MultiAsset module enables a single NFT to have multiple visual representations or assets. This feature allows for greater flexibility in how NFTs are displayed and used across different platforms and contexts.

Key concepts:
- Active assets: The currently displayed or used assets of an NFT
- Pending assets: Assets that can be added to an NFT but require acceptance
- Asset priorities: The order in which assets are displayed or used

### Nestable

The Nestable module allows NFTs to own other NFTs, creating a hierarchical structure of digital assets. This feature enables the creation of complex, composable NFTs and opens up new possibilities for ownership and management of digital assets.

Key concepts:
- Parent NFTs: NFTs that can own other NFTs
- Child NFTs: NFTs that can be owned by other NFTs
- Nested ownership: The ability to create hierarchies of NFTs

## Benefits and Use Cases

The RMRK standard offers several advantages over traditional NFT implementations:

1. Increased flexibility: Create more complex and dynamic NFTs that can adapt to different use cases.
2. Enhanced interoperability: RMRK NFTs can interact with each other across different collections and platforms.
3. Improved resource efficiency: Reduce the number of smart contracts needed for complex NFT systems.
4. Greater customization: Allow users to personalize their NFTs through equipping and nesting.

Use cases for RMRK NFTs include:

- Gaming: Create complex in-game items, characters, and inventories.
- Digital art: Develop interactive and evolving artworks.
- Virtual real estate: Build hierarchical property ownership systems.
- Collectibles: Design more engaging and dynamic collectible experiences.
- Identity and credentials: Create composite digital identities with nested achievements and certifications.

## Getting Started

To start working with RMRK NFTs, you'll need to familiarize yourself with the core interfaces:

- IERC6220: The Equippable interface
- IERC5773: The MultiAsset interface
- IERC7401: The Nestable interface

These interfaces define the key functions and events for each module, allowing you to interact with RMRK NFTs in your smart contracts and applications.

For more detailed information on each module and how to implement RMRK NFTs, please refer to the respective documentation pages.