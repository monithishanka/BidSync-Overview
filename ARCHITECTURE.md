# 📐 BidSync: Technical Architecture Overview

This document presents the high-level system architecture, core design patterns, and technologies powering the [BidSync Platform](https://bidsync.online).

---

## 📑 Architecture Contents

1. [High-Level System Topology](#1-high-level-system-topology)
2. [Cryptographic Sealed Bidding Subsystem](#2-cryptographic-sealed-bidding-subsystem)
3. [Google Gemini Multimodal AI KYC Engine](#3-google-gemini-multimodal-ai-kyc-engine)
4. [Statistical Evaluation & Anomaly Detection Model](#4-statistical-evaluation--anomaly-detection-model)
5. [Real-Time WebSocket Architecture](#5-real-time-websocket-architecture)
6. [Tamper-Evident Audit Ledger](#6-tamper-evident-audit-ledger)
7. [Data Architecture & Schemas](#7-data-architecture--schemas)

---

## 1. High-Level System Topology

BidSync is architected as a modern, multi-tier web application designed for high availability, security, and responsive real-time interactions:

```mermaid
flowchart TD
    subgraph ClientTier["Client Layer"]
        UI["React 19 Single Page Application"]
        PWA["PWA Service Worker & Offline Caching"]
    end

    subgraph APITier["API Gateway & Real-Time Layer"]
        Express["Express 4 REST Services"]
        SocketServer["Socket.io WebSocket Server"]
        Security["Security Middleware & Rate Limiting"]
    end

    subgraph ServiceTier["Core Processing Engines"]
        CryptoEngine["Cryptographic Sealed Bidding Subsystem"]
        GeminiKYC["Google Gemini Multimodal AI Engine"]
        AIAnomaly["Statistical Anomaly & Scorecard Engine"]
        ContractEngine["Contract Lifecycle & GRN Engine"]
        DisputeDesk["Commercial Arbitration Desk"]
        ExportEngine["PDF & CSV Export Services"]
        CronScheduler["Automated Background Scheduler"]
    end

    subgraph PersistenceTier["Storage & Persistence Layer"]
        Mongo[("MongoDB Atlas Database Cluster")]
        HashLedger["Tamper-Evident Audit Trail"]
        CloudStorage["Cloud Vault (AWS S3 / Cloudflare R2)"]
    end

    UI <--> Express
    UI <--> SocketServer
    PWA <--> UI

    Express <--> Security
    Express <--> CryptoEngine
    Express <--> GeminiKYC
    Express <--> AIAnomaly
    Express <--> ContractEngine
    Express <--> DisputeDesk
    Express <--> ExportEngine
    CronScheduler --> CryptoEngine

    CryptoEngine <--> Mongo
    GeminiKYC <--> Mongo
    ContractEngine <--> Mongo
    DisputeDesk <--> Mongo
    Express <--> HashLedger
    Express <--> CloudStorage
```

---

## 2. Cryptographic Sealed Bidding Subsystem

To eliminate bid leakage and favoritism, BidSync implements **Zero-Knowledge Envelope Encryption**. Submitted tender bids remain cryptographically sealed until the tender closing deadline expires.

### Key Lifecycle & Sealing Flow

```mermaid
sequenceDiagram
    autonumber
    actor Buyer
    participant Server as BidSync Platform
    actor Vendor
    participant DB as Database Cluster
    participant Cron as Background Scheduler

    Note over Buyer, Server: Step 1: Tender Initialization
    Buyer->>Server: Create Sealed Tender
    Server->>Server: Generate Asymmetric Keypair (RSA-2048)
    Server->>Server: Encrypt Private Key with Master Key (AES-256-GCM)
    Server->>DB: Store Public Key & Encrypted Private Key Escrow
    Server-->>Buyer: Tender Published

    Note over Vendor, DB: Step 2: Sealed Bid Submission
    Vendor->>Server: Submit Itemized Bid Proposal
    Server->>Server: Generate Ephemeral Symmetric Key
    Server->>Server: Encrypt Financial Payload with Symmetric Key
    Server->>Server: Wrap Symmetric Key with Tender Public Key
    Server->>Server: Generate Digital Receipt Hash
    Server->>DB: Store Encrypted Proposal & Digital Receipt
    Server-->>Vendor: Return Cryptographic Bid Receipt

    Note over Cron, DB: Step 3: Automated Unsealing Ceremony
    Cron->>Cron: Monitor Deadlines Automatically
    Cron->>DB: Detect Expired Tender
    Cron->>Server: Decrypt Tender Private Key from Escrow
    loop For Every Submitted Bid
        Server->>Server: Decrypt Symmetric Key & Reveal Bid Proposal
        Server->>DB: Update Bid Status to Unsealed
    end
    Server-->>Buyer: Real-Time WebSocket Unsealing Broadcast
```

### Security Highlights
- **Zero-Knowledge Privacy:** Unsealed figures cannot be inspected in plaintext while tenders are active.
- **Automated Unsealing Ceremony:** Timed background cron workers unseal all bids simultaneously upon deadline expiration.
- **Digital Non-Repudiation Receipts:** Vendors receive cryptographic hash receipts confirming exact submission integrity.

---

## 3. Google Gemini Multimodal AI KYC Engine

BidSync integrates Google Gemini Multimodal AI models via `@google/generative-ai` to automate supplier verification and eliminate fraud:

```mermaid
flowchart TD
    A["Supplier Uploads Business Certificate\n(PDF / PNG / JPEG)"] --> B["Secure Document Vaulting\n(AWS S3 / Cloudflare R2)"]
    B --> C["Google Gemini Multimodal AI Processing"]
    
    subgraph GeminiProcessing["Google Gemini Multimodal AI"]
        C --> D["Optical Character Recognition (OCR)"]
        D --> E["Semantic Entity Extraction\n• Business Legal Name\n• Registration Number (BRN/TIN)\n• Validity & Expiry Dates"]
    end
    
    E --> F["Automated Cross-Validation Desk"]
    
    subgraph CrossValidation["Compliance Evaluation"]
        F --> G["Cross-Match Entity Information"]
        G --> H["Assess Document Authenticity"]
        H --> I["Generate Confidence Score & Recommendation"]
    end
    
    I --> J["Admin Applicant Dossier Modal\n(One-Click Verification & Badge Grant)"]
```

---

## 4. Statistical Evaluation & Anomaly Detection Model

Upon unsealing, BidSync's analytical engine processes bids through objective statistical models:

- **Predatory Dumping Detection:** Evaluates bids relative to group distributions to flag suspiciously low offers that risk project abandonment or substandard materials.
- **Budget Overrun Flagging:** Automatically detects bids exceeding client budget thresholds.
- **Collusive Pricing Alerts:** Identifies statistically identical bids across independent suppliers to protect against cartel pricing.
- **Composite Vendor Scorecard:** Balances Commercial Price, Technical Compliance, Delivery Timeline, and Verified KYC Reliability to identify the **Rank #1 Best Value** proposal.

---

## 5. Real-Time WebSocket Architecture

Powered by **Socket.io 4**, BidSync provides zero-latency updates across isolated channel rooms:

- **Tender Rooms (`tender:{id}`):** Delivers live ticking countdown clocks, anonymous bid count increments, unsealing alerts, and pre-bid clarification Q&A sync.
- **User Rooms (`user:{id}`):** Dispatches private alerts for contract awards, delivery submissions, and verification updates.
- **Automatic Reconnection:** Built-in connection lifecycle management ensures resilient synchronization across fluctuating mobile networks.

---

## 6. Tamper-Evident Audit Ledger

BidSync incorporates a blockchain-inspired SHA-256 hash-chained audit ledger:
- Each administrative and procurement action links cryptographically to the hash of the preceding record.
- Any manual database tampering or record alteration immediately breaks the hash chain.
- Administrators can execute mathematical ledger integrity checks directly from the governance dashboard.

---

## 7. Data Architecture & Schemas

The database structure in MongoDB Atlas supports the end-to-end procurement lifecycle:

```mermaid
erDiagram
    User ||--o{ Tender : "publishes (Buyer)"
    User ||--o{ Bid : "submits (Vendor)"
    User ||--o{ Contract : "participates in"
    User ||--o{ Dispute : "files"
    User ||--o{ Rating : "exchanges"
    User ||--o{ AuditLog : "initiates"

    Tender ||--o{ Bid : "receives"
    Tender ||--o| Contract : "awards to"

    Contract ||--|{ Milestone : "tracks"
    Milestone ||--o| GoodsReceivedNote : "issues upon approval"
    Contract ||--o{ Dispute : "subject to"

    AuditLog ||--|| AuditLog : "cryptographically chains to prior"
```

---

<p align="center">
  <strong>BidSync Technical Architecture Overview</strong><br/>
  Authored by Monith Kumarasinghe • <a href="https://bidsync.online">bidsync.online</a>
</p>
