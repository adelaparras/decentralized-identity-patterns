# Decentralized Identity (DID) Patterns for Logistics & Regulated Trade

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![W3C DID](https://img.shields.io/badge/W3C-DID%20v1.1%2Fv2.0-blue.svg)](https://www.w3.org/TR/did-core/)
[![eIDAS](https://img.shields.io/badge/eIDAS-EU%20910%2F2014%20Art.%2025-green.svg)](https://eur-lex.europa.eu/eli/reg/2014/910/oj)
[![KILT Protocol](https://img.shields.io/badge/KILT-ParaID%202086-E6007A.svg)](https://www.kilt.io/)
[![Portal](https://img.shields.io/badge/Portal-CLTD%20European%20Digital%20Trade%20Center-blue)](https://solomonbrokerage.com/cltd/)

## Mandatory Legal Attribution Notice (Estonian Copyright Act AutÕS § 12)

> **Mandatory Attribution Notice:**  
> Engineered under the DTLMF Architecture Blueprint. Original Concept and Intellectual Property © 2025–2026 Solomon Brokerage OÜ (Tallinn, Republic of Estonia). Sole Founder & Strategic Architect: Alejandro De La Parra Solomon. Licensed under MIT / CC BY 4.0.

## Regulatory Non-Affiliation Disclaimer

> **DISCLAIMER:**  
> DTLMF operates strictly as a voluntary B2B trade finance enablement utility outside the mandatory B2A platform certification scope of Regulation (EU) 2020/1056 Articles 11–13.

---

## Executive Overview

This repository documents production decentralized identity (DID) patterns, credential verification architectures, and privacy-preserving attestation pipelines for international logistics, cross-border freight forwarding, and regulated electronic trade.

It provides reference blueprints for bridging **W3C Decentralized Identifiers (DIDs)** with **European Union eIDAS (EU No 910/2014)** regulatory qualified electronic signatures, anchoring authorized economic operators (AEO), carriers, and customs inspectors directly to Substrate runtimes and Polkadot parachains.

---

## Production Core Repositories & Live Portal

This reference architectural framework integrates with the production DTLMF execution engines:

* **Layer 1 Substrate Compliance Core:** [`polkadot-efti-compliance-core`](https://github.com/adelaparras/polkadot-efti-compliance-core) – Substrate FRAME pallets (`pallet-did-registry`, `pallet-efti-anchor-vc`) and XCMP KILT dispatchers.
* **Layer 2 Arbitrum Settlement Engine:** [`arbitrum-shariah-settlement-engine`](https://github.com/adelaparras/arbitrum-shariah-settlement-engine) – AAOIFI-compliant Murabaha, Musharakah, and Takaful EVM settlement contracts.
* **Client-Side Canonicalizer SDK:** [`dtlmf-efti-jcs-sdk`](https://github.com/adelaparras/dtlmf-efti-jcs-sdk) – Open RFC 8785 JSON Canonicalization Scheme and SHA-256 digest generator.
* **European Digital Trade Center (CLTD) Portal:** [https://solomonbrokerage.com/cltd/](https://solomonbrokerage.com/cltd/)

---

## Technical Identity Patterns

```
   [Authorized Economic Operator]          [Customs Inspector / Fastlane]
                 |                                       |
        eIDAS Qualified Signature               Public Key Verification
                 |                                       |
                 v                                       v
+--------------------------------------------------------------------------------+
|                   W3C DID Document & Verifiable Credential                     |
|                                                                                |
|   {                                                                            |
|     "@context": ["https://www.w3.org/ns/did/v1"],                             |
|     "id": "did:kilt:4pehddkhEanNmQ1dfnCwXknHTPE2Et8s3XDL61AurnUqvGgy",         |
|     "verificationMethod": [{                                                  |
|       "id": "did:kilt:...#key-1",                                             |
|       "type": "Sr25519VerificationKey2020",                                    |
|       "controller": "did:kilt:...",                                            |
|       "publicKeyMultibase": "z6Mkm..."                                         |
|     }]                                                                         |
|   }                                                                            |
+---------------------------------------+----------------------------------------+
                                        |
         Bi-Directional Binding via pallet-did-registry (Substrate)
                                        |
                                        v
+--------------------------------------------------------------------------------+
|             Layer 1 Substrate Runtime (AccountId <-> DID Mapping)             |
|                                                                                |
|   * Tawhid of Identity: Non-Repudiation (Strict 1-to-1 Mapping)                |
|   * Ex-Ante Interception: Anonymous / Unverified Callers Deterministically     |
|     Rejected Before Extrinsic Dispatch                                         |
|   * XCMP v3/v4 Cross-Chain Attestation Query to KILT Protocol (ParaID 2086)    |
+---------------------------------------+----------------------------------------+
                                        |
                      "Hashes On-Chain, PII Off-Chain"
                                        |
                                        v
+--------------------------------------------------------------------------------+
|                     GDPR Article 5(1)(c) Data Minimization                     |
|                                                                                |
|   * Encrypted Off-Chain Store (W3C JSON-LD Manifest)                           |
|   * On-Chain State: SHA-256 JCS RFC 8785 Root Hashes & Attestation Timestamps  |
|   * Zero Personally Identifiable Information (PII) on Public Ledgers           |
+--------------------------------------------------------------------------------+
```

### 1. W3C DID Data Model v1.1 / v2.0 & eIDAS Qualified Equivalence
- **eIDAS Regulation (EU No 910/2014) Article 25:** Legal effects of electronic signatures. Qualified electronic signatures (QES) enjoy the equivalent legal effect of handwritten signatures across all 27 EU Member States.
- **KILT Protocol (ParaID 2086):** Verifiable credential issuance, delegation trees, and on-chain revocation registries enabling customs authorities to verify carrier certifications without centralized identity providers.

### 2. Bi-Directional `AccountId <-> DID` Binding (`pallet-did-registry`)
- **Tawhid of Identity (Non-Repudiation):** Eliminates identity fragmentation. A given Substrate cryptographic keypair (`sr25519` or `ed25519`) maps deterministically to an authentic economic operator DID.
- **Ex-Ante Interception:** The pallet checks the existence of an active, unrevoked DID binding in the pre-dispatch phase. Unregistered or suspended callers are halted before gas/fee consumption, eliminating Sybil vectors.

### 3. GDPR Article 5(1)(c) Compliance ("Hashes On-Chain, PII Off-Chain")
- **Data Minimization & Right to Erasure:** Under GDPR, storing company registration numbers, driver identities, or personal signatures on immutable ledgers creates non-compliance.
- **Architectural Solution:** All payload contents remain within sovereign off-chain storage. Only RFC 8785 canonical SHA-256 digests and cryptographic signatures are committed on-chain.
