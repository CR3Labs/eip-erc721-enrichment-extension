---
eip: TBD
title: A new token standard which represents an enrichment of an existing NFT
author: Mike Roth (@mikehroth)
discussions-to: https://github.com/ethereum/EIPs/issues/
status: Draft
type: Standards Track
category: ERC
created: 2021-06-24
requires: 721
---

## Simple Summary
This standard defines a derivative of an ERC-721 token called a non-fungible enrichment which adds additional functionality to an existing NFT.

## Abstract
The ERC-721 token contract defines a way to create a non-fungible token but as a non-fungible token is generally designed to be immutable it does not provide for a method of managing external or mutable data which relates to the original NFT. This new token standard would enable a token which is "owned" by an existing NFT and may be purchased only by the owner of the underlying NFT to enrich the underlying NFT.

## Motivation
As NFTs become more sophisticated and start to represent complex assets it will be important to enhance existing NFTs in a manner which does not require the original immutable NFT to be directly modified. ...

## Specification
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

...TBD

- Tokens are not transferable
- Owner is a reference to another NFT (721 or 1155)
- Tokens may be purchased
- Tokens may be burned by the `owner` of the `ownerToken` (Underlying NFT)
- Metadata should be implemented using a mutable datastore which can be managed only by the `owner` of the `ownerToken` (Underlying NFT)
  - *See ceramic use case
- ...

Implementers of this standard MUST have all of the following functions:

```
pragma solidity ^0.8.0;

interface EnrichmentNFTInterface {

  event EnrichmentAsset(...);

  // ....

}

```

## Rationale

TBD


## Implementation
```
pragma solidity ^0.8.0;

import "./EnrichmentNFTInterface.sol";

abstract contract EnrichmentNFT, EnrichmentNFTInterface{
  using SafeERC20 for IERC20;

  function supportsInterface(bytes4 interfaceId) public view virtual override(ERC721, ERC1155Receiver) returns (bool) {
        return ERC721.supportsInterface(interfaceId) || ERC1155Receiver.supportsInterface(interfaceId);
  }

  // TBD

}
```

## Security Considerations


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
