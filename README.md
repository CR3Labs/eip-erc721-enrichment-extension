---
eip: XXXX
title: ERC-721 Enrichment Extension
description: A standard interface for bindable non-fungible tokens (token enrichments).
author: Mike Roth (@mikehroth)
discussions-to: 
status: Draft
type: Standards Track
category: ERC
created: 2021-07-01
requires: 165, 721
---

## Abstract

Proposes a standard API for Non-Fungible Enrichments (NFE) within smart contracts. An NFE is a non-fungible token which can be bound to another token and cannot be transferred once bound. This EIP defines basic functionality to mint, bind, and unbind NFEs.

## Motivation

As NFTs become more sophisticated and start to represent complex assets it will be important to enhance existing NFTs in a manner which does not require the original immutable NFT to be directly modified. Unlike composable NFTs (ERC-998) this standard
is designed to be lighter-weight and only enable the concept of "binding" information to existing NFTs.

The purpose of this document is to make NFEs a reality on Ethereum by creating consensus around a **maximally backward-compatible** but otherwise **minimal** interface definition.

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

`EIP-XXXX` tokens _must_ implement the interfaces:

- [EIP-165](./eip-165.md)'s `ERC165` (`0x01ffc9a7`)
- [EIP-721](./eip-721.md)'s `ERC721` (`0x80ac58cd`)
- [EIP-721](./eip-721.md)'s `ERC721Metadata` (`0x5b5e139f`)
- [EIP-721](./eip-721.md)'s `ERC721TokenReceiver` (`0x150B7A02`)

```solidity
// SPDX-License-Identifier: CC0-1.0
pragma solidity ^0.8.4;

/// @title Non-Fungible Enrichment
/// @dev See https://eips.ethereum.org/EIPS/eip-XXXX
/// Note: the ERC-165 identifier for this interface is 0xXXXXXXXX.
interface IERCXXXX /* is ERC165, ERC721Metadata, ERC721TokenReceiver */ {
  /// @dev This emits when an NFE is bound to an account by
  /// any mechanism.
  /// Note: For a reliable `_from` parameter, retrieve the transaction's
  /// authenticated `from` field.
  event Bound(
    address indexed contractAddress,
    uint256 indexed tokenId,
    uint256 enrichmentId,
    string uri
  );

  /// @dev This emits when an enrichment is unbound from an NFT by
  /// any mechanism.
  /// Note: For a reliable `_from` parameter, retrieve the transaction's
  /// authenticated `from` field.
  event Unbound(
    address indexed contractAddress,
    uint256 indexed tokenId,
    uint256 enrichmentId
  );

  /// @dev This function binds an enrichment to a token, transfers the enrichment 
  /// to the contract, sets the enrichment URI for this binding, and increments the 
  /// tokens enrichment balance.
  ///
  /// Requirements:
  /// - msg.sender must have a positive balance of the enrichment
  /// - msg.sender must own the token receiving the enrichment
  function bind(
      address contractAddress,
      uint256 tokenId,
      uint256 enrichmentId,
      string memory uri
  ) public virtual

  /// @dev Unbind an enrichment from a token. Transfers enrichment back to NFT owner, 
  /// unsets enrichment URI and sets the bound flag to false.
  ///
  ///  Requirements:
  ///  - enrichment must be bound
  ///  - enrichment can not be flagged as permanent
  ///  - sender must own the token with the bound enrichment
  function unbind(
    address contractAddress,
    uint256 tokenId,
    uint256 enrichmentId
  ) public virtual

  /// @dev This function is used to set a map of functions which are used to
  /// validate if an account is the owner of the NFT receiving the enrichment.
  ///
  /// Note: This function is only required if ownership is required to add an
  /// enrichment to an NFT.
  function setOwnerOfFunction(
    address contractAddress,
    string memory ownerOfFunc
  ) public virtual onlyOwner

  /// @dev Check if an NFT is owned by msg.sender. Security vulnerabilities of this 
  /// function are mitigated by using a `staticcall` to the external token contract.
  ///
  /// Note: this function makes use of the `ownerOf` function map created using 
  /// the `setOwnerOfFunction`.
  ///
  /// Requirements:
  /// - `contractAddress` cannot be a zero address.
  /// - must be a successful staticcall
  function ownsToken(
      address contractAddress,
      uint256 tokenId,
      address owner
  ) public view virtual returns

  /// @dev Return a bound enrichments URI
  function enrichmentURI(
    address contractAddress,
    uint256 tokenId,
    uint256 enrichmentId
  ) public view virtual returns (string memory)

  /// @dev Return whether or not an enrichment is bound to a token
  function isBound(
    address contractAddress,
    uint256 tokenId,
    uint256 enrichmentId
  ) public view virtual returns (bool)

  /// @dev Return the enrichment permanent flag
  function isPermanent(
    uint256 enrichmentId
  ) public view virtual returns (bool)
}
```

See [`EIP-721`](./eip-721.md) for a definition of its metadata JSON Schema.

## Rationale

### Interface

`EIP-XXXX` shall be maximally backward-compatible but still only expose a minimal and simple to implement interface definition.

As [`EIP-721`](./eip-721.md) tokens have seen widespread adoption with wallet providers and marketplaces, using its  interfaces with [`EIP-165`](./eip-165.md) for feature-detection potentially allows implementers to support `EIP-XXXX` tokens out of the box.

### Provenance Indexing

NFEs can be indexed by tracking the emission of `event Bound` and `event Unbound`. To guarantee reliable and implementation-independent indexable information, neither `event Bound` nor `event Unbound` include a `from` argument to depict the transaction sender. Instead, as a `from` property wouldn't be authenticated and hence opens a security vector, we omit it and advise indexers to substitute it with the transaction-level `from` field which gets authenticated through Ethereum's transaction signature validation prior to inclusion in a block.

## Backwards Compatibility

We have adopted the [`EIP-165`](./eip-165.md) and `ERC721Metadata` functions purposefully to create a high degree of backward compatibility with [`EIP-721`](./eip-721.md). We have deliberately used [`EIP-721`](./eip-721.md) terminology such as `function ownerOf(...)` to minimize the effort of familiarization for `EIP-XXXX` implementers already familiar with, e.g., [`EIP-20`](./eip-20.md) or [`EIP-721`](./eip-721.md).

## Reference Implementation

You can find an implementation of this standard in [../assets/eip-XXXX](../assets/eip-XXXX/src/ERCXXXX.sol).

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).