<pre>
  BIP: xx
  Layer: Applications
  Title: Frost Unique Key
  Author: Rsync25 & 22388o
  Comments-Summary: No comments yet.
  Comments-URI: https://github.com/bitcoin/bips/wiki/Comments:BIP-xx
  Status: Draft
  Type: Standards Track
  Created: 2024-01-01
  License: CC0-1.0
</pre>

## Abstract

This BIP proposes the integration of the FROST (Flexible Round-Optimized Schnorr Threshold) signature scheme into the Bitcoin protocol. FROST provides efficient and secure threshold signatures, leveraging a group G of prime order q with a hard Decisional Diffie-Hellman problem. The proposal assumes the existence of a generator g in G and employs a cryptographic hash function H mapping to Z∗q.

## Motivation

The motivation behind this proposal is to enhance the security and flexibility of signature schemes in Bitcoin. FROST introduces threshold signatures, allowing multiple parties to collaboratively create a signature while maintaining the security properties of Schnorr signatures.

### Real-World Problems Addressed
- **High Transaction Costs:** Traditional multisig setups require multiple public keys and signatures, increasing the on-chain footprint and fees.
- **Privacy Concerns:** Standard multisig exposes signer public keys on-chain, potentially revealing sensitive participant information.
- **Coordination Complexity:** Existing multisig systems struggle with dynamic participation and scalability.

### Comparative Efficiency
FROST provides:
1. **Lower On-Chain Costs:** A single aggregated signature reduces transaction size.
2. **Enhanced Privacy:** Public keys remain hidden, indistinguishable from single-signature outputs.
3. **Dynamic Flexibility:** Enables dynamic participant sets and adaptive quorum requirements.

## Specification

### Key Generation

**Inputs:**
  - G: Group of prime order q with a hard Decisional Diffie-Hellman problem.
  - g: Generator of G.
  - H: Cryptographic hash function mapping to Z∗q.

**Outputs:**
  - Private key: x $\leftarrow S (uniformly randomly selected from S).
  - Public key: Y = g^x.

### Signature Generation

**Inputs:**
  - Message m.
  - Private key x.
  - Group G, generator g, and hash function H.

**Outputs:**
  - Schnorr signature (s, R).

**Procedure:**
  1. Compute k $\leftarrow S.
  2. Compute R = g^k.
  3. Compute e = H(R || Y || m).
  4. Compute s = k + ex (mod q).

### Signature Verification

**Inputs:**
  - Signature (s, R).
  - Public key Y.
  - Message m.
  - Group G, generator g, and hash function H.

**Procedure:**
  1. Compute e = H(R || Y || m).
  2. Verify if g^s = R * Y^e (mod q).

## Compatibility

FROST seamlessly integrates with Taproot and the Schnorr signature scheme, ensuring backward compatibility post-Taproot upgrade. Transactions generated via FROST are indistinguishable from regular single-signature outputs. Wallet software, nodes, and libraries such as `bitcoinjs-lib` and `libsecp256k1` will require updates to include FROST primitives.

## Security Considerations

1. **Nonce Management:** Ensure proper handling of nonces to prevent leakage of private keys due to nonce reuse.
2. **Replay Attacks:** Protect against replay attacks by incorporating unique session identifiers during signing.
3. **Audit and Validation:** Rigorous cryptographic audits and comprehensive testing must precede adoption to identify vulnerabilities.

## Examples

### Example: Multi-Signature Wallet

1. A custodial wallet with 5 participants sets up a 3-of-5 FROST threshold signature.
2. Participants collaboratively generate a signature for a transaction.
3. The resulting single Schnorr signature is broadcast to the Bitcoin network, saving costs and preserving privacy.

### Why FROST?
1. **Efficiency:** FROST reduces the computational overhead of multisig transactions while minimizing coordination rounds.
2. **Scalability:** Supports large, dynamic participant sets with minimal interactive steps.
3. **Privacy:** Hides participant identities through aggregated signatures.
4. **Compatibility:** Directly builds upon Schnorr, leveraging existing infrastructure.

## Backward Compatibility

This proposal is backward-compatible with Bitcoin Core versions supporting Taproot. It does not require a hard fork and can be implemented within existing soft-fork upgrades.

## Reference Implementation

A reference implementation in `libsecp256k1` will include:
- Efficient key generation.
- Distributed signing protocols.
- Single-output Schnorr signature validation.

## Acknowledgments

Thanks to the Bitcoin development community and contributors to threshold signature research, including Dr. M. Back for the foundational work on FROST.

## References

1. Back, M., "FROST: Flexible Round-Optimized Schnorr Threshold Signatures."
2. Bitcoin Taproot Upgrade Documentation.
3. Schnorr Signature Overview by Bitcoin Core Developers.

## Copyright

This document is licensed under the Creative Commons CC0 1.0 Universal license.


