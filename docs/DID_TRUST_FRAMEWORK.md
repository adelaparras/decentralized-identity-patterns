# Decentralized Identity & Trust Framework Specification

## W3C DID Method Implementations
- **`did:key`**: Ephemeral cryptographic key pairs for zero-knowledge proofs.
- **`did:substrate`**: On-chain DID document anchoring on Polkadot parachains.
- **`did:ion`**: Sidetree protocol on Bitcoin layer for immutable trust resolution.

## Verifiable Credential Lifecycle
1. **Issuer**: Generates Ed25519 / ECDSA secp256k1 signed credential claims.
2. **Holder**: Stores credentials in secure enclave / hardware vault.
3. **Verifier**: Validates signature, revocation status, and schema compliance.
