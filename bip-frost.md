<pre>
  BIP: xx
  Layer: Applications
  Title: Frost 
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

## Specification

Key Generation

    Inputs:
        G: Group of prime order q with a hard Decisional Diffie-Hellman problem.
        g: Generator of G.
        H: Cryptographic hash function mapping to Z∗q.

    Outputs:
        Private key: x $← S (uniformly randomly selected from S).
        Public key: Y = g^x.

Signature Generation

    Inputs:
        Message m.
        Private key x.
        Group G, generator g, and hash function H.

    Outputs:
        Schnorr signature (s, R).

    Procedure:
        Compute k $← S.
        Compute R = g^k.
        Compute e = H(R || Y || m).
        Compute s = k + ex (mod q).

Signature Verification

    Inputs:
        Signature (s, R).
        Public key Y.
        Message m.
        Group G, generator g, and hash function H.

    Procedure:
        Compute e = H(R || Y || m).
        Verify if g^s = R * Y^e (mod q).

### 4. Compatibility

Discuss how Frost Multisig interacts with existing Bitcoin wallet software, network nodes, and other relevant components.

### 5. Security Considerations

Highlight the security features of Frost Multisig and any potential vulnerabilities that users should be aware of.

### 6. Examples


## Rationale

Explain the rationale behind choosing FROST for threshold signatures in Bitcoin, discussing its efficiency, security properties, and potential benefits.

## Backward Compatibility

This is compatible after Taproot soft fork and Bitcoin Core versions

## Reference Implementation

M. Back, "FROST: Flexible Round-Optimized Schnorr Threshold Signatures."

## Acknowledgments

Give credit to contributors, developers, or organizations that have played a significant role in the development of Frost Multisig.

## References

List any relevant literature, resources, or external documentation related to Frost Multisig.

## Copyright

This document is licensed under the Creative Commons CC0 1.0 Universal license.
