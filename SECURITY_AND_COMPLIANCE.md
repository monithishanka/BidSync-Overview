# 🛡️ BidSync: Security & Compliance Governance

This document outlines the security architecture, institutional governance principles, and data protection standards embedded within the [BidSync Platform](https://bidsync.online).

---

## 📑 Security Contents

1. [Procurement Threat Model](#1-procurement-threat-model)
2. [Zero-Knowledge Sealed Bidding Architecture](#2-zero-knowledge-sealed-bidding-architecture)
3. [Tamper-Evident Audit Trail](#3-tamper-evident-audit-trail)
4. [Role-Based Access Control (RBAC)](#4-role-based-access-control-rbac)
5. [Application Defense-in-Depth](#5-application-defense-in-depth)
6. [AI Data Privacy & Integrity Standards](#6-ai-data-privacy--integrity-standards)

---

## 1. Procurement Threat Model

Procurement platforms manage high-value transactions and sensitive commercial data. BidSync is engineered to address five primary procurement risks:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   Procurement Risk Mitigation Matrix                   │
├────────────────────────────┬───────────────────────────────────────────┤
│ Threat Vector              │ Architectural Safeguard                   │
├────────────────────────────┼───────────────────────────────────────────┤
│ 1. Insider Price Leaking   │ Zero-Knowledge Envelope Encryption:       │
│    & Bid Tampering         │ Financial data is encrypted until formal  │
│                            │ automated deadline unsealing.             │
├────────────────────────────┼───────────────────────────────────────────┤
│ 2. Competitor Price        │ Commercial figures remain unreadable      │
│    Peeking                 │ during active bidding phases.             │
├────────────────────────────┼───────────────────────────────────────────┤
│ 3. Fraudulent Supplier     │ Google Gemini Multimodal AI extracts and  │
│    Registrations           │ verifies legitimate business identities.  │
├────────────────────────────┼───────────────────────────────────────────┤
│ 4. Retroactive Audit Log   │ Cryptographic hash chaining detects any   │
│    Alteration              │ unauthorized modifications mathematically.│
├────────────────────────────┼───────────────────────────────────────────┤
│ 5. Session Hijacking       │ Hardened HTTP cookies with strict policy  │
│    & Impersonation         │ enforcement and rate limiting.            │
└────────────────────────────┴───────────────────────────────────────────┘
```

---

## 2. Zero-Knowledge Sealed Bidding Architecture

### Envelope Encryption
- **Asymmetric Key Separation:** Each tender utilizes an isolated asymmetric keypair. The private key remains encrypted in escrow until the deadline.
- **Symmetric Payload Sealing:** Bid financial figures are encrypted with ephemeral symmetric keys that are securely wrapped using the tender's public key.
- **Cryptographic Receipts:** Vendors receive an immutable digital fingerprint receipt confirming their submission was registered in a tamper-evident manner.

### Guarantees
- Bids cannot be decrypted by any party prior to the closing deadline.
- All unsealing operations are conducted simultaneously via background automation without manual human intervention.

---

## 3. Tamper-Evident Audit Trail

BidSync implements a cryptographic hash-chained audit ledger:
- Every governance action records an entry linked to the cryptographic digest of the previous log.
- This creates an unbroken verification chain from the genesis entry forward.
- Platform administrators can run instantaneous mathematical verification routines to verify that no log entry has been deleted, inserted, or modified.

---

## 4. Role-Based Access Control (RBAC)

Access to platform operations is strictly partitioned across distinct user roles:

| Capability | Public / Guest | Vendor | Buyer | Platform Administrator |
|---|:---:|:---:|:---:|:---:|
| **Browse Open Tenders** | ✅ | ✅ | ✅ | ✅ |
| **Download Tender Specifications** | ❌ | ✅ | ✅ | ✅ |
| **Submit Sealed Bids** | ❌ | ✅ | ❌ | ❌ |
| **Publish Tenders & BOQ** | ❌ | ❌ | ✅ | ❌ |
| **Review Unsealed Analytics** | ❌ | ❌ | ✅ | ✅ |
| **Award Contracts** | ❌ | ❌ | ✅ | ❌ |
| **Submit Delivery Milestones** | ❌ | ✅ | ❌ | ❌ |
| **Inspect Goods & Issue GRN** | ❌ | ❌ | ✅ | ❌ |
| **File Commercial Disputes** | ❌ | ✅ | ✅ | ❌ |
| **Arbitrate & Settle Disputes** | ❌ | ❌ | ❌ | ✅ |
| **Review KYC Verifications** | ❌ | ❌ | ❌ | ✅ |
| **Verify Audit Ledger Integrity** | ❌ | ❌ | ❌ | ✅ |

---

## 5. Application Defense-in-Depth

- **HTTP Security Headers:** Integrated Helmet configuration enforcing strict CSP, frameguard protection, and transport security.
- **API Rate Limiting:** Tiered rate limiters protect authentication and sensitive endpoints from brute-force attempts and scraping.
- **Authentication Hardening:** Password hashing using standard bcrypt with salt rounds, accompanied by session storage in dedicated database stores.
- **Input Validation:** Strict type-checking, payload sanitization, and whitelist-based file upload filtering.

---

## 6. AI Data Privacy & Integrity Standards

BidSync's AI-assisted features adhere to strict institutional data governance:
- **Ephemeral Processing:** Uploaded KYC certificates are processed in memory and never stored in unsecured temporary locations.
- **Privacy Protections:** Document analysis is performed solely for entity extraction and compliance verification.
- **Schema Validation:** Strict structured output constraints prevent prompt injection vulnerabilities from malicious document contents.

---

<p align="center">
  <strong>BidSync Security & Compliance Governance</strong><br/>
  Authored by Monith Kumarasinghe • <a href="https://bidsync.online">bidsync.online</a>
</p>
